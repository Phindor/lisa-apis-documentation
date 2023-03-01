# LISA API DOCUMENTATION

# AUTHENTICATION

NOTE: INITIAL PHASE OF THE AUTHENTICATION PROCESS WILL BE AUTOMATED ON THE DEVELOPER PORTAL! 

## REGISTER ON DEVELOPER PORTAL

The first step for getting started is to create a developer account on Lisa Open APIs.

```python 
[POST] {{base_uri}}/sandbox/open_api/v1/auth
```
### Expected Request

```python
{
    "name":"John Doe",
    "industry":"logistics",
    "integration_type":"mobile_app",
    "location":"kenya",
    "developer_email":"johndoe@gmail.com",
    "phone_number":"0712345678",
    "password":"1234567890"
}

```
### Expected Response

```python
{
    "success": true,
    "Lisa_response_code": 201,
    "APIresponse": "Developer successfully registered. Check email for verification OTP"
}

```
### Possible Client error
Lisa will respond with a detailed error description incase of a bad request made from the third party application
```python
{
    "success": false,
    "Lisa_response_code": 400,
    "error_description": "request body validation error",
    "APIerror": {
        "developer_email": [
            "The developer email has already been taken."
        ],
        "password": [
            "The password must be at least 10 characters."
        ]
    }
}

```
## Developer Account Verification
Developer accounts need to be verified before any subscription to any LISA APIs product is made. An email is sent to the developer email provided during registration. use the following to verify the developer email.

```python
[POST]{{base_uri}}/sandbox/open_api/v1/verify
```

### Expected Request

```python
{
    "email":"janedoe@gmail.com",
    "otp":7095
}

```
### Expected Response

```python
{
    "status": true,
    "Lisa_response_code": 200,
    "message": "Account verified. You may now login",
    "APIresponse": "Developer successfully verified. You can now log in"
}

```

### Possible Error
This may result as a result of either sending an incorrect OTP or a incorrect developer email.
```python
{
    "success": false,
    "message": "OTP does NOT Exist"
}
```

## Login

```python
[POST]{{base_uri}}/sandbox/open_api/v1/login
```

### Expected Request
```python
{
    "email":"johndoe@phindor.com",
    "password":"1234567890"
}
```
### Expected Response
```python
{
    "success": true,
    "Lisa_response_code": 200,
    "APIdescription": "Authentication successful",
    "token": "1|TWe25bziWGV9nxBKVeUUL5J7IWFuR5GCF7hWbYIS",
    "jsonData": {
        "developer_email": "johndoe@gmail.com",
        "developer_name": "John Doe",
        "developer_id":1
    }
}
```
### Possible Error

```python
{
    "success": false,
    "message": "Please ensure you have entered correct email and password"
}
```

## APP CREATION
Lisa uses an idea of "APP(S)" to wrap around all LISA products that are subscribed at any point in time. These applications represents third party system integration and therefore will include all the security credentials of any connected client. NO integration can therefore be done with no APP created. A developer can have more than 1 app and billing is done per APP.
You can get started by creating an APP:

```python
{{base_uri}}/sandbox/open_api/v1/create_apps
```
Expected Request

```python
{
    "developer_id":1,
    "app_name":"TuskysTestAccount",
    "domain":"Sandbox"
}

```

Expected Response

```python
{
    "success": true,
    "APIresponse": {
        "developer_id": 1,
        "app_name": "joy",
        "domain": "Sandbox",
        "consumer_key": "ca081d3cc21bed864a28693300cb1dd6",
        "consumer_secret": "2ee84881668ed07601eddcd190974d73b43176fb78df19bdb0fb45c774d1c3cf034ef5376a1e28d28a79c9814e66f86713c137447dafe7acfe53a20c7acb30c8",
        "updated_at": "2023-02-28T22:17:53.000000Z",
        "created_at": "2023-02-28T22:17:53.000000Z",
        "id": 1
    },
    "api_subscription_state": {
        "app_id": 1,
        "sales_prediction_api": 0,
        "customer_segmentation_api": 0,
        "profiling_api": 0,
        "personalization_api": 0
    }
}
```

Possible Errors
(a). Missing request params
```python
{
    "success": false,
    "Lisa_response_code": 400,
    "error_description": "request body validation error",
    "APIerror": {
        "developer_id": [
            "The developer id field is required."
        ],
        "app_name": [
            "The app name field is required."
        ],
        "domain": [
            "The domain field is required."
        ]
    }
}

```
(a). Invalid developer account 
```python
{
    "success": false,
    "Lisa_response_code": 404,
    "error_description": "LISA is not able to verify your developer account"
}

```
## GET APPS
```python
[GET]{{base_uri}}/sandbox/open_api/v1/apps/{developer_id}

```
Authorisation
```python
Bearer 1|TWe25bziWGV9nxBKVeUUL5J7IWFuR5GCF7hWbYIS
```
Expected Request

```python
NONE
```
Expected Response

```python
{
    "success": true,
    "APIresponse": [
        {
            "id": 1,
            "developer_id": 1,
            "app_name": "Tuskys",
            "consumer_key": "ca081d3cc21bed864a28693300cb1dd6",
            "consumer_secret": "2ee84881668ed07601eddcd190974d73b43176fb78df19bdb0fb45c774d1c3cf034ef5376a1e28d28a79c9814e66f86713c137447dafe7acfe53a20c7acb30c8",
            "expiry_seconds": "3599",
            "domain": "Sandbox",
            "created_at": "2023-02-28T22:17:53.000000Z",
            "updated_at": "2023-02-28T23:09:25.000000Z"
        }
    ]
}

```

# API PRODUCT SUBSCRIPTION
```python
[POST]{{base_uri}}/sandbox/open_api/v1/subscribe/{{app_id}}
```

Expected Request

NOTE: Current Available API is ONLY the Sales Prediction. but options are for the other API Products provided. For now test with the sales prediction api.

```python
{
    "sales_prediction_api": 1
}

```
Expected Response
```python
{
    "success": true,
    "api_response": "product subscription successful"
}

```
# ENDPOINTS AUTH
After setting up the developer accounts and having the necessary security credentials as well as application subscription for the needed API products, its time to autheticate all your endpoints, this endpoint will provide the access token that should be sent along with any request made. We are using a BASE64 encoded value of the concatenation of the consumer key and the consumer secret in that order as the secure access token to LISA APIs. this token has a time limit on production.

[GET]{{base_uri}}/sandbox/open_api/v1/generate/lisa_client_credentials

Expected Auth token
```python
Bearer BASE64ENCODEDTOKEN
```
Expected Response

```python
{
    "success": true,
    "Lisa_response_code": 200,
    "secure_access_token": "0cb8ef5fd57ae232afd1533f94d5c4a6968571408fcc0b8fe2bac271d6ad4569b9bd30dd389c9b2a88df7b18ca05b49c013f4e25a0575c10fc6f1da23c1ab5a5c179d45e12bcb6d98adbf5fc592f1fc7bbb1e6f9091b10e6aa925d819ca7f38419a3cf59495af48fcf116d99171d84ec09157107bdfaf5e7e014b1daec64f70b",
    "expires_in": 3599
}

```


