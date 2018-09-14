# Payload: LIBRARY\_DOCUMENT\_CREATED

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
      <td rowspan="2">Details (user ID and email) of the <strong>sharee of the creator of the library document</strong> who modified the library document on behalf of the creator in the case of account sharing.</td>
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
| `documentsInfo` | Object | Returns the IDs of the documents of a library document. Returned only if the conditional parameter `includeDocumentsInfo` is set to true and the payload size is less than the threshold. In some cases when document processing takes a lot of time, you might not get documentsInfo even if the conditional parameter `includeDocumentsInfo` was set to true, In such a case, try calling the v6 [GET /libraryDocuments/{libraryDocumentId}/documents](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/getLibraryDocumentInfo) API to get the details of the documents of a library document. |
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
        ],
       "documentsInfo":{  
          "documents":[  
             {  
                "id":"",
                "label":"",
                "numPages":"",
                "mimeType":"",
                "name":""
             }
          ]
       }
    }
}
```