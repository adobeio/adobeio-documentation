# Get the Signing URL
 
When the agreement is ready for signing, invoke `GET /agreements/{agreementId}/signingUrls` to get the signing URL:

```http
GET /api/rest/v6/agreements/3AANotRealIDQN8_gg/signingUrls HTTP/1.1
Host: api.na1.echosign.com
Authorization: Bearer 3AAABLblqZNotRelaTOKEN
```

You will get the following JSON response containing the signing URL:

```json
{
  "signingUrlSetInfos": [
    {
      "signingUrls": [
        {
          "email": "FJ@MYCOMPANY.COM",
          "esignUrl": "https://secure.na1.echosign.com/public/apiesign?pid=CBFNotTheRealIDw3w*"
        }
      ]
    }
  ]
}
```
    
Getting the signing URL becomes useful for scenarios involving in-person signing. Load the signing URL in a browser window on a mobile device and get the agreement signed in-person.

[TRY IT OUT](https://secure.na1.echosign.com/public/docs/restapi/v6#!/agreements/_0_1_2_3_4)

