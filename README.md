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
        "developer_name": "John Doe"
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





