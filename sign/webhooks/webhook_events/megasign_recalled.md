# Payload: MEGASIGN\_RECALLED

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
      <td rowspan="2"><p>Details (user ID and email) of the <strong>creator of the MegaSign.</strong></p></td>
    </tr>
    <tr>
      <td><code>participantUserEmail</code></td>
      <td>String</td>
    </tr>
    <tr>
      <td><code>actingUserId</code></td>
      <td>String</td>
      <td rowspan="2"><p>Details (user ID and email) of the <strong>creator of the MegaSign.</strong></p></td>
    </tr>
    <tr>
      <td><code>actingUserEmail</code></td>
      <td>String</td>
    </tr>
    <tr>
      <td><code>initiatingUserId</code></td>
      <td>String</td>
      <td rowspan="2">Details (user ID and email) of the <strong>sharee of the creator of the MegaSign</strong> who recalled the MegaSign on behalf of the creator in the case of account sharing.</td>
    </tr>
    <tr>
      <td><code>initiatingUserEmail</code></td>
      <td>String</td>
    </tr>
  </tbody>
</table>

### Inherted from MegaSign:
	
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
      <td>The unique identifier of the MegaSign parent agreement.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>name</code></td>
      <td>String</td>
      <td>The name of the agreement that will be used to identify it in emails, on the website, and elsewhere.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>status</code></td>
      <td>Enum</td>
      <td>State of the MegaSign.</td>
      <td>IN_PROCESS</td>
    </tr>
    <tr>
      <td><code>ccs</code></td>
      <td>Array of Strings</td>
      <td>Email IDs of cc: participants of the MegaSign</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>childAgreementsInfo</code></td>
      <td>Object</td>
      <td>Info corresponding to each child agreement of the MegaSign</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>createdDate</code></td>
      <td>Date</td>
      <td>Date when the MegaSign was created, in the format <code>yyyy-MM-dd'T'HH:mm:ssZ</code></td>
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
      <td>A unique identifier provided by an external system search for your transaction through API.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>firstReminderDelay</code></td>
      <td>integer</td>
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
      <td>The message associated with the agreement that the sender has provided, describing what is being sent or why their signature is required.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>postSignOption</code></td>
      <td>Object</td>
      <td>Determines the URL and associated properties for the success page to which the user will be taken after completing the signing process.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>reminderFrequency</code></td>
      <td>Enum</td>
      <td>Specifies how often reminders will be sent to the recipients.</td>
      <td>DAILY_UNTIL_SIGNED or WEEKLY_UNTIL_SIGNED</td>
    </tr>
    <tr>
      <td><code>senderEmail</code></td>
      <td>String</td>
      <td>Email of the sender.</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <td><code>signatureType</code></td>
      <td>Enum</td>
      <td>Specifies the type of signature requested on the agreement&mdash;written or e-signature.</td>
      <td>'ESIGN' or 'WRITTEN'</td>
    </tr>
    <tr>
      <td><code>vaultingInfo</code></td>
      <td>Object</td>
      <td>Specifies the vaulting properties that allow Adobe Sign to securely store documents with a vault provider.</td>
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
   "megaSign": {
      "name":"",
      "status":"",
      "id":"",
      "ccs": [
         {
            "email":""
         }
      ],
      "childAgreementsInfo": {
         "fileInfo": {
            "fileType":"",
            "childAgreementsInfoFileId":""
         }
      },
      "createdDate":"",
      "expirationTime":"",
      "externalId": {
         "id":""
      },
      "firstReminderDelay":"",
      "locale":"",
      "message":"",
      "postSignOption": {
         "redirectDelay":"",
         "redirectUrl":""
      },
      "reminderFrequency":"",
      "senderEmail":"",
      "signatureType":"",
      "vaultingInfo": {
         "enabled":""
      }
   }
}
```