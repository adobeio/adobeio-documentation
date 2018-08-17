# Send for Signing (Create an Agreement)

Your CRM system or document management system can send/upload documents for signing, either automatically or through user-initiated actions. When the document gets signed by all the parties, a PDF copy of the signed document(agreement) can be retrieved by your application.

![Sending a document for signing](../img/sign_devguide_1.png)

While getting the authorization code from the Adobe Sign service, you also got the API access point as part of a query parameter (see the  [Getting Started section](../gstarted/get_access_token.md)):

```http
https://myserver.com/?  
    code=CBNCKBAThIsIsNoTaReAlcs_sL4K32wCzs4N&
    api_access_point=https://api.na1.echosign.com/&
    web_access_point=https://secure.na1.echosign.com/
```

For all future service calls, we will be sending the requests to this access point.

## Upload a document
To upload a PDF document for signing, send a POST request to the `transientDocuments` endpoint. This is a multipart request consisting of filename, MIME type, and the file stream. You will get back an ID as a response that uniquely represents the document. Your application needs to specify the recipients and other sending options required for sending the document for signing. Your application can also specify a callback URL that will be used by Adobe Sign to notify when the signature process is complete.

```http
POST /api/rest/v6/transientDocuments HTTP/1.1
Host: api.na1.echosign.com
Authorization: Bearer MvyABjNotARealTokenHkYyi
Content-Type: multipart/form-data
Content-Disposition: form-data; name=";File"; filename="MyPDF.pdf"

<PDF CONTENT>
```

You will get the following JSON body containing the `transientDocumentId` that will uniquely represent the uploaded document:

```json
{
    "transientDocumentId":"3AAABLblqZhBVYbgJbl--NotArEaLID_zjaBYK"
}
```

The document uploaded through this call is termed as a _transient document_ since it is available only for 7 days after the upload.

You can only upload one file at a time through this request.

[TRY IT OUT](https://secure.na1.echosign.com/public/docs/restapi/v6#!/transientDocuments/createTransientDocument)

## Send the document
Once you have uploaded the document, send the document to all the related parties for signing. For this to happen, you need to create an _agreement._

For creating an agreement, send a POST request to the `/agreements` endpoint with the following JSON body:

```json
POST /api/rest/v6/agreements HTTP/1.1
Host: api.na1.echosign.com
Authorization: Bearer 3AAABLblNOTREALTOKENLDaV
Content-Type: application/json

{
    "fileInfos": [{
        "transientDocumentId": "<copy-transient-from-the-upload-document-step>"
    }],
    "name": "MyTestAgreement",
    "participantSetsInfo": [{
        "memberInfos": [{
            "email": "signer@somecompany.com"
        }],
        "order": 1,
        "role": "SIGNER"
    }],
    "signatureType": "ESIGN",
    "state": "IN_PROCESS"
}
```

Replace the value for the following attributes with the correct values:

| **Attribute** | **Description** |
|---|---|
| `transientDocumentId` | The unique ID representing the uploaded document. |
| `name` | The name of the agreement. |
| `email` | Recipient&rsquo;s email address. |
| `signatureType` | The type of signature you would like to request. The possible values are `ESIGN` and `WRITTEN`. |
| `order` | Index indicating the position at which this signing group needs to sign. Signing group to sign at first place is assigned 1 as index. |
| `role` | Role of the participant set. The possible values are: `SIGNER`, `APPROVER`, `ACCEPTOR`, `CERTIFIED_RECIPIENT`,  `FORM_FILLER` or `DELEGATE_TO_SIGNER`, `DELEGATE_TO_APPROVER`, `DELEGATE_TO_ACCEPTOR`, `DELEGATE_TO_CERTIFIED_RECIPIENT`, `DELEGATE_TO_FORM_FILLER`, or `SHARE`. |
| `state` | The state in which the agreement should land. The possible values are `AUTHORING`, `DRAFT`, or `IN_PROCESS`. You can use a) `DRAFT` to incrementally build the agreement before sending out, b) `AUTHORING` to add or edit form fields in the agreement, c) `IN_PROCESS` to immediately send the agreement. You can use the `PUT /agreements/{agreementId}/state` endpoint to transition an agreement between the above-mentioned states. An allowed transition would follow this sequence: `DRAFT` -&gt; `AUTHORING` -&gt; `IN_PROCESS` -&gt; `CANCELLED`.


You will get the following response containing the `id`:

```json
{
    "id": "<an-adobe-sign-generated-id>"
}
```

The returned `agreementId` must be used to refer to the agreement in all subsequent API calls. This ID must be used to retrieve up-to-date status of the agreement, either by polling or when Adobe Sign notifies your application of any status change.

[TRY IT OUT](https://secure.na1.echosign.com/public/docs/restapi/v6#!/agreements/) 