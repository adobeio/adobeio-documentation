- [Introduction](#org7a914d8)
- [Authentication and Authorization](#org8a0a008)
  - [Creating an Integration](#org56f8047)
  - [Collecting the Necessary Identifiers](#org223aea2)
  - [Request Headers](#orgf2e325f)
- [Registering a Webhook](#orgb9b3187)
  - [Application Webhooks and Authorized Users](#org5e8d2e9)
- [Event Types](#org5615df6)
  - [Retrieving a List of Event Types](#orgd349b92)
- [The Webhook Endpoint](#org7fbee92)
  - [The Challenge Request](#org00dc8fb)
  - [Webhook Event Delivery](#orge065f46)
- [The Notification Payload](#orgf96c770)
- [Signatures](#org172d24d)
- [Webhook API Endpoints](#org17976d4)
  - [POST /csm/webhooks](#orgc025636)
  - [GET /csm/webhooks/{clientId}](#org56b68bc)
  - [GET /csm/webhooks/{clientId}/{registrationId}](#org3bf2700)
  - [DELETE /csm/webhooks/{clientId}/{registrationId}](#orgaf0c78a)
  - [POST /csm/webhooks/{clientId}/{registrationId}/{status}](#org8928a9e)
- [Error Codes](#orgc147a97)
  - [400 Bad Request](#orgeb6bace)
    - [Invalid JSON](#org4a01350)
    - [Invalid web hook Request](#orgfdee48e)
    - [Invalid Event of Interest, no matching Event.](#org59cdbfa)
  - [401 Unauthorized](#org3094f60)
    - [401013 Oauth token is not valid](#org06d46c8)
  - [404 Not Found](#org721b14d)
  - [415 Unsupported Media Type](#org24e3cb1)
  - [500 Internal Server Error](#orga1bf49a)



<a id="org7a914d8"></a>

# Introduction

**Adobe I/O Events** enables building reactive, event-driven applications, based on events originating from various Adobe Services (Creative Cloud, Adobe Experience Manager, etc.)

Events are triggered by an **Event Provider**, like Adobe Creative Cloud Assets, whenever a certain real-world action occurs, such as creating a new asset.

To start listening for events your application registers a **webhook**, specifying which **Event Types** from which **Event Providers** it wants to receive. Whenever a matching event gets triggered, your application is notified through an HTTP request.

Through the Webhooks API you can create, retrieve, update, or delete webhook registrations, specifying the types of events to be notified of, as well as the URLs where notifications should be sent to.

For a gentle introduction you can follow the [Getting Started Guide](file://#). This document describes in detail how to use the Webhooks API, including general considerations like authorization and content encoding, and an overview of the specific operations that are available. It also describes the requirements that your application's webhook endpoint needs to conform to.


<a id="org8a0a008"></a>

# Authentication and Authorization


<a id="org56f8047"></a>

## Creating an Integration

To access the Webhooks API you first need to create a new **Integration** (also referred to as an "application") in the [Adobe I/O Console](https://console.adobe.io/).

See the [Creative SDK Getting Started Guide](https://creativesdk.adobe.com/docs/web/#/articles/gettingstarted/index.html), in particular the section "Registering Your Application".


<a id="org223aea2"></a>

## Collecting the Necessary Identifiers

Once you have set up the integration, you will find four pieces of identifying information

-   **Consumer ID**: You will find two numbers of five or more digits in the URL of the integration's overview page. The first is your consumer ID. It will be the same for all integrations you create.
-   **Application ID**: The second number in the URL is the integration's (application's) unique ID.
-   **Client ID**: This hexadecimal code is needed to communicate with the Creative SDK, in particular you will need this to authenticate users. You also need to use this as part of the request when calling the Webhooks API. Older documentation may refer to this value as the "API Key"
-   **Client Secret**: You will use this hexadecimal code to validate incoming webhook requests, to make sure they are actually coming from Adobe I/O Events, and not from an untrusted actor. Keep this value secret.

Using the [Creative SDK User Auth UI](https://creativesdk.adobe.com/docs/web/#/articles/userauthui/index.html) for your platform (iOS, Android, Web), you can let users log in to your application using their Adobe ID. This way a user **authorizes** your application to make API requests on their behalf, and to be notified of events "owned" by this user.

Upon succesful authorization you will get a **token** back from the Creative SDK (also known as a **OAuth2 Token** or **Bearer Token**).

For development and testing you can instead quickly retrieve a token linked to your Adobe ID by opening the [Adobe I/O Console](https://console.adobe.io/), opening the JavaScript console (F12), and entering `adobeIMS.getAccessToken()`.


<a id="orgf2e325f"></a>

## Request Headers

With these pieces in hand you can now set up the necessary HTTP headers to make authenticated requests to the API. These headers must be present on each request:

```restclient
Authorization: Bearer <Your Token>
x-ams-consumer-id: <Your Consumer ID>
x-ams-application-id: <Your Application ID>
```

POST or PUT requests that contain a request body must also carry a `Content-Type` header with a value of `application/json`.

You will also have to pass the **Client ID** along with certain Webhooks API request, either as part of the URL, or as part of the request body. Consult the documentation of individual API calls for details.


<a id="orgb9b3187"></a>

# Registering a Webhook

You can register a webhook through the [Adobe I/O Console](https://console.adobe.io/) web interface, or through the Webhooks API, by POSTing a Registration JSON object.

The `"client_id"`, `"name"`, `"description"`, and `"webhook_url"` are required, as well as a list of `"events_of_interest"`, containing the events and event provider to listen to.

How to find your `"client_id"` is described in the section on [Authentication and Authorization](#org8a0a008). `"name"` and `"description"` can be chosen freely.

The `"webhook_url"` is the URL where events will be delivered. How to set this up and handle incoming events is described in the [Webhook Endpoint](#org7fbee92) section.

If the registration is successful, then you will get the same Registration object back as a response, but with several fields added.

-   `"registration_id"` A unique identifier for this registration. Use this to query, update, or delete the registration later on.
-   `"status"` If the registration was successful, then this will equal `"VERIFIED"`. If instead the status is `"VERIFICATION_FAILED"`, then make sure your webhook is responding correctly to the challenge request. Refer to the [The Webhook Endpoint](#org7fbee92) section for details.

```restclient
POST https://csm.adobe.io/csm/webhooks
Authorization: Bearer OAUTH2_TOKEN
x-ams-consumer-id: CONSUMER_ID
x-ams-application-id: APPLICATION_ID
Content-Type: application/json

{
  "client_id": "CLIENT_ID",
  "name": "WEBHOOK_NAME",
  "description": ":WEBHOOK_DESCRIPTION",
  "webhook_url": "WEBHOOK_URL",
  "events_of_interest": [
    {"provider": "PROVIDER", "event_code": "EVENT_TYPE"},
    {"provider": "PROVIDER", "event_code": "EVENT_TYPE"}
  ]
}
```


<a id="org5e8d2e9"></a>

## Application Webhooks and Authorized Users

Once a webhook is registered, your application will receive events for **all authorized users**. As soon as a users authorizes your application (using the Creative SDK Auth UI), you will start receiving events for that user.

In other words, you only need to register for a certain event type once for the whole application, and not for each individual user.


<a id="org5615df6"></a>

# Event Types

When registering a webhook, you pass in a list of events you want to be notified of. Each event in the list is specified by an Event Provider, and an Event Code.

The exact events that are available to you depends on the Event Providers your organization has access to, and may change over time. These are the events provided by Adobe Creative Cloud Assets.

| Provider  | Event Code     | Description                  | Type  |
|--------- |-------------- |---------------------------- |----- |
| ccstorage | asset\_created | Creative Cloud Asset Created | asset |
| ccstorage | asset\_updated | Creative Cloud Asset Updated | asset |
| ccstorage | asset\_deleted | Creative Cloud Asset Deleted | asset |

For example, to be notified when assets are created and updated, but not when they are deleted, the Registration object would contain this list of `"events_of_interest"`:

```json
{
  ...
  "events_of_interest": [
    {"provider": "ccstorage", "event_code": "asset_created"},
    {"provider": "ccstorage", "event_code": "asset_updated"}
  ]
}
```


<a id="orgd349b92"></a>

## Retrieving a List of Event Types

You can get a list of all the events available to your organization by querying the API.

Events are part of an Event Provider, which are in turn organized in Event Provider Groups.

**Request**

```restclient
GET https://csm.adobe.io/csm/events/metadata
Authorization: Bearer <Your Token>
x-ams-consumer-id: <Your Consumer ID>
x-ams-application-id: <Your Application ID>
```

**Sample Response Body**

```json
[
  {
    "grouping": "Creative Cloud",
    "label": "Creative Cloud",
    "enabled": true,
    "providers": [
      {
        "provider": "ccstorage",
        "label": "Creative Cloud Assets",
        "enabled": true,
        "type": "adobeid",
        "organization_id": "ADOBEID",
        "events": [
          {
            "provider": "ccstorage",
            "description": "Creative Cloud Asset Deleted",
            "label": "Creative Cloud Asset Deleted",
            "topicName": "ccstorage_asset_deleted",
            "event_code": "asset_deleted",
            "event_type": "asset"
          },
          {
            "provider": "ccstorage",
            "description": "Creative Cloud Asset Created",
            "label": "Creative Cloud Asset Created",
            "topicName": "ccstorage_asset_created",
            "event_code": "asset_created",
            "event_type": "asset"
          },
          {
            "provider": "ccstorage",
            "description": "Creative Cloud Asset Updated",
            "label": "Creative Cloud Asset Updated",
            "topicName": "ccstorage_asset_updated",
            "event_code": "asset_updated",
            "event_type": "asset"
          }
        ]
      }
    ]
  }
]
```


<a id="org7fbee92"></a>

# The Webhook Endpoint

When registering a webhook, you pass in a URL where events need to be delivered. For example, if your application lives at [<https://example.com>](https://example.com) then you could have a [<https://example.com/adobe_events>](https://example.com/adobe_events) endpoint that is responsible for handling incoming events.

This URL must be accessible over HTTPS (encrypted HTTP) on the standard HTTPS port 443, and it must be able to handle both GET and POST requests.

The SSL certificate must be valid: this means it is not allowed to be self-signed, and it should not have expired.


<a id="org00dc8fb"></a>

## The Challenge Request

When registering a webhook, Adobe I/O Events will make a pre-flight request to the webhook URL to verify that it is correctly set up. This is a HTTP GET request, with a `challenge` query parameter. The application must respond by echoing back this `challenge`.

**Challenge Request**

```restclient
GET https://example.com/adobe_events?challenge=6d9445-f41025fed66-332ef104f446
```

**Response**

You can either respond by placing the challenge value directly in the response body

```restclient
HTTP/1.1 200 OK

6d9445-f41025fed66-332ef104f446
```

or by responding with a JSON object with a `"challenge"`. Make sure to include the correct `Content-Type` header.

```restclient
HTTP/1.1 200 OK
Content-type: application/json

{"challenge":"6d9445-f41025fed66-332ef104f446"}
```

If this exchange is successful, then the webhook is successfully registered, and its state is set to `VERIFIED`, if not it is set to `VERIFICATION_FAILED`.


<a id="orge065f46"></a>

## Webhook Event Delivery

When events are triggered for which you have registered a webhook, then Adobe I/O Events will perform a HTTP POST request to the webhook URL, with as request body the JSON encoded Event object.

**Webhook Request**

```restclient
POST https://example.com/adobe_events
x-adobe-signature: 5gme7hUx+DedKsfGMiCpiVZubv9neXyIGv1JNYbc5IDmf4KYd6jE5HEQpAIuo/N83tLp9jrNwoP0Brrk1ltEXA==

{"id":"f9218f73-feaa-425f-866d-4940b77fb7d4",
 "category":"asset",
 "source":"creative-cloud",
 "asset":{
   "type":"asset_created",
   "url":"https://cc-api-storage.adobe.io/id/urn:aaid:sc:eu:f7f4c7b9-9216-4f23-a41e-185cc2e1e2b3",
   "filename":"cat.png",
   "pathname":"/files/cat.png",
   "asset_id":"urn:aaid:sc:eu:f7f4c7b9-9216-4f23-a41e-185cc2e1e2b3",
   "user_id":"3CEFA11A5901B53B0A495CC2@AdobeID",
   "mime_type":"image/png"
 },
 "created_at":"2017-05-08T17:34:59.683Z"}
```

If your application managed to successfully process the event, it should respond with a HTTP status code in the 2XX range . If the status code is in the 5XX range (server error), then the delivery is considered to have failed.

If no response is received within 30 seconds, then the request times out and delivery is also considered to have failed. If you need to perform lengthy operations in response to an incoming event, then consider doing them outside the HTTP handler, to make sure the delivery doesn't time out.

If webhook delivery fails then the request will be retried later on. Retries will continue with increasing intervals, until eventually giving up and changing the status of the webhook registration from `VERIFIED` to `HOOK_UNREACHABLE`.

Currently 5 retries are attempted, with intervals starting at 8 seconds, and increasing 1.5 times for each successive retry. This may change in the future.


<a id="orgf96c770"></a>

# The Notification Payload

A POST request to your webhook will have a JSON Event object as request body. It contains event metadata, and a nested object with event data.

Metadata includes

-   `"id"` a unique identifier for this event
-   `"source"` the event provider that triggered the event
-   `"created_at"` the time the event occured represented as an ISO 8601 timestamp

The event data available depends on the type of event. Adobe Creative Cloud Assets events all contain a `"asset"`, with location, owner, and asset identifier, as well as a `"type"` field containing the event code.

```json
{
    "id":"f9218f73-feaa-425f-866d-4940b77fb7d4",
    "category":"asset",
    "source":"creative-cloud",
    "asset":{
      "type":"asset_created",
      "url":"https://cc-api-storage.adobe.io/id/urn:aaid:sc:eu:f7f4c7b9-9216-4f23-a41e-185cc2e1e2b3",
      "filename":"cat.png",
      "pathname":"/files/cat.png",
      "asset_id":"urn:aaid:sc:eu:f7f4c7b9-9216-4f23-a41e-185cc2e1e2b3",
      "user_id":"3CEFA11A5901B53B0A495CC2@AdobeID",
      "mime_type":"image/png"
    },
    "created_at":"2017-05-08T17:34:59.683Z"
  }
```


<a id="org172d24d"></a>

# Signatures

Your webhook URL must by necessity be accessible from the open internet. This means third party actors can send forged requests to it, tricking your application into handling fake events.

To prevent this from happening, Adobe I/O Events will add a `x-adobe-signature` header to each POST request it does to your webhook URL, which allows you to verify that the request was really made by Adobe I/O Events.

This signature or "message authentication code" is computed using a cryptographic hash function and a secret key, applied to the body of the HTTP request.

In particular a SHA256 HMAC is computed of the JSON payload, using the **Client Secret** as a secret key, and then turned into a Base64 digest.

Upon receiving a request, you should repeat this calculation and compare the result to the value in the `x-adobe-signature` header, and reject the request unless they match. Since the **Client Secret** is only known by you and Adobe I/O Events, this is a reliable way to verify the authenticity of the request.

**HMAC check implementation in JavaScript (pseudo-code)**

```javascript
var crypto = require('crypto')
const hmac = crypto.createHmac('sha256', ADOBE_CLIENT_SECRET)
hmac.update(raw_request_body)

if (request.header('x-adobe-signature') !== hmac.digest('base64')) {
  throw new Error('x-adobe-signature HMAC check failed')
}
```


<a id="org17976d4"></a>

# Webhook API Endpoints

| Operation                                                 | Response              | Description                                         |
|--------------------------------------------------------- |--------------------- |--------------------------------------------------- |
| `POST /csm/webhooks`                                      | Registration          | Creates a Client Application's WebHook registration |
| `GET /csm/webhooks/{clientId}`                            | Array of Registration | Get all Webhook registration details                |
| `GET /csm/webhooks/{clientId}/{registrationId}`           | Registration          | Get a specific WebHook registration details         |
| `DELETE /csm/webhooks/{clientId}/{registrationId}`        | (empty)               | Deletes webhook registration                        |
| `POST /csm/webhooks/{clientId}/{registrationId}/{status}` | Registration          | Enables/disables a webhook registration             |

In all these calls `clientId` is the Client ID that identifies the Integration, as found in the Adobe I/O Console.

`registrationId` is a UUID representing a single webhook registration.

`status` can be either `ENABLED` or `DISABLED`.


<a id="orgc025636"></a>

## POST /csm/webhooks

Creates a new webhook registration. The request payload is the Registration object. See [Registering a Webhook](#orgb9b3187).


<a id="org56b68bc"></a>

## GET /csm/webhooks/{clientId}

Get a list of all existing webhook registrations. The response body is a JSON array of Registration objects.


<a id="org3bf2700"></a>

## GET /csm/webhooks/{clientId}/{registrationId}

Retrieve a single webhook Registraion object based on its UUID. The response body is the Registration object.


<a id="orgaf0c78a"></a>

## DELETE /csm/webhooks/{clientId}/{registrationId}

Delete a Registraion object based on its UUID. This returns an empty response body with a status code of 204 (No Content)


<a id="org8928a9e"></a>

## POST /csm/webhooks/{clientId}/{registrationId}/{status}

With this endpoint you can temporarily disable or re-enable a registration, thus halting or resuming the delivery of events. The value of `status` should be either `ENABLED` or `DISABLED`. Any value other than `ENABLED` or `DISABLED` will result in a response of 404 Not Found.

The response body is the updated Registration object. The current status (enabled or disabled) can be found in its `"integration_status"` field.


<a id="orgc147a97"></a>

# Error Codes

This is a partial list of possible error codes, including an indication of what might cause them. Unsuccessful API requests will return a HTTP status code in the 4XX range (client error) or in the 5XX range (server error).


<a id="orgeb6bace"></a>

## 400 Bad Request

This is a generic response indicating that the API request you made is in some way invalid. The response message might contain an indication of what is wrong. Carefully check that all necessary headers are present, and that any JSON payload contains all required fields and is free of syntax errors.

```json
HTTP/1.1 400 Bad Request

may not be null (path = CSMResource.createUserWebhookRegistration.arg1.description, invalidValue = null)
```


<a id="org4a01350"></a>

### Invalid JSON

The JSON payload is syntactically invalid. Make sure the request body conforms to the JSON specification.

```json
HTTP/1.1 400 Bad Request

Unexpected character ('"' (code 34)): was expecting comma to separate Object entries
at [Source: org.glassfish.jersey.message.internal.ReaderInterceptorExecutor$UnCloseableInputStream@7d48c717; line: 5, column: 3]
```


<a id="orgfdee48e"></a>

### Invalid web hook Request

Make sure the given webhook\_url is in fact a valid URL.

```json
HTTP/1.1 400 Bad Request

{
  "reason": "Invalid web hook Request id: 1YaXyhR0VXv4j2ezb0CeP3Aq9Ql1hp3l.",
  "message": "Webhook is invalid"
}
```


<a id="org59cdbfa"></a>

### Invalid Event of Interest, no matching Event.

The given `event_code` was not recognized. See [Retrieving a List of Event Types](#orgd349b92) to fetch a list of valid event codes.

```
HTTP/1.1 400 Bad Request

{
  "reason": "Invalid Event of Interest, no matching Event. Request id: PKWu4CeFYo3R7hXg1epAPlZ2T0cMgVi3.",
  "message": "Bad Request"
}
```


<a id="org3094f60"></a>

## 401 Unauthorized


<a id="org06d46c8"></a>

### 401013 Oauth token is not valid

Triggered when the `Authorization` header is missing or invalid, or if a valid header is present but the OAuth token has expired.

```json
HTTP/1.1 401 Unauthorized
Content-Type: application/json

{
  "error_code": "401013",
  "message": "Oauth token is not valid"
}
```


<a id="org721b14d"></a>

## 404 Not Found

The resource designated by the URL does not exist. This happens for instance when doing a `GET` or `DELETE` for a certain webhook registration, but the `registrationId` in the URL does not correspond with any existing registrations.

```
HTTP/1.1 404 Not Found

{
  "reason": "HTTP 404 Not Found",
  "message": "Application error"
}
```


<a id="org24e3cb1"></a>

## 415 Unsupported Media Type

Triggered when the `Content-Type` header is missing or invalid. For requests that include a request body, this should be set to `Content-Type: application/json; charset=UTF-8`

```json
HTTP/1.1 415 Unsupported Media Type

{
  "reason": "HTTP 415 Unsupported Media Type Request id: h6mfgf2lPZSb1RaqIrDvXxrICj2T7ANF.",
  "message": "Application error"
}
```


<a id="orga1bf49a"></a>

## 500 Internal Server Error

This usually means an implementation error on the side of Adobe I/O Events. Please file a support request.

```json
{
  "reason": "There was an error processing your request. Request id: gdkhJeSutvjC0PzC0Rg3jir43BH3grjd.",
  "message": "System error"
}
```
