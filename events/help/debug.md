<!--:navorder: 1-->

# Debugging Adobe I/O Events 

This page captures the most common troubleshooting scenarios when working with Adobe Events. 

* [AEM Events](#aemevents)
* [Analytics Triggers Events](#analyticstriggersevents)

## AEM Events

If your integration isn't receiving events from your installation of Adobe Experience Manager, 
you can perform any of several health checks 
on your setup to see where the communication between AEM and your integration is breaking down. 

For more information on health checks, see:

- [Operations Dashboard](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/operations-dashboard.html) in Adobe Help
- [Perform an AEM Health and Configuration Check](../using/aem-event-setup.md#performawebhookhealthcheck) in the topic [Setting up AEM Events with Adobe I/O Events](../using/aem-event-setup.md) 


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
