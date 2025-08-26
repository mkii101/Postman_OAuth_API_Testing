# OAuth Project - Postman API Collection

This project contains a Postman collection for testing OAuth 2.0 authentication and API requests using Google's OAuth endpoints and a sample resource server. The collection demonstrates the full OAuth flow: obtaining an authorization code, exchanging it for an access token, and using the token to access protected resources.

---

## Table of Contents

- [Overview](#overview)
- [Collection Structure](#collection-structure)
- [OAuth 2.0 Flow](#oauth-20-flow)
- [Requests](#requests)
  - [getCode](#getcode)
  - [ExchangeCode](#exchangecode)
  - [ActualRequest](#actualrequest)
  - [random](#random)
- [Variables](#variables)
- [How to Use](#how-to-use)
- [Test Scripts & Automation](#test-scripts--automation)
- [Tips](#tips)

---

## Overview

This collection demonstrates how to:

- Initiate the OAuth 2.0 Authorization Code flow with Google.
- Exchange the authorization code for an access token.
- Use the access token to access a protected API endpoint.
- Validate API responses using Postman test scripts.

---

## Collection Structure

- **getCode**: Initiates the OAuth 2.0 authorization request.
- **ExchangeCode**: Exchanges the authorization code for an access token.
- **ActualRequest**: Uses the access token to access a protected resource and validates the response.
- **random**: Example request to the resource server with different parameters.

---

## OAuth 2.0 Flow

1. **Authorization Request**:  
   The `getCode` request opens the Google OAuth consent screen for the user to authorize access.

2. **Authorization Code**:  
   After consent, Google redirects to the specified `redirect_uri` with a `code` parameter.

3. **Token Exchange**:  
   The `ExchangeCode` request exchanges the authorization code for an access token.

4. **API Access**:  
   The `ActualRequest` uses the access token to access protected resources.

---

## Requests

### getCode

- **Method**: GET  
- **Endpoint**:  
  ```
  https://accounts.google.com/o/oauth2/v2/auth
  ```
- **Query Parameters**:
  - `scope`: https://www.googleapis.com/auth/userinfo.email
  - `auth_url`: https://accounts.google.com/o/oauth2/v2/auth
  - `client_id`: 692183103107-p0m7ent2hk7suguv4vq22hjcfhcr43pj.apps.googleusercontent.com
  - `response_type`: code
  - `redirect_uri`: https://rahulshettyacademy.com/getCourse.php

**Purpose**:  
Initiates the OAuth flow. Open this URL in a browser, log in, and authorize. Copy the `code` from the redirect URL for the next step.

---

### ExchangeCode

- **Method**: POST  
- **Endpoint**:  
  ```
  https://www.googleapis.com/oauth2/v4/token
  ```
- **Query Parameters**:
  - `code`: (Paste the code from previous step)
  - `client_id`: 692183103107-p0m7ent2hk7suguv4vq22hjcfhcr43pj.apps.googleusercontent.com
  - `client_secret`: erZOWM9g3UtwNRj340YYaK_W
  - `redirect_uri`: https://rahulshettyacademy.com/getCourse.php
  - `grant_type`: authorization_code

**Purpose**:  
Exchanges the authorization code for an access token. The access token is saved as a global variable for use in subsequent requests.

---

### ActualRequest

- **Method**: GET  
- **Endpoint**:  
  ```
  https://rahulshettyacademy.com/getCourse.php
  ```
- **Query Parameters**:
  - `code`: (authorization code or access token)
  - `scope`, `authuser`, `prompt`: as required

- **Auth**: OAuth2 (access token is used)

**Purpose**:  
Accesses a protected resource using the access token. The response is validated using Postman test scripts.

---

### random

- **Method**: GET  
- **Endpoint**:  
  ```
  https://rahulshettyacademy.com/getCourse.php
  ```
- **Query Parameters**:  
  Similar to `ActualRequest`, but with different code values.

---

## Variables

- `code`: The authorization code obtained from the OAuth consent screen.
- `access_token`: Set automatically after the `ExchangeCode` request.

---

## How to Use

1. **Import the Collection**  
   - In Postman, click `Import` and select `OAuth Project.postman_collection.json`.

2. **Run the OAuth Flow**  
   - Send the `getCode` request. Copy the `code` from the redirected URL.
   - Set the `code` variable in Postman (Globals or Environment).
   - Send the `ExchangeCode` request to obtain the access token.
   - The access token is saved automatically.

3. **Access Protected Resource**  
   - Send the `ActualRequest` to access the API using the access token.
   - Review the test results in the Postman Test Results tab.

---

## Test Scripts & Automation

- **ExchangeCode**:  
  Saves the `access_token` to Postman globals for use in subsequent requests.

- **ActualRequest**:  
  - Verifies the presence and structure of specific courses in the response.
  - Checks that the sum of API course prices equals 90.
  - Validates that the list of web course titles matches the expected array.

---

## Tips

- **Debugging**: Use `console.log` in test scripts to inspect variables and responses.
- **Token Expiry**: If the access token expires, repeat the OAuth flow to obtain a new code and token.
- **Security**: Do not share your client secret or access tokens publicly.

---

## References

- [Google OAuth 2.0 Documentation](https://developers.google.com/identity/protocols/oauth2)
- [Postman OAuth 2.0 Guide](https://learning.postman.com/docs/sending-