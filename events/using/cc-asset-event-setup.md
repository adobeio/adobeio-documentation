# Set up Creative Cloud Asset Events on Adobe I/O Events

These instructions describe how to set up Creative Cloud Asset events using Adobe I/O Events. You can use Adobe I/O for notification of CC Asset events.

1. [Introduction](#Introduction)

2. [Create an Integration](#create-integration)

3. [Use Adobe I/O](#Use-Adobe-I/O)

4. [Watch the Solution Work](#Watch-It-Work)

<a id="Introduction">&nbsp;</a>

## Introduction
Creative Cloud Assets provides a simple set of events to which you can subscribe: 
- **asset-updated:** Triggers when an asset is changed or modified.
- **asset-created:** Triggers when a new asset is uploaded to Creative Cloud, or when an asset is _copied_ to a new folder (not when an asset is merely moved).
- **asset-deleted:** Triggers when an asset is _permanently_ deleted from Creative Cloud. Merely archiving an asset does not trigger the event. 

There are no events for the following activities:
- Moving a file from one folder in Creative Cloud to another.
- Renaming a file. This is because assets are tracked in Creative Cloud by GUIDs, and the GUID doesn't change when the file is renamed. Creative Cloud recognizes that the asset hasn't changed, and can still find the asset by the same GUID. Any URL paths you create to that file, however, would change, since they do include the filename.

### Getting access to Creative CLoud Assets events
Unlike other Cloud Platform event providers, Creative Cloud Assets does not require an enterprise account, or administrative status, to gain access for creating integrations or receiving events. However, the integrations you create will still need to authenticate the same way any other Adobe integrations do (see Authentication under [Introduction to Events](../intro.md)). Consider what kind of authentication your integration needs before you start, and follow the correct procedure (see [Adobe Authentication](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)) to get the access rights your integration needs.

## <span id="create-integration">Create an Integration</span>
For the purposes of this example, you&rsquo;ll be creating an individual integration using your personal Adobe ID. 

To create an integration for Creative Cloud Assets:

1. Log into [Adobe I/O Console](https://console.adobe.io) using your personal account. You'll see a list of any integrations you've created so far. If this is your first, you'll see a button for creating an integration.

    ![Create an integration](../../img/CCA_Events_01.png "Create an integration")  

2. Select "New integration". The "Create a new integration" screen appears. 

    ![Choosing a new integration type](../../img/CCA_Events_02.png "Choosing a new integration type")

3. Select "Receive new real-time events" and continue.

4. Select an event provider: Since you&rsquo;re using your personal account, the only provider you&rsquo;ll see is Creative Cloud Assets. Choose &ldquo;Creative Cloud Assets&rdquo; and continue.

    ![Selecting the provider](../../img/CCA_Events_03.png "Selecting the provider")

5. You're offered one last chance to update an existing integration, if you have any; select "New integration" and continue.

    ![Choosing a new integration type](../../img/CCA_Events_03.png "Choosing a new integration type")

6. Enter details for the integration. Console needs a name and a description; these can be whatever you want, subject only to length restrictions. Choose "Web" for the platform and provide a redirect URI and a redirect URI pattern.

    ![Entering integration details](../../img/CCA_Events_04.png "Entering integration details")

>Note: Your integration needs to send a redirect URI to Adobe when it authenticates on behalf of a user, to send them to your integration once authentication is complete. The redirect URI you provide here is a default, to which Adobe I/O will fall back if the redirect URI in your authentication request fails. The redirect URI pattern is used by Adobe I/O to validate the redirect URI you provide with an authentication request. ALL redirect URIs must use HTTPS.

### Add a webhook
When you select &ldquo;Add webhook&rdquo;, the dialog expands to provide fiels for you to define a webhook to receive Adobe Events. (For more on webhooks, see [Adobe I/O Events Webhooks](../intro/webhook_docs_intro.md).) The webhook you ultimately use should be part of the app you develop as your integration. For now, however, it's easy to set up a simple webhook to test your integration&rsquo;s connection with Adobe Events. 

Several tools exist on the web that can be used for this purpose: [ngrok](https://ngrok.com/), [Postman](https://www.getpostman.com/), and [Webtask](https://webtask.io), for example. For this example, use Webtask.

1. Go to https://webtask.io. On the home page, either sign in (if you already have an account) or select &ldquo;Try it now!&rdquo;.

    ![Webtask home page](../../img/CCA_Events_05.png "Webtask home page")

2. You'll be taken to the Webtask web UI. There's also a command-line tool you can install if you choose.

    ![Webtask home page](../../img/CCA_Events_06.png "Webtask home page")

3. Select &ldquo;Create a new one&rdquo;. Once you focus on &ldquo;Webtask Function&rdquo;, the dialog expands to offer you a choice of creating an empty one, or selecting a template. 

    ![Creating an empty webtask](../../img/CCA_Events_07.png "Creating an empty webtask")

4. Start with an empty one. Give it a name suitable for your integration and select Save. A new webtask opens with the default template in place.

    ![The new webtask open](../../img/CCA_Events_08.png "The new webtask open")

5. Copy and paste this code into the Webtask editor, replacing the default contents, and select Save: 

```js
var express = require('express');
var Webtask = require('webtask-tools');
var bodyParser = require('body-parser');
var app = express();

app.use(bodyParser.json());
app.get('/webhook', function (req, res) {
   var result = "No challenge";
   if (req.query["challenge"]){
      result = req.query["challenge"]
      console.log("got challenge: " + req.query["challenge"]);
   } else {
      console.log("no challenge")
   }
   res.status(200).send(result)
});

app.post('/webhook', function (req, res) { 
   console.log(req.body)
   res.writeHead(200, { 'Content-Type': 'application/text' });
   res.end("pong");
});

module.exports = Webtask.fromExpress(app);
```

This simple webhook is designed merely to do what Adobe Events requires: handle an HTTPS GET request containing a `challenge` parameter by returning the value of the challenge parameter itself. 

6. With the code in the Webtask editor, you'll see the URL for your webtask&rsquo;s endpoint at the bottom of your browser window: 

    ![The webtask's endpoint URL](../../img/CCA_Events_09.png "The webtask's endpoint URL")

    Notice also that the `app.get` and `app.post` functions specify a relative path, `/webhook`: you&rsquo; need to add this to the end of the webtask&rsquo; endpoint URL to reach your webhook. For example, 

    `https://wt-7ef4200cf2cfff39f542f26708adb75c-0.run.webtask.io/CCAssetsEvents_webhook`**`/webhook`**

7. Now you're ready to complete the webhook registration process in Adobe I/O Console. Return to that window and enter the name, URL, and description for the webhook, pasting in the URL you got from Webtask with the `/webhook` term added. Select all three events to receive: 
    - Creative Cloud Asset Created (`asset-created`) 
    - Creative Cloud Asset Updated (`asset-updated`)
    - Creative Cloud Asset Deleted (`asset-deleted`)

    ![Entering webhook details](../../img/CCA_Events_10.png "Entering webhook details")

8. Save and complete the CAPTCHA. Select &ldquo;Create  integration". At this point, Adobe Events sends a test event to your webhook's destination URL. If your webhook responds correctly with the contents of the `challenge` parameter, Adobe Events completes your integration:

    ![Integration created](../../img/CCA_Events_10.png "Integration created")

    Select &ldquo;Continue to Integration details&rdquo; to view and manage your integration.
    