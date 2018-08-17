# Create a Widget
 
To create a widget through the API, you must first call /transientDocuments, then send a POST request to upload the document. This is a multipart request consisting of filename, MIME type, and the file stream. The returned `transientDocumentId` is to be used to refer to the document in the widget creation call (`/widgets`, POST). The API endpoint, in addition to the widget key, returns an embed-code, which can be used for embedding the widget within your application, as well as a URL at which the widget gets hosted. The URL can be posted within your application for users to navigate to for signing a document.

```http
POST /api/rest/v6/widgets HTTP/1.1
Host: api.na1.echosign.com
Authorization: Bearer 3AAABLblqZNotRelaTOKEN
Content-Type: application/json
{
    "name": "MyTestWidget",
    "widgetParticipantSetInfo": {
        "memberInfos": [{
            "email": ""
        }],
    "role": "A valid role of the widget signer (SIGNER/APPROVER)"
    },
    "state": "A valid state in which the widget should land (ACTIVE/AUTHORING/DRAFT)"
}
```

You will get the following JSON response:

```json
{
    id: <The unique identifier of widget which can be used to retrieve the data entered by the signers.>
}
```

Now, the Widget URL can be circulated to the parents for signing. At any time, to get information about the Widget, send a GET request to `/widgets/{widgetId}`.

```http
GET /api/rest/v6/widgets/3AAANotTheRealID6o HTTP/1.1
Host: api.na1.echosign.com
```

You will get a JSON response containing details about the widget, including participants&rsquo; information and status.

You can also send a GET request to `/widgets/{widgetId}/formData` to retrieve the data entered (by the parents) in the document when it got signed.

Each time a widget is signed by a person, a separate instance of a document gets created. To get the agreements created using the widget, call `/widgets/{widgetID}/agreements` GET where _widgetID_ is the key returned by the service while creating the widget. To retrieve the data filled by the users at the time of signing the widget, call `GET /widgets/{widgetID}/formData`. The service returns data in comma-separated value (CSV) file format. The first line includes the column header names, and each row represents a distinct instance of the widget.

[TRY IT OUT](https://secure.na1.echosign.com/public/docs/restapi/v6#!/widgets/)