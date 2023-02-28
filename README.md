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



