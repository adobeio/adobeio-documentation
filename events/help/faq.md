<!--:navorder: 3-->

# Adobe I/O Events Frequently Asked Questions (FAQ)

* [General questions](#generalquestions)
* [AEM events](#aemevents)
* [Analytics Triggers events](#analyticstriggersevents)

## General questions
_**What are I/O Events?**_  
I/O Events make meaningful Adobe Cloud Platform events available to 1st and 3rd party developers.

_**Which events are currently supported via I/O?**_  
Currently two Adobe solutions are supported via I/O Events:

* Adobe Experience Manager Page, Asset, & Custom Events (create, update, delete)
* Adobe Analytics Triggers
* We will soon add Creative Cloud Asset Events (create, update, delete)

_**What permissions are required to use I/O Events?**_  
To access the Adobe I/O Console and create or manage integrations for AEM and Analytics Triggers, a user must have Enterprise Administrator permissions.

_**Which subscription types do I/O Events support?**_  
I/O Events support webhooks for near-real time notifications (push) as well as a Journaling API (pull) to grab groups of events at a time.

_**How far back are I/O Events available via the Journaling API?**_  
Adobe I/O stores up to 30 days of subscribed events that can be retrieved via the Journaling API.

_**What happens if a webhook is down?**_  
I/O Events will retry up to five times. A webhook is considered inactive after five unsuccessful retries with either no response or a response other than HTTP 2XX. The Journaling API can be used to retrieve events that were published while the webhook is down.

_**Are there other ways to access I/O Events?**_  
Yes: 
- [Azuqua](https://www.azuqua.com) provides connectors for both AEM events and Analytics Triggers events.
- [Microsoft Flow](https://flow.microsoft.com) has a connector for Creative Cloud Asset events.

_**What is JWT and what is it used for?**_  
JSON Web Token (JWT) is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. For more information on JSON Web Tokens, see [Introduction to JSON Web Tokens](https://jwt.io/introduction/) on JWT.io.

To use Adobe I/O Events, you&rsquo;ll need a set of client credentials to authenticate the identity of the caller and authorize access. The type of application you are building determines the type of integration that provides the client credentials you will need.

A Service Account integration allows your application to call Adobe services on behalf of the application itself, or on behalf of an enterprise organization.

For this type of integration, you&rsquo;ll need to create a JSON Web Token (JWT) that encapsulates your credentials and begin each API session by exchanging the JWT for an access token. The JWT encodes all of the identity and security information required to obtain an access token,  and must be signed with the private key that is associated with a public key certificate specified on your integration.

_**Where can I find documentation on JSON Web Token (JWT) Service accounts and how to set them up?**_  
See the Authentication Guide: [Creating a JSON Web Token](https://www.adobe.io/apis/cloudplatform/console/authentication/createjwt.html).

_**Do you have sample libraries for JWT?**_  
Yes:
- Python: https://pyjwt.readthedocs.io/en/latest/
- .Net: https://github.com/jwt-dotnet/jwt
- Java: https://github.com/jwtk/jjwt
- Objective C: https://github.com/yourkarma/JWT
- Node.js: https://github.com/auth0/node-jsonwebtoken
- Node.js: https://www.npmjs.com/package/jsonwebtoken
- Node.js: https://www.npmjs.com/package/jwt-simple
- JavaScript tutorial: - https://www.jonathan-petitcolas.com/2014/11/27/creating-json-web-token-in-javascript.html
- Javascript: http://kjur.github.io/jsrsasign/

## AEM events

_**Which versions of AEM is supported for I/O Events?**_  

AEM 6.2.xx, 6.3.xx as well as 6.4.xx, see [Set up AEM Events with Adobe I/O Events](../using/aem-event-setup.md##installtheaemeventproxypackage) 

_**What do I need to leverage AEM events?**_  
A separate package is required to use AEM I/O Events. 

_**Where can I find instructions on setting up AEM I/O Events?**_  
See [Set up AEM Events with Adobe I/O Events](../using/aem-event-setup.md) in this guide.

_**What does the AEM event payload look like?**_  
Here are sample payloads for all AEM events: 

- _Asset events:_
    - Asset created:

        ```json
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

    - Asset deleted:
    
        ```json
        {
          "@id": "82235bac-2b81-4e70-90b5-2bd1f04b5c7b",
          "@type": "xdmDeleted",
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
            "xdmAsset:asset_id": "urn:aaid:aem:4123ba4c-93a8-4c5d-b979-ffbbe4318185"
          },
          "@context": {
            "activitystreams": "http://www.w3.org/ns/activitystreams#",
            "xdmEventEnvelope": "https://ns.adobe.com/xdm/common/eventenvelope#",
            "xdmAsset": "http://ns.adobe.com/xdm/assets/asset#",
            "xdmImsOrg": "https://ns.adobe.com/xdm/ims/organization#",
            "xdmContentRepository": "https://ns.adobe.com/xdm/content/repository",
            "xdmAemUser": "https://ns.adobe.com/xdm/aem/user#",
            "xdmDeleted": "https://ns.adobe.com/xdm/common/event/deleted#"
          }
        }
        ```

    - Asset updated:
    
        ```json
        {
          "@id": "82235bac-2b81-4e70-90b5-2bd1f04b5c7b",
          "@type": "xdmUpdated",
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
            "activitystreams:mediaType": "image/png",
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
            "xdmUpdated": "https://ns.adobe.com/xdm/common/event/updated#"
          }
        }
        ```

- _Custom OSGI events:_
    - Custom OSGI event emitted:
      
        ```json
        {
          "@id" : "82235bac-2b81-4e70-90b5-2bd1f04b5c7b",
          "@type" : "xdmEmitted",
          "activitystreams:published" : "2016-07-16T19:20:30+01:00",
          "activitystreams:to" : {
            "@type" : "xdmImsOrg",
            "xdmImsOrg:id" : "08B3E5CE5822FC520A494229@AdobeOrg"
          },
          "activitystreams:generator" : {
            "@type" : "xdmContentRepository",
            "xdmContentRepository:root" : "http://francois.corp.adobe.com:4502/"
          },
          "activitystreams:object" : {
            "@type" : "osgiEvent:io/adobe/event/sample/sku",
            "osgiEvent:topic" : "io/adobe/event/sample/sku",
            "osgiEvent:properties" : {
              "type" : "created",
              "id" : "1234"
            }
          },
          "xdmEventEnvelope:objectType" : "osgiEvent:io/adobe/event/sample/sku",
          "@context": {
            "activitystreams": "http://www.w3.org/ns/activitystreams#",
            "xdmEventEnvelope": "https://ns.adobe.com/xdm/common/eventenvelope#",
            "xdmImsOrg": "https://ns.adobe.com/xdm/ims/organization#",
            "xdmContentRepository": "https://ns.adobe.com/xdm/content/repository",
            "xdmEmitted" : "https://ns.adobe.com/xdm/common/event/emitted#",
            "osgiEvent" : "https://osgi.org/javadoc/r4v42/org/osgi/service/event/Event.html"
          }
        }
        ```

- _Page events:_
    - Page published:
    
        ```json
        {
          "@id": "82235bac-2b81-4e70-90b5-2bd1f04b5c7b",
          "@type": "xdmPublished",
          "xdmEventEnvelope:objectType": "xdmComponentizedPage",
          "activitystreams:published": "2016-07-16T19:20:30+01:00",
          "activitystreams:to": {
            "xdmImsOrg:id": "08B3E5CE5822FC520A494229@AdobeOrg",
            "@type": "xdmImsOrg"
          },
          "activitystreams:generator": {
            "xdmContentRepository:root": "https://cloud-action-dev.corp.adobe.com:4502/",
            "@type": "xdmContentRepository"
          },
          "activitystreams:actor": {
            "xdmAemUser:id": "admin",
            "@type": "xdmAemUser"
          },
          "activitystreams:object": {
            "@id": "http://adobesummit.adobesandbox.com:4502/content/geometrixx/en/vintage.html",
            "@type": "xdmComponentizedPage",
            "xdmComponentizedPage:title": "Vintage Collection",
            "xdmComponentizedPage:path": "/content/geometrixx/en/vintage.html"
          },
          "@context": {
            "activitystreams": "http://www.w3.org/ns/activitystreams#",
            "xdmEventEnvelope": "https://ns.adobe.com/xdm/common/eventenvelope#",
            "xdmComponentizedPage": "https://ns.adobe.com/xdm/content/componentized-page#",
            "xdmImsOrg": "https://ns.adobe.com/xdm/ims/organization#",
            "xdmContentRepository": "https://ns.adobe.com/xdm/content/repository#",
            "xdmAemUser": "https://ns.adobe.com/xdm/aem/user#",
            "xdmPublished": "https://ns.adobe.com/xdm/common/event/published#"
          }
        }
        ```
    - Page unpublished:
    
        ```json
        {
          "@id": "82235bac-2b81-4e70-90b5-2bd1f04b5c7b",
          "@type": "xdmUnpublished",
          "xdmEventEnvelope:objectType": "xdmComponentizedPage",
          "activitystreams:published": "2016-07-16T19:20:30+01:00",
          "activitystreams:to": {
            "xdmImsOrg:id": "08B3E5CE5822FC520A494229@AdobeOrg",
            "@type": "xdmImsOrg"
          },
          "activitystreams:generator": {
            "xdmContentRepository:root": "https://cloud-action-dev.corp.adobe.com:4502/",
            "@type": "xdmContentRepository"
          },
          "activitystreams:actor": {
            "xdmAemUser:id": "admin",
            "@type": "xdmAemUser"
          },
          "activitystreams:object": {
            "@id": "http://adobesummit.adobesandbox.com:4502/content/geometrixx/en/vintage.html",
            "@type": "xdmComponentizedPage",
            "xdmComponentizedPage:title": "Vintage Collection",
            "xdmComponentizedPage:path": "/content/geometrixx/en/vintage.html"
          },
          "@context": {
            "activitystreams": "http://www.w3.org/ns/activitystreams#",
            "xdmEventEnvelope": "https://ns.adobe.com/xdm/common/eventenvelope#",
            "xdmComponentizedPage": "https://ns.adobe.com/xdm/content/componentized-page#",
            "xdmImsOrg": "https://ns.adobe.com/xdm/ims/organization#",
            "xdmContentRepository": "https://ns.adobe.com/xdm/content/repository#",
            "xdmAemUser": "https://ns.adobe.com/xdm/aem/user#",
            "xdmPublished": "https://ns.adobe.com/xdm/common/event/published#"
          }
        }
        ```

**During installation, my health check is failing. What should I do?**  
One possible solution is to try waiting for two minutes and checking a few more times. 
Sometimes the first health check fails even though the connection is actually working. 

If your health check consistently fails, check the [AEM Events](debug#aem-events) section of the Debugging Guide: &ldquo;AEM Events not firing&rdquo;.

## Analytics Triggers Events
**Where can I find instructions on setting up Analytics Triggers for I/O?**  
You'll find it in this guide at [Integrate Analytics Triggers with Adobe I/O Events](../using/analytics-triggers-event-setup.md). 

**Where do I configure Analytics Triggers for I/O?**  
Analytics Triggers are set up and managed via the Experience Cloud Activation UI. Once a Trigger has been created, it will appear in Adobe I/O Console under the available I/O Events list.

**What does an Analytics Triggers payload look like?**  
Here is a sample:

```json
{
  "event_id": "52ebf673-8aeb-4347-8852-bf86a18292e4",
  "event": {
    "envelopeType": "DATA",
    "partition": 13,
    "offset": 438465548,
    "createTime": 1516324157242,
    "topic": "triggers",
    "com.adobe.mcloud.pipeline.pipelineMessage": {
      "header": {
        "messageType": "TRIGGER",
        "source": "triggers",
        "sentTime": 1516324157228,
        "imsOrg": "C74F69D7594880280A495D09@AdobeOrg",
        "properties": [
          {
            "name": "trace",
            "value": "false"
          },
          {
            "name": "sourceFirstTimestamp",
            "value": "1516324106"
          },
          {
            "name": "sourceLastTimestamp",
            "value": "1516324128"
          },
          {
            "name": "triggerFiredTimestamp",
            "value": "1516324153995"
          }
        ],
        "messageId": "1a69fc40-7600-4928-b7bb-d66588a045f3"
      },
      "com.adobe.mcloud.protocol.trigger": {
        "triggerId": "697514a8-3337-4efc-ba75-1f0ba896c288",
        "triggerTimestamp": 1516324157228,
        "mcId": "00000000000000000000000000000000000000",
        "enrichments": {
          "analyticsHitSummary": {
            "dimensions": {
              "eVar3": {
                "type": "string",
                "data": [
                  "localhost:4502/content/we-retail.html",
                  "localhost:4502/content/we-retail/us/en/men.html",
                  "localhost:4502/content/we-retail.html",
                  "localhost:4502/content/we-retail/us/en.html",
                  "localhost:4502/content/we-retail/us/en.html",
                  "localhost:4502/content/we-retail/us/en/products/men/shirts/eton-short-sleeve-shirt.html",
                  "localhost:4502/content/we-retail/us/en/products/men/shirts/eton-short-sleeve-shirt.html",
                  "localhost:4502/content/we-retail/us/en/men.html",
                  "localhost:4502/content/we-retail/us/en/user/cart.html"
                ],
                "name": "eVar3",
                "source": "session summary"
              },
              "pageURL": {
                "type": "string",
                "data": [
                  "http://localhost:4502/content/we-retail.html",
                  "",
                  "",
                  "http://localhost:4502/content/we-retail/us/en.html",
                  "",
                  "",
                  "http://localhost:4502/content/we-retail/us/en/products/men/shirts/eton-short-sleeve-shirt.html",
                  "http://localhost:4502/content/we-retail/us/en/men.html",
                  "http://localhost:4502/content/we-retail/us/en/user/cart.html"
                ],
                "name": "pageURL",
                "source": "session summary"
              }
            },
            "products": {}
          }
        },
        "triggerPath": [
          {
            "timestamp": 1516324118010,
            "stateId": "start_and_and",
            "transition": "null"
          },
          {
            "timestamp": 1516324148711,
            "stateId": "vmi_and_1",
            "transition": "conditional -> select * where evars.evars.eVar3 like 'localhost:4502/content/we-retail/us/en/user/cart.html'"
          },
          {
            "timestamp": 1516324148711,
            "stateId": "notify_wait",
            "transition": "states visited -> [StateVisitedNode [stateId=vmi_and_1, count=1, operator=GE]]"
          },
          {
            "timestamp": 1516324153994,
            "stateId": "notify",
            "transition": "inactive_timeout -> 5"
          }
        ]
      }
    }
  }
```
  
**I receive errors trying to access Triggers.**  
The company/org is entitled for Analytics Triggers but I receive the following errors when trying to set up a Trigger:

![Report Suite null](../../img/events_FAQ_01.png "Report Suite null")

![Error fetching Report Suites](../../img/events_FAQ_02.png "Error fetching Report Suites")

**To fix:**  Ensure that Triggers is enabled in the Analytics Product Profile in the Admin Console. 

![Enabling Triggers in Admin Console](../../img/events_FAQ_03.png "Enabling Triggers in Admin Console")
