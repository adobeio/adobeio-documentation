# Send reminders

A signing reminder can be sent to all the signers if they have not signed the agreement. When you send a reminder, the signers will get the same notification email that was originally sent.

![Sending a reminder](../img/sign_devguide_3.png)

```http
POST /api/rest/v5/reminders HTTP/1.1
Host: api.na1.echosign.com
Access-Token: 3AAABLblNOTREALTOKENLDaV
Content-Type: application/json
{
    "agreementId":"3AAABNotRealAgreementID"
}
```

Note that you need to provide the `agreementId` in the request header. You will get the following response from the server:

```json
{
  "participantEmailsSet":[
    {
      "participantEmailSetInfo":[
        {
          "participantEmail":"SIGNERG@COMPANY.COM"
        }
      ]
    }
  ],
  "result":"REMINDER_SENT"
}
```

[TRY IT OUT](https://secure.na1.echosign.com/public/docs/restapi/v5#!/reminders/)

