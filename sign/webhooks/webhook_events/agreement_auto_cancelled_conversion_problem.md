# Payload: AGREEMENT\_AUTO\_CANCELLED\_CONVERSION\_PROBLEM

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
      <td rowspan="2"><p>Details (user ID and email) of the <strong>sender or originator of the agreement.</strong></p>
        <p>In case the agreement is created by signing a widget, then this will be details of the <strong>creator of the widget.</strong></p></td>
    </tr>
    <tr>
      <td><code>participantUserEmail</code></td>
      <td>String</td>
    </tr>
    <tr>
      <td><code>actingUserId</code></td>
      <td>String</td>
      <td rowspan="2"><p>Details (user ID and email) of the <strong>sender or originator of the agreement.</strong></p>
        <p>In case the agreement is created by signing a widget, then this will be details of the <strong>creator of the widget.</strong></p></td>
    </tr>
    <tr>
      <td><code>actingUserEmail</code></td>
      <td>String</td>
    </tr>
    <tr>
      <td><code>initiatingUserId</code></td>
      <td>String</td>
      <td rowspan="2"><p>Both these fields will never be part of the payload.</p></td>
    </tr>
    <tr>
      <td><code>initiatingUserEmail</code></td>
      <td>String</td>
    </tr>
  </tbody>
</table>

### Inherited from Agreement:

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
      <td>The unique identifier of the agreement, which can be used to query status and download signed documents.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>name</code></td>
      <td>String</td>
      <td>The name of the agreement that will be used to identify it, in emails and on the website.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>signatureType</code></td>
      <td>Enum</td>
      <td>Specifies the type of signature requested on the agreement&mdash;written or e-signature.<br></td>
      <td><code>ESIGN</code>, <code>WRITTEN</code></td>
    </tr>
    <tr>
      <td><code>status</code></td>
      <td>Enum</td>
      <td>The current status of the agreement.</td>
      <td><code>OUT_FOR_SIGNATURE</code>, <code>OUT_FOR_DELIVERY</code>, <code>OUT_FOR_ACCEPTANCE</code>, <code>OUT_FOR_FORM_FILLING</code>, <code>OUT_FOR_APPROVAL</code>, <code>AUTHORING</code>, <code>CANCELLED</code>, <code>SIGNED</code>, <code>APPROVED</code>, <code>DELIVERED</code>, <code>ACCEPTED</code>, <code>FORM_FILLED</code>, <code>EXPIRED</code>, <code>ARCHIVED</code>, <code>PREFILL</code>, <code>WIDGET_WAITING_FOR_VERIFICATION</code>, <code>DRAFT</code>, <code>DOCUMENTS_NOT_YET_PROCESSED</code>, <code>WAITING_FOR_FAXING</code>, <code>WAITING_FOR_VERIFICATION</code></td>
    </tr>
    <tr>
      <td><code>ccs</code></td>
      <td>Array of Strings</td>
      <td>Email IDs of cc: participants of the agreement.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>deviceInfo</code></td>
      <td>Object</td>
      <td>Device info of the offline device. It will only be returned in the case of offline agreement creation.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>documentVisibilityEnabled</code></td>
      <td>Boolean</td>
      <td>Determines whether limited document visibility is enabled or not. Will never be returned in offline agreement creation.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>createdDate</code></td>
      <td>Date</td>
      <td>Agreement creation date.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>expirationTime</code></td>
      <td>Timestamp</td>
      <td>The date after which the agreement can no longer be signed, if an expiration date is configured. The value is nil if an expiration date is not set for the document.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>externalId</code></td>
      <td>Object</td>
      <td>A unique identifier provided by an external system search for your transaction through the API.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>postSignOption</code></td>
      <td>Object</td>
      <td>Determines the URL and associated properties for the success page the user will be taken to after completing the signing process. Will never be returned in offline agreement creation.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>firstReminderDelay</code></td>
      <td>Integer</td>
      <td>Integer which specifies the delay in hours before sending the first reminder. The minimum value allowed is 1 hour and the maximum value can&rsquo;t be more than the difference between the agreement creation time and the expiration time of the agreement in hours. If this is not specified while creating the agreement, but the reminder frequency is specified, then the first reminder will be sent based on frequency: in other words, if the reminder is created with frequency specified as daily, the <code>firstReminderDelay</code> will be 24 hours. Will never be returned in offline agreement creation.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>locale</code></td>
      <td>String</td>
      <td>The locale associated with this agreement. Specifies the language for the signing page and emails: for example, <code>en_US</code> or <code>fr_FR</code>. If none specified, defaults to the language configured for the agreement sender.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>message</code></td>
      <td>String</td>
      <td>The message associated with the agreement that the sender has provided.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>reminderFrequency</code></td>
      <td>Enum</td>
      <td>Specifies how often reminders will be sent to the recipients.</td>
      <td><code>DAILY_UNTIL_SIGNED</code>, <code>WEEKLY_UNTIL_SIGNED</code></td>
    </tr>
    <tr>
      <td><code>senderEmail</code></td>
      <td>String</td>
      <td>Email of the sender.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>vaultingInfo</code></td>
      <td>Object</td>
      <td>Specifies the vaulting properties that allow Adobe Sign to securely store documents with a vault provider.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>workflowId</code></td>
      <td>String</td>
      <td>Identifier of the custom workflow that defines the routing path of an agreement. Will not be returned in offline agreement creation.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>participantSetsInfo</code></td>
      <td>Object</td>
      <td>Returns a list of one or more participant sets. A participant set may have one or more participants. Returned only if conditional parameter <code>includeParticipantInfo</code> is set to true and the payload size is less than the threshold.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>nextParticipantSets</code></td>
      <td>Object</td>
      <td>Information about the next participant sets.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>documentsInfo</code></td>
      <td>Object<br></td>
      <td>Returns the IDs of the documents of an agreement. Returned only if conditional parameters <code>includeDocumentsInfo</code> is set to true and payload size is less than the threshold.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>supportingDocuments</code></td>
      <td>Object</td>
      <td>Returns the supporting documents of an agreement. Returned only if the conditional parameter <code>includeDocumentsInfo</code> is set to true and payload size is less than the threshold. </td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>conditionalParametersTrimmed</code></td>
      <td>Array of Strings</td>
      <td>If event notification payload size exceeds the defined threshold, the conditional parameters will not be sent in the notification request, even if they are set to true by the webhook creator. The <code>conditionalParametersTrimmed</code> attribute will be set to the keys trimmed in this case. If no conditional parameters are specified by the webhook creator, or if they are specified, but no key is trimmed, this parameter will not be returned.</td>
      <td>&nbsp;</td>
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
   "webhookNotificationApplicableUsers":[  
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
   "eventResourceParentType":"",
   "eventResourceParentId":"", 
   "participantUserId":"",
   "participantUserEmail":"",
   "actingUserId":"",
   "actingUserEmail":"",
   "actingUserIpAddress":"",
   "initiatingUserId":"",
   "initiatingUserEmail":"",
   "agreement":{  
      "id":"",
      "name":"",
      "signatureType":"",
      "status":"",
      "ccs":[  
         {  
            "email":"",
            "label":"",
            "visiblePages":[  
               ""
            ]
         }
      ],
      "deviceInfo":{  
         "applicationDescription":"",
         "deviceDescription":"",
         "location":{  
            "latitude":"",
            "longitude":""
         },
         "deviceTime":""
      },
      "documentVisibilityEnabled":"",
      "createdDate":"",
      "expirationTime":"",
      "externalId":{  
         "id":""
      },
      "postSignOption":{  
         "redirectDelay":"",
         "redirectUrl":""
      },
      "firstReminderDelay":"",
      "locale":"",
      "message":"",
      "reminderFrequency":"",
      "senderEmail":"",
      "vaultingInfo":{  
         "enabled":""
      },
      "workflowId":"",
      "participantSetsInfo":{  
         "participantSets":[  
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
               "status":"",
               "id":"",
               "name":"",
               "privateMessage":""
            }
         ]
      }
   }
}
```