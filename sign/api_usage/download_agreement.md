# Download the Agreement
 
Once an agreement is signed, your application can retrieve the signed copy of the PDF and store that within your application.

![download the agreement](../img/sign_devguide_4.png)

The signed agreement can also be retrieved by sending a GET request to `/agreements/{agreementId}/combinedDocument`. This will return a single combined PDF document for the documents associated with the agreement. To retrieve any supporting document, you can send a GET request to `/agreements/{agreementId}/documents`. This will return the IDs of all the main and supporting documents of an agreement.

The returned document ID can be used in the `/agreements/{agreementId}/documents/{documentId}` call to retrieve the file stream of a document of the agreement. Depending on your application, you can also retrieve the form field data that your signer may have filled in to the document when signing the document by sending a GET request to `/agreements/{agreementId}/formData`. The data can be used to update your calling application with the information provided by the signer during signing.

Send the following GET request to retrieve the signed agreement:

```http
GET /api/rest/v6/agreements/3AAA5NOTREALIDiH/combinedDocument HTTP/1.1
Host: api.na1.echosign.com:443
Authorization: Bearer 3AAABLblqZhB9BF
```

The response body will contain the content of the PDF file, which you can save locally through your application.

Note that the agreement can be downloaded even before it gets signed. Provide the `versionId` attribute when invoking `GET /agreements/{agreementId}/combinedDocument/` to get the correct version of the agreement. For example, when the agreement is sent to two entities for signing, and when only one entity signs, the document can still be downloaded. If the `versionId` is not specified, the document in the latest state is returned.

[TRY IT OUT](https://secure.na1.echosign.com/public/docs/restapi/v6#!/agreements/_0_1_2_3_4_5_6)

