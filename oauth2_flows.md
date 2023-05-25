# OAuth2 Flows

These are the URLs for each of the OAuth2 flows described in the slides.

## Authorization Code Flow

Client to Authorization Server:
```
GET http://auth-server.com/authorize?response_type=code&client_id=<CLIENT_ID>&redirect_uri=<REDIRECT_URI>&scope=<SCOPE>&state=<RANDOM_STRING>
```

Authorization Server to Client:
```
GET https://<REDIRECT_URI>?code=<AUTH_CODE>&state=<STATE>
```

Client to Authorization Server:
```
POST https://auth-server.com/token

   grant_type=authorization_code
   &code=<AUTH_CODE>
   &redirect_uri=<REDIRECT_URI>
   &client_id=<CLIENT_ID>
   &client_secret=<CLIENT_SECRET>
```

Authorization Server to Client:
```
{
    "access_token": "<ACCESS_TOKEN>", 
    "refresh_token": "<REFRESH_TOKEN>", 
    "token_type": "Bearer", 
    "expires_in": <SECONDS>
}
```

Client to Resource Server:
```
GET https://res-server.com/resource

  Authorization: Bearer <ACCESS_TOKEN>
```

## Implicit Flow

Client to Authorzation Server:
```
GET http://auth-server.com/authorize?response_type=token&client_id=<CLIENT_ID>&redirect_uri=<REDIRECT_URI>&scope=<SCOPE>&state=<RANDOM_STRING>
```

Authorization Server to Client:
```
GET https://<REDIRECT_URI>#token=<ACCESS_TOKEN>&state=<STATE>&token_type=Bearer&expires_in=<SECONDS>
```

Client to Resource Server:
```
GET https://res-server.com/resource

  Authorization: Bearer <ACCESS_TOKEN>
```

## Authorization Code Flow with PKCE

Client to Authorization Server:
```
GET http://auth-server.com/authorize?response_type=code&client_id=<CLIENT_ID>&redirect_uri=<REDIRECT_URI>&scope=<SCOPE>&state=<RANDOM_STRING>&code_clallenge=<HASHED_CHALLENGE>&code_challenge_method=S256
```

Authorization Server to Client:
```
GET https://<REDIRECT_URI>?code=<AUTH_CODE>&state=<STATE>
```

Client to Authorization Server:
```
POST https://auth-server.com/token

   grant_type=authorization_code&code=<AUTH_CODE>
   &redirect_uri=<REDIRECT_URI>
   &client_id=<CLIENT_ID>
   &client_secret=<CLIENT_SECRET>
   &code_verifier=<UNHASHED_CODE>
```

Authorization Server to Client:
```
{
    "access_token": "<ACCESS_TOKEN>", 
    "refresh_token": "<REFRESH_TOKEN>", 
    "token_type": "Bearer", 
    "expires_in": <SECONDS>
}
```

Client to Resource Server:
```
GET https://res-server.com/resource

  Authorization: Bearer <ACCESS_TOKEN>
```

## Client Credentials Flow

Client to Authorzation Server:
```
POST https://auth-server.com/token

   grant_type=client_credentials
   &client_id=<CLIENT_ID>
   &client_secret=<CLIENT_SECRET>
   &state=<RANDOM_STRING>
```

Authorization Server to Client:
```
{
    "access_token": "<ACCESS_TOKEN>", 
    "refresh_token": "<REFRESH_TOKEN>", 
    "token_type": "Bearer", 
    "expires_in": <SECONDS>
}
```

Client to Resource Server:
```
GET https://res-server.com/resource

  Authorization: Bearer <ACCESS_TOKEN>
```

## Password Flow

Client to Authorization Server:
```
POST https://auth-server.com/token

   grant_type=password
   &client_id=<CLIENT_ID>
   &username=<USERNAME>
   &password=<PASSWORD>
```

Authorization Server to Client:
```
{
    "access_token": "<ACCESS_TOKEN>", 
    "refresh_token": "<REFRESH_TOKEN>", 
    "token_type": "Bearer", 
    "expires_in": <SECONDS>
}
```

Client to Resource Server:
```
GET https://res-server.com/resource

  Authorization: Bearer <ACCESS_TOKEN>
```

## Refresh Token Flow

Client to Authorization Server:
```
POST https://auth-server.com/token

   grant_type=refresh_token
   &client_id=<CLIENT_ID>
   &client_secret=<CLIENT_SECRET>
   &refresh_token=<REFRESH_TOKEN>
```

Authorization Server to Client:
```
{
    "access_token": "<ACCESS_TOKEN>", 
    "refresh_token": "<REFRESH_TOKEN>", 
    "token_type": "Bearer", 
    "expires_in": <SECONDS>
}
```

## Device Flow

Client to Authorization Server
```
POST https://auth-server.com/token

   response_type=device_code
   &client_id=<CLIENT_ID>
```

Authorization Server to Client:
```
{
    "device_code": "<DEVICE_CODE>",
    "user_code": "<USER_CODE>",
    "verification_uri": "https://auth-server.com/device",
    "interval": <SECONDS>,
    "expires_in": <SECONDS>
}
```

Client to Authorization Server (polling):
```
POST https://auth-server.com/token

  grant_type=device&client_id=<CLIENT_ID>
  &device_code=<DEVICE_CODE>
```

Authorization Server to Client:
```
{"error": "authorization_pending"}
```

Second device to Authorization Server:
```
POST https://auth-server.com/device

   user_code=<USER_CODE>
```

Client to Authorization Server (still polling):
```
POST https://auth-server.com/token 

  grant_type=device&client_id=<CLIENT_ID>
  &device_code=<DEVICE_CODE>
```

Authorization Server to Client:
```
{
    "access_token": "<ACCESS_TOKEN>",
    "refresh_token": "<REFRESH_TOKEN>",
    "state": "create",
    "token_type": "Bearer",
    "expires_in": <SECONDS>
}
```
