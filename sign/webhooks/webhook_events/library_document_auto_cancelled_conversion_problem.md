# Payload: LIBRARY\_DOCUMENT\_AUTO\_CANCELLED\_CONVERSION\_PROBLEM

## Payload attributes

### Event-specific:

<table>
  <thead>
    <tr>
      <th>Parameter name</th>
      <th>Type</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>participantUserId</code></td>
      <td>String</td>
      <td rowspan="2">Details (user ID and email) of the <strong>creator of the library document.</strong></td>
    </tr>
    <tr>
      <td><code>participantUserEmail</code></td>
      <td>String</td>
    </tr>
    <tr>
      <td><code>actingUserId</code></td>
      <td>String</td>
      <td rowspan="2">Details (user ID and email) of the <strong>creator of the library document.</strong></td>
    </tr>
    <tr>
      <td><code>actingUserEmail</code></td>
      <td>String</td>
    </tr>
    <tr>
      <td><code>initiatingUserId</code></td>
      <td>String</td>
      <td rowspan="2">Both these fields will never be part of the payload.</td>
    </tr>
    <tr>
      <td><code>initiatingUserEmail</code></td>
      <td>String</td>
    </tr>
  </tbody>
</table>

### Inherited from Library Document:
	
| Parameter name | Type | Description | Possible enums |
|---|---|---|---|
| `id` | String | The unique identifier that is used to refer to the library template. |   |
| `name` | String | The name of the library template that will be used to identify it, in emails and on the website. |   |
| `creatorEmail` | String | Email of the creator of the library document. |   |
| `createdDate` | Date | Library document creation date. |   |
| `status` | Enum | The current status of the library document. | `AUTHORING`, `ACTIVE`, or `REMOVED` |
| `sharingMode` | String | Specifies who should have access to this library document. | `USER`, `GROUP`, `ACCOUNT`, or `GLOBAL` |
| `templateTypes` | String | A list of one or more library template types. | `DOCUMENT` or `FORM_FIELD_LAYER` |
| `conditionalParametersTrimmed` | String[] | If the event notification payload size exceeds the defined threshold, the conditional parameters will not be sent in the notification request, even if they are set to true by the webhook creator. The `conditionalParametersTrimmed` parameter will be set to the keys trimmed in this case. If no conditional parameters are specified by the webhook creator, or if they are specified, but no key is trimmed, this parameter will not be returned. |

## Payload template

```json
{  
    "webhookId":"",
    "webhookName":"",
    "webhookNotificationId":"",
    "webhookUrlInfo": {  
       "url":""
    },
    "webhookScope":"",
    "webhookNotificationApplicableUsers":[  
        {  
            "id":"",
            "email":"",
            "role":"",
            "payloadApplicable":""
        }
    ] ,
    "event":"",
    "subEvent":"",
    "eventDate":"",
    "eventResourceType":"",
    "eventResourceParentType":"",
    "eventResourceParentId":"",
    "participantUserId":"",
    "participantUserEmail":"",
    "actingUserId":"",
    "actingUserEmail":"",
    "actingUserIpAddress":"",
    "initiatingUserId":"",
    "initiatingUserEmail":"",
    "libraryDocument" : {  
        "id":"",
        "name":"",
        "creatorEmail":"",
        "createdDate":"",
        "status":"",
        "sharingMode":"",
        "templateTypes":[  
            ""
        ]
    }
}
```