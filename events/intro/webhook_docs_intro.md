<!--:nav_order:1-->

# Introduction to Adobe I/O Events Webhooks

- [Introduction](#introduction)
- [Concepts](#concepts)
    - [An example](#anexample)
- [Your first webhook](#yourfirstwebhook)
    - [The challenge request](#thechallengerequest)
    - [Testing with ngrok](#testingwithngrok)
- [Create an integration](#creatinganintegration)
- [Registering the webhook](#registeringthewebhook)
- [Receiving events](#receivingevents)
- [Authenticating events](#authenticatingevents)

## Introduction

With Adobe I/O Events webhooks, your application can sign up to be notified whenever certain events occur. For example, when a user uploads a file to Adobe Creative Cloud Assets, this action generates an event. With the right webhook in place, your application is instantly notified that this event happened.

To start receiving events, you register a webhook, specifying a webhook URL and the types of events you want to receive. Each event will result in a HTTP request to the given URL, notifying your application.

## Concepts

An **Event** is a JSON object that describes something that happened. 

Events originate from **Event Providers**. Each event provider publishes specific types of events, identified by an **Event Code**.

A **Webhook URL** receives event JSON objects as HTTP POST requests.

You start receiving events by creating a **Webhook Registration**, providing a name, description, webhook URL, and a list of event types you are interested in.

### An example

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

Now when a file is uploaded, Adobe I/O Events performs the following HTTP request:

```json
POST https://acme.example.com/webhook HTTP/1.1
content-type: application/json

{
  "@id": "82235bac-2b81-4e70-90b5-2bd1f04b5c7b",
  "@type": "xdmCreated",
  "xdmEventEnvelope:objectType": "xdmAsset",
  "activitystreams:published": "2016-07-16T19:20:30+01:00",
  "activitystreams:to": {
    "xdmImsOrg:id": "08B3E5CE5822FC520A494229@AdobeOrg",
    "@type": "xdmImsOrg"
  },
  "activitystreams:generator": {
    "xdmContentRepository:root": "http://francois.corp.adobe.com:4502/",
    "@type": "xdmContentRepository"
  },
  "activitystreams:actor": {
    "xdmAemUser:id": "admin",
    "@type": "xdmAemUser"
  },
  "activitystreams:object": {
    "@type": "xdmAsset",
    "xdmAsset:asset_id": "urn:aaid:aem:4123ba4c-93a8-4c5d-b979-ffbbe4318185",
    "xdmAsset:asset_name": "Fx_DUKE-small.png",
    "xdmAsset:etag": "6fc55d0389d856ae7deccebba54f110e",
    "xdmAsset:path": "/content/dam/Fx_DUKE-small.png",
    "xdmAsset:format": "image/png"
  },
  "@context": {
    "activitystreams": "http://www.w3.org/ns/activitystreams#",
    "xdmEventEnvelope": "https://ns.adobe.com/xdm/common/eventenvelope#",
    "xdmAsset": "http://ns.adobe.com/xdm/assets/asset#",
    "xdmImsOrg": "https://ns.adobe.com/xdm/ims/organization#",
    "xdmContentRepository": "https://ns.adobe.com/xdm/content/repository",
    "xdmAemUser": "https://ns.adobe.com/xdm/aem/user#",
    "xdmCreated": "https://ns.adobe.com/xdm/common/event/created#"
  }
}
```

## Your first webhook

Before you can register a webhook, the webhook needs to be online and operational. If not, then the registration will fail. So you need to take care of setting that up first. Your webhook must be hosted on a server. For development, you may use localhost along with a tool like ngrok (see below).

Your webhook needs to

-   be accessible from the internet (localhost won't work)
-   be reachable over HTTPS
-   correctly respond to a "challenge" request

### The challenge request

When registering a webhook, Adobe I/O Events will first try to verify that the URL is valid. To do this, it sends an HTTP GET request, with a `challenge` query parameter. The webhook should respond with a body containing the value of the `challenge` query parameter.

```http
GET https://acme.example.com?challenge=8ec8d794-e0ab-42df-9017-e3dada8e84f7
```

#### Response

You can either respond by placing the challenge value directly in the response body:

```http
HTTP/1.1 200 OK

"8ec8d794-e0ab-42df-9017-e3dada8e84f7"
```

or by responding with a JSON object, including the correct `content-type` header:

```http
HTTP/1.1 200 OK
Content-type: application/json

{"challenge":"8ec8d794-e0ab-42df-9017-e3dada8e84f7"}
```

Typically, you would build your webhook to respond to the Adobe challenge in a method to handle HTTP GET requests, and then include another method for handling the HTTP POST requests that will be coming from Adobe containing actual event payloads. For testing purposes, though, you can start with something as simple as this PHP script: 

```php
<?php
 header('Content-Type: text/plain');
 echo $_GET['challenge']; 
?>
```

**Note:** Make sure your response is given in the correct content-type. If your webhook script places the challenge value directly in the response body, make sure it's returned as plain text (`text/plain`). For a JSON response, make sure it's `application/json`. Returning a response in the incorrect content-type may cause extraneous code to be returned, which will not validate with Adobe I/O Events.

### Testing with ngrok
[Ngrok](https://ngrok.com/) is a utility for enabling secure introspectable tunnels to your localhost. With ngrok, you can securely expose a local web server to the internet and run your own personal web services from your own machine, safely encrypted behind your local NAT or firewall. With ngrok, you can iterate quickly without redeploying your app or affecting your customers. 

Among other things, ngrok is a great tool for testing webhooks. Once you've downloaded and installed [ngrok](https://ngrok.com/), you run it from a command line, specifying the protocol and port you want to monitor:
```ngrok http 80```

![ngrok on port 80](../../img/ngrok.png "ngrok on port 80")

In the ngrok UI, you can see the URL for viewing the ngrok logs, labeled "Web Interface", plus the public-facing URLs ngrok generates to forward HTTP and HTTPS traffic to your localhost. You can use either of those public-facing URLs to register your Webhook with Adobe I/O, so long as your application is configured to respond on your localhost accordingly. Once your testing phase is complete, you can replace the ngrok URL in your Adobe I/O integration with the public URL for your deployed app.

## Creating an integration

To create an integration:

1. Open the [Adobe I/O Console](http://console.adobe.io/); by default, you should be on the Integrations panel. If not, select it from the top menu.  
  
  ![Adobe I/O Console](../../img/events_console_01.png "Adobe I/O Console")  
  
2. Select [New integration](https://console.adobe.io/integrations/new). You can choose **Access an API,** **Receive near real-time events,**  or **Deploy serverless actions.** Choose **Receive near real-time events** and select **Continue**.  
  
  ![Create a new integration](../../img/events_console_02.png "Create a new integration")  
  
3. Now you&rsquo;re given the choice of Adobe event provider for your integration. Choose **Creative Cloud Assets** and select **Continue.**  
  
  ![Choosing Creative Cloud Assets](../../img/events_console_03.png "Choosing Creative Cloud Assets")  
  
4. Now you can choose between creating a brand new integration, or updating an existing one. Choose to create a new integration.  
  
  ![A brand new integration](../../img/events_console_04.png "A brand new integration")  
  
5. Enter a name and description for the integration. As a platform, choose **Web.** You also need to specifiy a default redirect URI and redirect URI pattern. These are only relevant when using the [Creative Cloud SDK User Auth UI](https://creativesdk.adobe.com/docs/web/#/articles/userauthui/index.html). For now you can fill in `https://example.com` and `https://example\.com*` respectively.  
  
  ![Specifying the integration](../../img/events_console_05.png "Specifying the integration")

## Registering the webhook

To complete the integration, you need to add a webhook. 

1. Select **Add Event Registration.** The dialog expands to display the webhook details.  

2. Give the webhook a name and description. As the webhook URL, fill in the URL provided by ngrok, but change the protocol to `https`; for example,  `https://595ae592.ngrok.io`. Also, check the boxes for the three available event types: Creative Cloud Asset Deleted, Updated, and Created.  
  
  ![Specifying the webhook](../../img/events_console_06.png "Specifying the webhook")  

3. Select **Save,** complete the CAPTCHA (&ldquo;I'm not a robot&rdquo;), and then select **Create integration.** You should see an acknowledgement that your integration has been created. 
  
  ![Completing the integration](../../img/events_console_07.png "Completing the integration")  

4. Check the ngrok log. You should see a `GET` request, including the `challenge` that was passed along in the URL.  
  
  ![The challenge GET request received in ngrok](../../img/ngrok_2.png "The challenge GET request received in ngrok")  

5. Return to the Adobe I/O Console. Select **Continue to integration details** and you'll be shown the Integration Overview. This is where you can see all your integration details and make updates as needed.  
  
  ![The Integration Overview](../../img/events_console_08.png "The Integration Overview")  

6. Select the Events tab. Your webhook should be listed as &ldquo;Active&rdquo;.    
  
  ![The active webhook in the Console](../../img/events_console_09.png "The active webhook in the Console")

  >**Note:** If you had made an error in transcribing the URL, Adobe Events&rsquo; test of your webhook would have failed; instead of seeing the confirmation screen, you&rsquo;d see an error: &ldquo;Webhook verification failed or unreachable&rdquo;. You can also get this error if, for any reason, your webhook&rsquo;s endpoint is down. 
  >
  >In general, Adobe I/O Events will always confirm that your webhook received an event by means of the response code your webhook sends to each HTTP POST request. If Adobe fails to receive a 200 OK response code within 10 seconds, it retries the request, including a special header: `x-adobe-retry-count`. The value of this header begins at 1. If the first retry request fails as well, Adobe waits, then retries again, incrementing the value of `x-adobe-retry-count` with each retry until it reaches 5. Each wait interval is the square of the previous interval. 
  >
  >Once five retries are attempted, Adobe sends one last challenge request, and if that fails, Adobe marks the webhook as invalid and stops sending requests. To restart the flow of requests, once you have fixed the problem preventing your webhook from responding, you must log into Adobe I/O Console and reactivate the webhook. While your webhook is marked invalid, Adobe will continue to log events, even though it isn&rsquo;t sending them; you can retrieve all your events for the past 30 days by means of the [Journaling API](../intro/journaling_api.md).
    
## Receiving events
For development, you must first provide consent for yourself. You must follow the last step in https://www.adobe.io/apis/cloudplatform/events/documentation.html#receiveevents, which is `https://ims-na1.adobelogin.com/ims/authorize/v1?response_type=code&client_id=api_key_from_io_console&scope=AdobeID%2Copenid%2Ccreative_sdk`, where you will replace `api_key_from_io_console` with the value from your Adobe I/O Console overview tab `API Key (Client ID)`, for this integration.

Log in to [Creative Cloud Assets (<https://assets.adobe.com>)](https://assets.adobe.com). Use the same Adobe ID as the one you used in the Adobe I/O Console. Now upload a file and check the ngrok logs again. If all went well, then an `asset_created` event was just delivered to your webhook. 

![The POST request received in ngrok](../../img/ngrok_2.png "The POST request received in ngrok")  

### Receiving events for users
The webhook you created through the Adobe I/O Console uses your own credentials, and so only receives events related to your Adobe ID. In a real-world application, you would use the credentials of an authenticated user to register a webhook through the API. This way you will receive events related to that user. Depending on your scenario and the Adobe service you&rsquo;re targeting, you may have to enable different types of authentication; see the [Adobe I/O Authentication Overview]() for more information on how to set up your app for authentication with your users.

For Creative Cloud Asset events, you&rsquo;ll need to add the Creative Cloud SDK service to your integration and implement the User Auth UI; see [Setting Up Creative Cloud Asset Events](../using/cc-asset-event-setup.md) for details. 

## Authenticating events
Your webhook URL must by necessity be accessible from the open internet. This means third-party actors can send forged requests to it, tricking your application into handling fake events.
 
To prevent this from happening, Adobe I/O Events will add a `x-adobe-signature` header to each POST request it does to your webhook URL, which allows you to verify that the request was really made by Adobe I/O Events.
 
This signature or &ldquo;message authentication code&rdquo; is computed using a cryptographic hash function and a secret key applied to the body of the HTTP request. In particular, a SHA256 [HMAC](https://en.wikipedia.org/wiki/HMAC) is computed of the JSON payload, using your **client secret** as a secret key, and then turned into a Base64 digest. You can find your client secret in the Overview tab of your integration:

![Retrieve client secret](../../img/events_console_10.png "Retrieve client secret")
 
Upon receiving a request, you should repeat this calculation and compare the result to the value in the `x-adobe-signature` header, and reject the request unless they match. Since the client secret is known only by you and Adobe I/O Events, this is a reliable way to verify the authenticity of the request.
 
**HMAC check implementation in JavaScript (pseudo-code):**
 
```javascript
var crypto = require('crypto')
const hmac = crypto.createHmac('sha256', ADOBE_CLIENT_SECRET)
hmac.update(raw_request_body)
 
if (request.header('x-adobe-signature') !== hmac.digest('base64')) {
  throw new Error('x-adobe-signature HMAC check failed')
}

