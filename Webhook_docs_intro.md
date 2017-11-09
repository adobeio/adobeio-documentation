# Adobe I/O Events Webhooks

- [Introduction](#org3786b01)
- [Concepts](#org81068e4)
  - [An example:](#org07fb732)
- [Your first webhook](#orgbb36f22)
  - [The Challenge Request](#orgec22b7a)
  - [Using webscript.io](#org1762841)
- [Create an Integration](#org926a538)
- [Registering the Webhook](#orgef08b06)
- [Receiving Events](#orgecb4ae5)
- [Using the API](#orgd004238)
  - [Authentication](#org649c409)
  - [Creating the webhook](#org226a51a)
    - [Response](#org85f36da)
  - [Checking that it worked](#org1a8bf8b)
    - [Response](#org5cc962d)



<a id="org3786b01"></a>

## Introduction

With Adobe I/O Events webhooks, your application can sign up to be notified whenever certain events occur. For example, when a user uploads a file to Adobe Creative Cloud Assets, this action generates an event. With the right webhook in place, your application is instantly notified that this event happened.

To start receiving events, you register a webhook, specifying a webhook URL and the types of events you want to receive. Each event will result in a HTTP request to the given URL, notifying your application.


<a id="org81068e4"></a>

## Concepts

An **Event** is a JSON object that describes something that happened.

Events originate from **Event Providers**. Each event provider publishes specific types of events, identified by an **Event Code**.

A **Webhook URL** receives event JSON objects as HTTP POST requests.

You start receiving events by creating a **Webhook Registration**, providing a name, description, webhook URL, and a list of event types you are interested in.


<a id="org07fb732"></a>

### An example:

Acme Inc. wants to be notified when a new file is uploaded to Adobe Creative Cloud Assets, so it creates the following webhook registration:

```json
{
  "name": "Acme Webhook",
  "description": "Listen for newly created files",
  "webhook_url": "https://acme.example.com/webhook",
  "events_of_interest": [
    {"provider": "ccstorage", "event_code": "asset_created"}
  ]
}
```

Now when a file is uploaded, Adobe I/O events performs the following HTTP request

```json
POST https://acme.example.com/webhook HTTP/1.1
Content-type: application/json

{
  "id":"f9218f73-feaa-425f-11111-4940b77fb7d4",
  "category":"asset",
  "source":"creative-cloud",
  "asset":{
    "type":"asset_created",
    "url":"https://cc-api-storage.adobe.io/id/urn:aaid:sc:eu:f7f4c7b9-9216-4f23-a41e-111111111111",
    "filename":"cat.png",
    "pathname":"/files/cat.png",
    "asset_id":"urn:aaid:sc:eu:f7f4c7b9-9216-4f23-a41e-185cc2e1e2b3",
    "user_id":"ABC123@AdobeID",
    "mime_type":"image/png"
  },
  "created_at":"2017-05-08T17:34:59.683Z"
}
```


<a id="orgbb36f22"></a>

## Your first webhook

Before you can register a webhook, the webhook needs to be online and operational. If not then the registration will fail. So you need to take care of setting that up first.

Your webhook needs to

-   be accessible from the internet (localhost won't work)
-   be reachable over HTTPS
-   correctly respond to a "challenge" request


<a id="orgec22b7a"></a>

### The Challenge Request

When registering a webhook, Adobe I/O Events will first try to verify that the URL is valid. To do this it sends a HTTP GET request, with a `challenge` query parameter. The webhook should respond with a body containing the value of the `challenge` query parameter.

```
GET https://acme.example.com?challenge=8ec8d794-e0ab-42df-9017-e3dada8e84f7
```

#### Response

You can either respond by placing the challenge value directly in the response body

```restclient
HTTP/1.1 200 OK

8ec8d794-e0ab-42df-9017-e3dada8e84f7
```

or by responding with a JSON object, including the correct `Content-Type` header

```restclient
HTTP/1.1 200 OK
Content-type: application/json

{"challenge":"8ec8d794-e0ab-42df-9017-e3dada8e84f7"}
```
**Note:** Make sure your response is given in the correct content-type. If your webhook script places the challenge value directly in the response body, make sure it's returned as plain text (`text/plain`). For a JSON response, make sure it's `application/json`. Returning a response in the incorrect content-type may cause extraneous code to be returned, which will not validate with Adobe I/O Events.

<a id="org1762841"></a>

### Testing with ngrok
[Ngrok](https://ngrok.com/) is a utility for enabling secure introspectable tunnels to your localhost. With ngrok, you can securely expose a local web server to the internet and run your own personal web services from your own machine, safely encrypted behind your local NAT or firewall. With ngrok, you can iterate quickly without redeploying your app or affecting your customers. 

Among other things, ngrok is a great tool for testing webhooks. Once you've downloaded and installed [ngrok](https://ngrok.com/), you run it from a command line, specifying the protocol and port you want to monitor:
```ngrok http 80```

![ngrok on port 80](img/ngrok.png "ngrok on port 80")

In the ngrok UI, you can see the URL for viewing the ngrok logs, labeled "Web Interface", plus the public-facing URLs ngrok generates to forward HTTP and HTTPS traffic to your localhost. You can use either of those public-facing URLs to register your Webhook with Adobe I/O, so long as your application is configured to respond on your localhost accordingly. Once your testing phase is complete, you would then replace the ngrok URL in your Adobe I/O integration with the public URL for your deployed app.

<a id="org926a538"></a>

## Create an Integration

To create an integration:

1. Open the [Adobe I/O Console](http://console.adobe.io/); by default, you should be on the Integations panel. If not, select it from the top menu.  
  
  ![Adobe I/O Console](img/Console_1.png "Adobe I/O Console")  
  
2. Select [New integration](https://console.adobe.io/integrations/new). You have the choice between "Access An API" or "Receive Realtime Events". Choose "Access an API" and select "Continue".  
  
  ![Create a new integration](img/Console_2.png "Create a new integration")  
  
3. Now you're given the choice of Adobe service for your integration. Choose "Creative SDK" and select "Continue".  
  
  ![Choosing Creative SDK](img/Console_3.png "Choosing Creative SDK")  
  
4. Now you can choose between creating a brand new integration, or updating an existing one. Create a new integration.  
  
  ![A brand new integration](img/Console_4.png "A brand new integration")  
  
5. Enter a name and description for the integration. As a platform, choose "Web". You also need to specifiy a Redirect URL and Redirect URL pattern. These are only relevant when using the [Creative Cloud SDK User Auth UI](https://creativesdk.adobe.com/docs/web/#/articles/userauthui/index.html). For now you can fill in `https://example.com` and `https://example\.com/.*` respectively. Select "Create Integration".  
  
  ![Completing the integration](img/Console_5.png "Completing the integration")  

6. You'll see an acknowledgement that your integration has been created. Select "Continue to Integration details" to open the integration overview.  
  
  ![Go to integration details](img/Console_6.png "Go to integration details")  

<a id="orgef08b06"></a>

## Registering the Webhook

Now that you've created the integration, you need to register your webhook. 

1. On the integration overview page, select the Events tab. Under "Available Event Providers" you can now add "Creative Cloud Assets".  
  
  ![The Events tab](img/Console_7.png "The Events tab")  

2. Select "Add Webhook". Give the webhook a name and description. As the webhook URL, fill in the URL provided by ngrok, but change the protocol to `https`; for example,  `https://595ae592.ngrok.io`. Also, check the boxes for the three available event types: Asset Created, Updated, Deleted.  
  
  ![Registering the webhook](img/Console_8.png "Registering the webhook")  

3. Select "Save", and go check the ngrok log. You should see a `GET` request, including the `challenge` that was passed along in the URL.  
  
  ![The webhook request received in ngrok](img/ngrok_2.png "The webhook request received in ngrok")  

4. Check the Adobe I/O Console and your webhook should be listed as "Active".  
  
  ![The active webhook in the Console](img/Console_9.png "The active webhook in the Console")  

<a id="orgecb4ae5"></a>

## Receiving Events

Log in to [Creative Cloud Assets (<https://assets.adobe.com>)](https://assets.adobe.com). Use the same Adobe ID as the one you used in the Adobe I/O Console.

The webhook you created through the Adobe I/O Console uses your own credentials, and so only receives events related to your Adobe ID. In a real-world application, you would use the credentials of an authenticated user to register a webhook through the API. This way you will receive events related to that user.

Now upload a file and check the ngrok logs again. If all went well, then an `asset_created` event was just delivered to your webhook.


<a id="orgd004238"></a>

## Using the API

You can also create webhooks, and retrieve information about them, using the Adobe I/O Events REST API. The following examples will use [cURL](https://curl.haxx.se/) to make HTTP requests from the terminal.


<a id="org649c409"></a>

### Authentication

All API requests must include four pieces of identifying information: the `CLIENT_ID`, `CONSUMER_ID`, and `APPLICATION_ID` of the integration, as well as a OAuth2 token.

The `CLIENT_ID` is found on the overview page of the integration under "Client Credentials / API Key (Client ID)"

The `CONSUMER_ID` and `APPLICATION_ID` can be found in the URL of the integration, for instance, `https://console.adobe.io/integrations/9999/1111/overview` has a `CONSUMER_ID` of 9999 and a `APPLICATION_ID` of 1111.

For development and testing you can find an OAuth2 token corresponding to your own Adobe ID by going to [console.adobe.io](https://console.adobe.io/integrations/new), opening the browser console (F12), and executing `adobeIMS.getAccessToken()`.

In a real-world application you would use the [Creative SDK](http://creativesdk.adobe.com/) to acquire an OAuth2 Token for the currently authenticated user. How to do that depends on your platform (iOS, Android, or Web), and is beyond the scope of this introduction.

The token, `CLIENT_ID`, `APPLICATION_ID`, and `CONSUMER_ID` are included in each request as HTTP headers:

```restclient
Authorization: Bearer OAUTH2_TOKEN
x-api-key: CLIENT_ID
x-ams-application-id: APPLICATION_ID
x-ams-consumer-id: CONSUMER_ID
```

The `CLIENT_ID` is also included in the request body.


<a id="org226a51a"></a>

### Creating the webhook

Instead of going through the Adobe I/O Console, you can use the CSM (Channel & Subscription Management) API to do the same thing. This cURL command registers a webhook. Make sure to replace `OAUTH2_TOKEN`, `APPLICATION_ID`, `CONSUMER_ID`, and `CLIENT_ID` with their actual values.

_Note: In order to run the cURL commands and call the APIs directly, you need to contact the Adobe I/O Events team to make sure your client ID is whitelisted for those APIs. You can do that via the email address that invited you to the beta._

```shell
curl https://csm.adobe.io/csm/webhooks \
  -H 'Authorization: Bearer OAUTH2_TOKEN' \
  -H 'x-api-key: CLIENT_ID'
  -H 'x-ams-application-id: APPLICATION_ID' \
  -H 'x-ams-consumer-id: CONSUMER_ID' \
  -H 'Content-Type: application/json' \
  -X POST \
  -d '{"client_id": "CLIENT_ID",
       "name": "My first webhook",
       "description": "My first webhook description",
       "webhook_url": "https://sleeping-beauty.webscript.io/script",
       "events_of_interest": [
         {"provider": "ccstorage", "event_code": "asset_created"},
         {"provider": "ccstorage", "event_code": "asset_updated"},
         {"provider": "ccstorage", "event_code": "asset_deleted"}]}'
```


<a id="org85f36da"></a>

#### Response

```json
{
  "description" : "My first webhook description",
  "client_id" : "ff2df829879cff2df829879c",
  "registration_id" : "6bd980fa-00d3-4d61-be8e-5d4f14691111",
  "status" : "VERIFIED",
  "delivery_type" : "WEBHOOK",
  "id" : 1049,
  "webhook_url" : "https://demo-ddldny.webscript.io/script",
  "events_of_interest" : [
    {
      "event_code" : "asset_created",
      "provider" : "ccstorage"
    },
    {
      "provider" : "ccstorage",
      "event_code" : "asset_deleted"
    },
    {
      "event_code" : "asset_updated",
      "provider" : "ccstorage"
    }
  ],
  "parent_client_id" : "ff2df829879cff2df829879c",
  "type" : "APP",
  "name" : "My first webhook",
  "integration_status" : "ENABLED"
}
```


<a id="org1a8bf8b"></a>

### Checking that it worked

You can also use the API to get a list of all the webhooks, so you can verify that the webhook is really there.

This uses a GET request, so there is no request body. Instead the `CLIENT_ID` is now included in the URL.

```shell
curl https://csm.adobe.io/csm/webhooks/CLIENT_ID \
  -H 'Authorization: Bearer OAUTH2_TOKEN' \
  -H 'x-api-key: CLIENT_ID'
  -H 'x-ams-application-id: APPLICATION_ID' \
  -H 'x-ams-consumer-id: CONSUMER_ID'
```


<a id="org5cc962d"></a>

### Response

```json
[
  {
    "description" : "My first webhook description",
    "client_id" : "ff2df829879cff2df829879c",
    "registration_id" : "6bd980fa-00d3-4d61-be8e-5d4f14691111",
    "status" : "VERIFIED",
    "delivery_type" : "WEBHOOK",
    "id" : 1049,
    "webhook_url" : "https://demo-ddldny.webscript.io/script",
    "events_of_interest" : [
      {
        "event_code" : "asset_created",
        "provider" : "ccstorage"
      },
      {
        "provider" : "ccstorage",
        "event_code" : "asset_deleted"
      },
      {
        "event_code" : "asset_updated",
        "provider" : "ccstorage"
      }
    ],
    "parent_client_id" : "ff2df829879cff2df829879c",
    "type" : "APP",
    "name" : "My first webhook",
    "integration_status" : "ENABLED"
  }
]
```
