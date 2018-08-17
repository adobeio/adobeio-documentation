# Send Reminders
 
A signing reminder can be sent to all the signers if they have not signed the agreement. When you send a reminder, the signers will get the same notification email that was originally sent.

![Sending a reminder](../img/sign_devguide_3.png)

```http
POST /api/rest/v6/agreements/{agreementId}/reminders HTTP/1.1
Host: api.na1.echosign.com
Authorization: Bearer 3AAABLblNOTREALTOKENLDaV
Content-Type: application/json
{
  "recipientParticipantIds": [
    "<id of a participant>."
  ],
  "nextSentDate": "< The date when the reminder is scheduled to be sent next.>",
  "status": "< valid status of reminder (ACTIVE)>"
}
```

Note that you need to provide the `agreementId` in the request URL. You will get the following response from the server:

```json
{
   id: <An identifier of the reminder resource created on the server>
}
```

[TRY IT OUT](https://secure.na1.echosign.com/public/docs/restapi/v6#!/agreements/createReminderOnParticipant)

