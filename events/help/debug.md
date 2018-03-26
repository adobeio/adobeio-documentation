<!--:navorder: 1-->

# Debugging Adobe I/O Events 

This page captures the most common troubleshooting scenarios when working with Adobe Events. 

* [AEM Events](#aemevents)
* [Analytics Triggers Events](#analyticstriggersevents)

## AEM Events

If your integration isn't receiving events from your installation of Adobe Experience Manager, you can perform any of several health checks on your setup to see where the communication between AEM and your integration is breaking down. For more information on health checks, see:

- [Operations Dashboard](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/operations-dashboard.html) in Adobe Help
- [Perform an AEM Health and Configuration Check](../using/aem-event-setup.md#performanaemhealthandconfigurationcheck) in the topic [Setting up AEM Events with Adobe I/O Events](../using/aem-event-setup.md) 

Please try the following health checks in order:

**Health check 1: eventproxy, conf**  
Conf stands for configuration. This health check makes sure that all your configurations properly load. 

If this health check is failing, check the following:

- Make sure you have the correct event proxy package and update it to the latest version (currently [0.32.226](https://github.com/adobeio/adobeio-documentation/files/1723221/aem-event-proxy-0.36.226.zip)) if needed.
- Verify that when you installed your package, you chose the right install settings.
- Make sure your AEM instance is compatible with Events (must be version 6.2 or higher).
- Check the permissions for your event proxy package.
- Match your I/O Events configuration to the integration that you've created in the Adobe I/O Console.

**Health check 2: eventproxy, ims**  
This health check ensures that your AEM instance is able to exchange JWT tokens with the Adobe I/O Identity Management System (IMS). This verifies that your IMS-related configurations are correct and working, including the eventproxy-service user keystore configuration, the Adobe I/O Console-originated API key, the Technical Account ID, the Organization ID and the client secret.

If this health check is failing, check the following:

- Match your I/O Events configuration to the integration that you've created in Adobe I/O Console.
- Make sure that the keystore you've uploaded matches the public certificate in your integration.

**Health check 3: eventproxy, csm**  
This health check ensures that the event metadata and the provider associated with your AEM instance are registered in Adobe I/O Channel & Subscription Management (CSM). This verifies that the AEM instance is successfully registered as an event provider with Adobe I/O CSM.

**Health check 4: eventproxy, eventreceiver**  
This health check makes sure events are sent to and received by the Adobe I/O Event receiver (the AEM ingress adapater). It will result in a POST message in your webhook (if you have your webhook added to your integration in the Adobe I/O Console).

If this health check is failing, check the following:

- Check your webhook and make sure that it is still marked as valid in your integration. You can select **Verify** to send another GET challenge to your webhook for validation.  
 
**Health check 5: eventproxy, custom**

This health check emits a custom OSGI event sample to your registered webhook. It will result in a message in your webhook (if you have your webhook added to your integration in the Adobe I/O Console).

If this health check is failing, check the following:

- Check your webhook as in health check 4.

## Analytics Triggers Events
If your Analytics Triggers events aren't coming through to your integration, a breakdown in communication may have occurred at any step in the events process. You'll need to check each step in order to verify where the breakdown has occurred and then fix your configuration accordingly.

The process of communicating Analytics Triggers via I/O Events consists of the following steps:

(1) Web page > (2) Analytics call > (3) Analytics Triggers > (4) Adobe I/O Events > (5) Webhook

**Debug 1 > 2**

If 1 > 2 is working, it means that Analytics code has been embedded in your webpage, and analytics calls (**Note:** not necessarily Trigger calls) are firing and going through. 
You can verify your Adobe Analytics connection via the Debugger:

https://chrome.google.com/webstore/detail/debugger-for-adobe-analyt/bdingoflfadhnjohjaplginnpjeclmof
https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj

**Debug 2 > 3**

If 2 > 3 is working, it means that your Triggers pattern is valid and reflects the customer behavioral pattern that you are trying to mirror. If you verified that the Analytics connection is going through, but no Trigger has been fired, you can try the following methods:

- Make sure you have outlasted the &ldquo;inactivity time&rdquo; that you've set: for example, if you set it to be 10 minutes, then make sure there are absolutely no actions on your page for 10 minutes so that Triggers can identify this pattern and fire.
- Compare your Triggers setting to your Analytics live output.
- Make sure you are talking to the correct reporting suite.
- Make sure you are using the correct eVar/prop to set up rules in Triggers.
- If you have a URL, try removing the prefix (use 'localhost' instead of 'http://localhost').

**Debug 3 > 4**

If 3 > 4 is working, it means that your Triggers payload is arriving at the Adobe I/O Event Gateway. If you can see your Trigger fired, but it's not arriving at your webhook, you should first debug 4 > 5 to make sure your webhook is valid and ready to receive events. If 4 > 5 works and you are still not receiving events, it could be that something went wrong in the Triggers-Pipeline-Event Gateway process. Unfortunately, there's no way to easily debug this step at the moment. Please open an issue on the [Events GitHub project](https://github.com/adobeio/adobeio-documentation). 

**Debug 4 > 5**

If 4 > 5 is working, it means that your webhook is valid and ready to receive events. You can verify your connection by selecting **Retry** for your webhook on I/O Console UI. You should receive a challenge. Your webhook needs to be able to return the challenge to be marked as a valid webhook. If it is marked as failed on the console UI, visit the topic [Set up Webhook: Example](../using/aem-event-setup.md#settingupawebhookexample) for sample webhook code.
