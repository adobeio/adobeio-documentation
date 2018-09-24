<!--:navorder: 3-->

# Journaling API

- [Accessing the journaling endpoint](#accessingthejournalingendpoint)
- [Calling the API](#callingtheapi)
- [Getting the response](#gettingtheresponse)
- [Controlling the response](#controllingtheresponse)

For enterprise developers, Adobe offers another way to consume events besides webhooks: journaling. The Adobe I/O Events Journaling API enables enterprise integrations to consume events according to their own cadence and process them in bulk. Unlike webhooks, no additional registration or other configuration is required; every enterprise integration that is registered for events is automatically enabled for journaling. Journaling data is retained for 7 days.



Rather than webhooks, which are a _push_ model for events, journaling is a _pull_ model, in which the integration issues an API call to pull a list of events from Adobe. As with webhooks, Adobe delivers the event list as a JSON object on the following model: 

```json
[
  {
    "events": [
      "string"
    ],
    "next": "string"
  }
]
```

There is only one API for journaling:

`/events/organizations/{orgId}/integrations/{intId}/{registrationId}`

This API gets all events for a given event registration.

Make sure that the `I/O Management API` is added as a service in your integration (using the `Services` tab in the I/O Console), in order to be able to invoke the journaling API.

## Accessing the journaling endpoint

Adobe I/O Console makes it easy to use the API by providing you with an endpoint URL with the parameters filled in:

1. Log into [console.adobe.io](https://console.adobe.io) and open your integration. 

2. Select the Events tab. 

3. Under Event Providers, find the event registration for which you want to pull events and select View.

4. Find the Journaling section of the event details and copy the URL for the unique endpoint. 

## Calling the API

To issue the API call, you need to provide two additional parameters: 

* Your integration&rsquo;s API key. This is displayed in the Overview tab for your integration in the Adobe I/O Console.
* A JWT token. See [Authentication: Creating a JWT Token](https://www.adobe.io/apis/cloudplatform/console/authentication/createjwt.html) for how to create a JWT token.

You combine the URL you got from the Journaling section of the event details with your API key and JWT token to make the call

```
curl -H “Authorization: Bearer $USER_TOKEN” -H “x-api-key: $API_KEY
https://api.adobe.io/events/organizations/2316/integrations/5670/fa28f4d0-3438-429f-98b8-0a25cb49498b
```

## Getting the response
Your call results in a response containing a JSON object listing all the events for that event registration. 

**Sample output:**
```json
{
   "events":[
      {
         "event_id":"7dd9e3c4-0d3f-42d5-abb4-1776e209b080",
         "event":{
            "@id":"N/A",
            "@type":"xdmCreated",
            "activitystreams:published":"2018-03-01T17:54:14-08",
            "activitystreams:to":{
               "@type":"xdmImsOrg",
               "xdmImsOrg:id":"01DC1FC45956A5810A494138@AdobeOrg"
            },
            "activitystreams:generator":{
               "@type":"xdmContentRepository",
               "xdmContentRepository:root":"http://localhost-roberto-aem63:4502"
            },
            "activitystreams:actor":{
               "@id":"healthcheck-user",
               "@type":"xdmAemUser"
            },
            "activitystreams:object":{
               "@type":"xdmAsset",
               "xdmAsset:asset_name":"healthcheck.png",
               "xdmAsset:path":"/content/dam/healthcheck.png",
               "xdmAsset:format":"image/png"
            },
            "xdmEventEnvelope:objectType":"xdmAsset"
         }
      },
      {
         "event_id":"ff244403-ca7c-4993-bbda-3c8915ce0b32",
         "event":{
            "@id":"N/A",
            "@type":"xdmCreated",
            "activitystreams:published":"2018-03-06T15:19:03-08",
            "activitystreams:to":{
               "@type":"xdmImsOrg",
               "xdmImsOrg:id":"01DC1FC45956A5810A494138@AdobeOrg"
            },
            "activitystreams:generator":{
               "@type":"xdmContentRepository",
               "xdmContentRepository:root":"http://localhost-roberto-aem63:4502"
            },
            "activitystreams:actor":{
               "@id":"healthcheck-user",
               "@type":"xdmAemUser"
            },
            "activitystreams:object":{
               "@type":"xdmAsset",
               "xdmAsset:asset_name":"healthcheck.png",
               "xdmAsset:path":"/content/dam/healthcheck.png",
               "xdmAsset:format":"image/png"
            },
            "xdmEventEnvelope:objectType":"xdmAsset"
         }
      },
      {
         "event_id":"2159b72c-e284-4899-b572-08da250e3614",
         "event":{
            "@id":"N/A",
            "@type":"xdmCreated",
            "activitystreams:published":"2018-03-06T15:19:59-08",
            "activitystreams:to":{
               "@type":"xdmImsOrg",
               "xdmImsOrg:id":"01DC1FC45956A5810A494138@AdobeOrg"
            },
            "activitystreams:generator":{
               "@type":"xdmContentRepository",
               "xdmContentRepository:root":"http://localhost-roberto-aem63:4502"
            },
            "activitystreams:actor":{
               "@id":"healthcheck-user",
               "@type":"xdmAemUser"
            },
            "activitystreams:object":{
               "@type":"xdmAsset",
               "xdmAsset:asset_name":"healthcheck.png",
               "xdmAsset:path":"/content/dam/healthcheck.png",
               "xdmAsset:format":"image/png"
            },
            "xdmEventEnvelope:objectType":"xdmAsset"
         }
      }
   ],
   "next":""
}
```

## Controlling the response
By default, every call to the Journaling API returns a list of the latest 100 events, or all events if there are fewer than 100. The Journaling API offers two optional query parameters you can include in your URL for controlling the response:

* **pageSize:** This integer parameter lets you specify how many of the most recent events to retrieve. 
* **from:** This string parameter lets you provide the ID of the first event you want returned; the rest of events in the response will follow.

Adding these two parameters to the URL:

```
curl -H “Authorization: Bearer $USER_TOKEN” -H “x-api-key: $API_KEY” 
https://api.adobe.io/events/organizations/2316/integrations/5670/fa28f4d0-3438-429f-98b8-0a25cb49498b
?pageSize=25&from=2159b72c-e284-4899-b572-08da250e3614
```

This would return a list of 25 events, beginning with event ID 2159b72c-e284-4899-b572-08da250e3614.

### Using &ldquo;next&rdquo;
The `next` parameter is used in conjunction with the `from` parameter to get sequential sets of events. If you specify an event ID in `from` that, together with the number of events you retrieve (either the default 100 or the number specified by `pageSize`), produces a result set that doesn't include the most recent event, then the `next` attribute in the JSON payload gives the event ID of the next event following the set returned. (Conversely: if `next` is empty, you have the latest event.)

You can use the value of `next` to feed the `from` parameter in the next API call you make; by repeating this process, you can get sets of events in perfect sequence until you retrieve the most recent event. This way, you can process events in batches whose size you can control and never miss an event.
