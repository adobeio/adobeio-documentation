# JWT Authentication Reference

To establish a secure service-to-service Adobe I/O API session, you must create a JSON Web Token (JWT) that encapsulates the identity of your integration, and exchange it for an access token. Every request to an Adobe service must include the access token in the Authorization header, along with the API Key (Client ID) that was generated when you created the integration in the [Adobe I/O Console](https://console.adobe.io/).

* A typical access token is valid for 24 hours after it is issued.
* You can request multiple access tokens. Previous tokens are not invalidated when a new one is issued. You can authorize requests with any valid access token. This allows you to overlap access tokens to ensure your integration is always able to connect to Adobe.

## Access request syntax

To initiate an API session,use the JWT to obtain an access token from Adobe by making a POST request to Adobe's Identity Management Service (IMS).

* Send a POST request to:

> https://ims-na1.adobelogin.com/ims/exchange/jwt

* The body of the request should contain URL-encoded parameters with your Client ID (API Key), Client Secret, and JWT:

> ```client_id={api_key_value}&client_secret={client_secret_value}&jwt_token={base64_encoded_JWT}```

## Request parameters

Pass URL-encoded parameters in the body of your POST request:

Parameter | Description
--------- | -----------
client_id | The API key generated for your integration.
client_secret | The client secret generated for your integration.
jwt_token | A base-64 encoded JSON Web Token that encapsulates the identity of your integration, signed with a private key that corresponds to a public key certificate attached to the integration.

## Responses

When a request has been understood and at least partially completed, it returns with HTTP status 200. On success, the response contains a valid access token. Pass this token in the **Authorization** header in all subsequent requests to an Adobe service.

A failed request can result in a response with an HTTP status of 400 or 401 and one of the following error messages in the response body:

Response | Description
-------- | -----------
400 invalid_client | Integration does not exist. This applies both to the **client_id** parameter and the **aud** in the JWT. The **client_id** parameter and the **aud** field in the JWT do not match.
401 invalid_client | Integration does not have the **exchange_jwt** scope. This indicates an improper client configuration. Contact Adobe I/O team to resolve it. The client ID and client secret combination is invalid.
400 invalid_token | JWT is missing or cannot be decoded. JWT has expired. In this case, the **error_description** contains more details. The exp or jti field of the JWT is not an integer.
400 invalid_signature |The JWT signature does not match any certificates attached to the integration. The signature does not match the algorithm specified in the JWT header.
400 invalid_jti | The binding requires a JTI, but the jti field is missing or was previously used.
400 invalid_scope | Indicates a problem with the requested scope for the token. The JWT must include **"https://ims-na1.adobelogin.com/s/ent_user_sdk": true**. Specific scope problems can be: Metascopes in the JWT do not match metascopes in the binding. Metascopes in the JWT do not match target client scopes. Metascopes in the JWT contain a scope or scopes that do not exist. The JWT has no metascopes.
400 bad_request | JWT payload can be decoded and decrypted but contents are incorrect. Can occur when values for fields such as **sub, iss, exp,** or **jti** are not in the proper format.

## Example

```
========================= REQUEST ==========================
POST https://ims-na1.adobelogin.com/ims/exchange/jwt
-------------------------- body ----------------------------
client_id={myClientId}&client_secret={myClientSecret}&jwt_token={myJSONWebToken}
------------------------- headers --------------------------
Content-Type: application/x-www-form-urlencoded
Cache-Control: no-cache
```

For an example of a Python script that creates and sends this type of request, see the [User Management Walkthrough](https://www.adobe.io/apis/cloudplatform/usermanagement/docs/samples.html).
