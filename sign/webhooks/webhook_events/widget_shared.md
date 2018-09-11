# Payload: WIDGET\_SHARED

## Payload attributes

### Event-specific: 

<table>
  <thead>
    <tr>
      <th><strong>Parameter name</strong></th>
      <th><strong>Type</strong></th>
      <th><strong>Description</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>participantUserId</code></td>
      <td>String</td>
      <td rowspan="2"><p>Details (user ID and email) of the user <strong>to whom the widget is shared.</strong></p></td>
    </tr>
    <tr>
      <td><code>participantUserEmail</code></td>
      <td>String</td>
    </tr>
    <tr>
      <td><code>actingUserId</code></td>
      <td>String</td>
      <td rowspan="2"><p>Details (user ID and email) of the user <strong>by whom the widget is shared.</strong> This will be creator of the widget.</p></td>
    </tr>
    <tr>
      <td><code>actingUserEmail</code></td>
      <td>String</td>
    </tr>
    <tr>
      <td><code>initiatingUserId</code></td>
      <td>String</td>
      <td rowspan="2"><p>Details (user ID and email) of the <strong>sharee of the user</strong> on whose behalf this widget is shared in the case of account sharing.</p></td>
    </tr>
    <tr>
      <td><code>initiatingUserEmail</code></td>
      <td>String</td>
    </tr>
  </tbody>
</table>

### Inherted from Widget:
	
<table>
  <thead>
    <tr>
      <th><strong>Parameter name</strong></th>
      <th><strong>Type</strong></th>
      <th><strong>Description</strong></th>
      <th><strong>Possible enums</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>id</code></td>
      <td>String</td>
      <td>The unique identifier of widget, which can be used to retrieve the data entered by the signers.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>name</code></td>
      <td>String</td>
      <td>The name of the widget that will be used to identify it, in emails, website, and other places.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>status</code></td>
      <td>Enum</td>
      <td>The current status of the widget.</td>
      <td><code>DRAFT</code>, <code>AUTHORING</code>, <code>ACTIVE</code>, <code>DOCUMENTS_NOT_YET_PROCESSED</code>, <code>DISABLED</code>, <code>DISCARDED</code></td>
    </tr>
    <tr>
      <td><code>ccs</code></td>
      <td>Array of Strings</td>
      <td>Email IDs of cc: participants of the widget.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>authFailureInfo</code></td>
      <td>Object</td>
      <td>URL and associated properties for the error page  to which the widget signer will be taken after failing to authenticate.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>completionInfo</code></td>
      <td>Object</td>
      <td>URL and associated properties for the success page to which the widget signer will be taken after performing the desired action on the widget.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>disabledWidgetOptions</code></td>
      <td>Object</td>
      <td>Specifies the custom message that will be displayed to the user or the URL to which the user will be redirected when the widget is accessed in a disabled state.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>creatorEmail</code></td>
      <td>String</td>
      <td>Email of of the widget creator.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>createdDate</code></td>
      <td>Date</td>
      <td>Widget creation date.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>locale</code></td>
      <td>String</td>
      <td>Locale associated with this widget: specifies the language for the signing page and emails; for example <code>en_US</code> or <code>fr_FR</code>. If none is specified, this defaults to the language configured for the widget creator.<br></td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>vaultingInfo</code></td>
      <td>Object</td>
      <td>Specifies the vaulting properties that allow Adobe Sign to securely store documents with a vault provider.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>participantSetsInfo</code></td>
      <td>Object</td>
      <td>List of all the participants in the widget except the widget signer. Returned only if the conditional parameter <code>includeParticipantInfo</code> is set to true and the payload size is less than the threshold.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>widgetParticipantSet</code></td>
      <td>Object</td>
      <td>Represents widget participant info for whom email was not provided.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>documentsInfo</code></td>
      <td>Object</td>
      <td>Retrieves the IDs of the documents associated with widget. Returned only if the conditional parameter <code>includeDocumentsInfo</code> is set to true and payload size is less than the threshold</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>conditionalParametersTrimmed</code></td>
      <td>Array of Strings</td>
      <td>If event notification payload size exceeds the defined threshold, the conditional parameters will not be sent in the notification request, even if they are set to true by the webhook creator. The <code>conditionalParametersTrimmed</code> attribute will be set to the keys trimmed in this case. If no conditional parameters are specified by the webhook creator, or if they are specified, but no key is trimmed, this parameter will not be returned.</td>
    </tr>
  </tbody>
</table>

## Payload template

```json
{  
   "webhookId":"",
   "webhookName":"",
   "webhookNotificationId":"",
   "webhookUrlInfo":{  
      "url":""
   },
   "webhookScope":"",
   "webhookNotificationApplicableUsers" :[  
    {  
      "id":"", 
      "email":"", 
      "role":"", 
      "payloadApplicable":"" 
     } 
   ], 
   "event":"",
   "subEvent":"",
   "eventDate":"", 
   "eventResourceType":"",
   "participantUserId":"",
   "participantUserEmail":"",
   "actingUserId":"",
   "actingUserEmail":"",
   "actingUserIpAddress":"", 
   "initiatingUserId":"",
   "initiatingUserEmail":"",
   "widget":{  
      "name":"",
      "status":"",
      "id":"",
      "authFailureInfo":{  
         "url":"",
         "deframe":"",
         "delay":""
      },
      "ccs":[  
         {  
            "email":""
         }
      ],
      "completionInfo":{  
         "url":"",
         "deframe":"",
         "delay":""
      },
      "disabledWidgetOptions":{  
         "message":"",
         "redirectUrl":""
      },
      "creatorEmail":"",
      "createdDate":"",
      "locale":"",
      "vaultingInfo":{  
         "enabled":""
      },
      "participantSetsInfo":{  
         "additionalParticipantSets":[  
            {  
               "memberInfos":[  
                  {  
                     "id":"",
                     "email":"",
                     "company":"",
                     "name":"",
                     "privateMessage":"",
                     "status":""
                  }
               ],
               "order":"",
               "role":"",
               "id":""
            }
         ],
         "widgetParticipantSet":{  
            "memberInfos":[  
               {  
                  "id":"",
                  "email":"",
                  "company":"",
                  "name":"",
                  "privateMessage":"",
                  "status":""
               }
            ],
            "order":"",
            "role":"",
            "id":""
         }
      },
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