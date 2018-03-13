# Debugging Adobe I/O Events 

This page captures the most common troubleshooting scenarios when working with Adobe Events. 

* [AEM Events](#aem)
* [Analytics Triggers](#analytics)
* [JSON Web Tokens](#jwt)

## <a id="aem">AEM Events</a>

_**Issues during Installation – Unable to Upload your keystore.p12**_
The upload of the keystore.p12 file can fail. Here is a sample log message for this error:

```
javax.servlet.ServletException: java.security.UnrecoverableKeyException: Get Key failed: Given final block not properly padded at com.adobe.granite.security.user.internal.servlets.KeyStoreManagingServlet.doPost(KeyStoreManagingServlet.java:331)
```

**Possible Solutions:**

we resolved this by generating the key files on another computer and using that info. so try this first. 
this is likely a mismatch of password. There are 3 password in this process, a keystore password (A), a private key password (B), and an account password (C)
Refer to this gist: https://gist.github.com/sarahxxu/4bdb1fca4c88194242f673cfdf2d94ad
The keystore password (A) is created in line 33-34, the private key password (B) is created in line 25-26, and the account password (C) is likely created when you are on the eventproxy account config screen 
Please find their corresponding mapping in UI in the images below. 


double check that you are uploading the keystore.p12 file, and make sure you are using the one that you just created
double check your openssl version (the instruction above is created with OpenSSL 0.9.8zh)
command: openssl version

_**AEM Events not firing – Debug**_
The best way to debug AEM Events is through the health checks. By running through all the health checks, we can identify where the problem is. Most of it is covered in our documentation, here we are mainly discussing how to debug when you see this health check failing. 

**Health check 1: eventproxy, conf**

Conf stands for configuration. This health check makes sure that all your configurations properly load.
If this health check is failing, the best steps to do it:

Make sure you have the correct package version and update to the latest (currently 0.32.221)
And that when you installed your package you chose the right install setting
Make sure your AEM instance is compatible (6.2 and 6.3, 6.4 support unknown)
Check permissions for eventproxy
Match your I/O Events configuration to the integration that you've created in console

**Health check 2: eventproxy, ims**

This health check ensures that the AEM instance is able to exchange JWT tokens with Adobe I/O Identity Management System (IMS). This verifies that your IMS related configurations are correct and working, including the eventproxy-service user Keystore configuration, the Adobe I/O console-originated API key, the Technical Account ID, the Organization ID and the client secret.

If this health check is failing, the best steps to do it:

Match your I/O Events configuration to the integration that you've created in console
Make sure that they keystore you've uploaded match the public certificate in your integration

**Health check 3: eventproxy, csm**

This health check ensures that the event metadata and the provider associated with the AEM instance are registered in Adobe I/O Channel & Subscription Management (CSM). This verifies that the AEM instance is successfully registered as an event provider with Adobe I/O CSM.

You can also check this via Heimdall. Go to the providers tab and put in your org id. If your AEM instance shows up, it has been successfully registered with CSM. 

**Health check 4: eventproxy, eventreceiver**

This health check makes sure events are sent to and received by Adobe I/O Event receiver (the AEM ingress adapater), it will result in a POST message in your webhook (if you have your webhook added to your Console)

If this health check is failing, the best steps to do it:

Check your webhook and make sure that it is still marked as valid in your integration
you can click 'verify' to send another 'GET' challenge to your webhook for validation
you can also check this on Heimdall. On home page, put in the client id for your integration, and you can see all the webhooks registered and its current state. 
 
**Health check 5: eventproxy, custom**

This health check emits a custom OSGI event sample to your registered webhook, it will result in a message in your webhook (if you have your webhook added to your Console)

If this health check is failing, the best steps to do it:

Check your webhook (see above)
 
## <a id="analytics">Analytics Triggers</a>
Analytics Triggers via I/O Events consists of the following steps/components:

1. Web Page -> 2. Analytics Call -> 3. Analytics Triggers -> 4. Adobe I/O Events -> 5. Webhook

**Debug 1->2**

If 1→2 is working, it means that Analytics code has been embedded in your webpage, and analytics calls (note! not necessarily Trigger calls) are firing and going through. 
You can verify your Adobe Analytics connection via the Debugger:

https://chrome.google.com/webstore/detail/debugger-for-adobe-analyt/bdingoflfadhnjohjaplginnpjeclmof
https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj

**Debug 2->3**

If 2→3 is working, it means that your Triggers pattern is valid and reflects the customer behavioral pattern that you are trying to mirror
If you verified that Analytics connection is going through, but no Trigger has been fired, you can try the following methods:

Make sure you have outlasted the "inactivity time" that you've set. e.g. if you set it to be 10 mins, then make sure there is absolutely no actions on your page for 10 mins so that Triggers can identify this pattern and fire.
Compare your Triggers setting to your Analytics live output
Make sure you are talking to the correct reporting suite
Make sure you are using the correct eVar/prop to set up rules in Triggers
If you have an url, try removing the prefix (use 'localhost' instead of 'http://localhost')

**Debug 3->4**

If 3→4 is working, it means that your Triggers payload is arriving at the Adobe I/O Event Gateway. If you can see your Trigger fired, but it's not arriving at your webhook you should first debug 4->5 to make sure your webhook is valid and ready to receive events. If 4->5 works and you are still not receiving events, it could be that something went wrong in the Triggers-Pipeline-Event Gateway process. Unfortunately, there's no way to easily debug this step at the moment. The only way is to contact the I/O Events Engineering team. 

One possible debug method:

Open a webhook on https://io-webhook.herokuapp.com/, and add it to your webhook list on I/O Console listening to your Trigger. If you receive events on io-webhook.herokuapp.com, but not on your webhook, there's something wrong with your webhook. Move to debug 4->5. 
If neither is receiving any events, even though you see Triggers firing on Trigger UI, contact I/O Events Engineering Team. 
Debug 4->5

If 4→5 is working, it means that your webhook is valid and ready to receive events. 
You can verify your connection by clicking the "retry" button for your webhook on I/O Console UI. You should receive a challenge. 
Your webhook needs to be able to return the challenge to be marked as a valid webhook. If it is marked as failed on the console UI, visit https://github.com/adobeio/adobeio-documentation/blob/master/events/event-setup/aem-event-setup.md#set-up-webhook-example for sample webhook code.

_**How to use the Triggers API**_
For debugging purposes, you might want to use the Triggers API to decrease the inactivity time for your trigger so that it fires faster. (The UI only allows the trigger to fire after 10 mins of inactivity)

**Quick way to use Triggers API to debug:**

See [Triggers Developer Guide](https://wiki.corp.adobe.com/display/omtrplatform/Triggers+Developer+Guide). You can actually use a (simpler to obtain) user access token. While you are connected to the adminconsole.adobe.com, go to the browser console, and type: adobeIMS.getAccessToken(). You can use this token as your access token for Triggers API. 
Helpful Resources:

Swagger Documentation: https://triggers.adobe.net/swagger
Here's a Postman collection that could help: [Triggers API.postman_collection.json](file:///download/attachments/1429652448/Triggers%2520API.postman_collection.json%3fversion=2&modificationDate=1516239199597&api=v2)
Environment variable in this postman collection
client_id: ims client id
client_secret: ims client secret
code: ims service token authorization code
access_token: the access token from your first call
org_id: your adobe org id
trigger_id: the trigger id you want to call/update

## <a id="jwt">JSON Web Tokens</a>
Use Adobe I/O Console to generate a JWT and try using that to request an access token

Go to the I/O console and try putting the private key in the "JWT" panel to generate a JWT token, and try the curl command
if this doesn't work, there's something with the private.key file you have
solution: generate a new set of public/private key (see adobe.io documentation for how to do that)
if this does work, it means you have the right key, and we move on to debug the way you generate your jwt token
Double check the JWT payload against the payload in console

exp - it should be a dynamic time stamp based on when you generate this jwt
iss - adobe org id
sub - technical account
aud - ims-na1.adobelogin.com/c/+clientid
different products
campaign - "https://ims-na1.adobelogin.com/s/ent_campaign_sdk" : true
target - "https://ims-na1.adobelogin.com/s/ent_marketing_sdk" : true
The JWT should be encoded in RS256

Use your https://jwt.io/ to decode your encoded JWT and verify signature