# Introduction

<a id="intro_what"></a>
**Adobe I/O Events** enables building reactive, event-driven applications, based on events originating from various Adobe Services (Creative Cloud, Adobe Experience Manager, etc.)

Events are triggered by an **Event Provider**, like Adobe Creative Cloud Assets, whenever a certain real-world action occurs, such as creating a new asset.

To start listening for events, your application registers a **webhook**, specifying which **Event Types** from which **Event Providers** it wants to receive. Whenever a matching event gets triggered, your application is notified through an HTTP request.

Through the Webhooks API you can create, retrieve, update, or delete webhook registrations, specifying the types of events to be notified of, as well as the URLs where notifications should be sent to.

## Prerequisites

**Creative Cloud Events**
To work with Creative Cloud Events, you would need an active Adobe ID.

**Experience Cloud Events**
To work with events for Adobe services in Experience Cloud, you would need to have corresponding entitlements for this Adobe service in your organization, and administrative permission for your org to create integrations. 

## Get Started with Available Events
- Creative Cloud Events
    - [Creative Cloud Asset Events](using/cc-asset-event-setup.md)
- Experience Cloud Events
    - [Adobe Experience Manager Events](using/aem-event-setup.md)
    - [Analytics Triggers Events (Private beta)](using/analytics-triggers-event-setup.md)


## Key Concepts
- [Webhook Introduction](Webhook_docs_intro.md)
  - [Sample Webhook in Node.js](https://github.com/adobeio/io-event-sample-webhook)
  - [Sample Webhook in Python]() *TK from Carmen*
  - [Sample Webhook in PHP]() *TK*
  - [Work with Webhooks using Webhook API](help/Webhook_docs_reference.md)
- [Journaling API](intro/journaling_api.md)
- [CSM API / Console API (references doc) - in the future might be reference to its own doc (tk)](intro/csm-console-api.md)
- [Use cases for events](intro/use_cases.md)


 
