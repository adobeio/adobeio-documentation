# Creating a JWT for a Node.js App

This example illustrates how to create a JSON Web Token for a node.js application using the library jwt-simple. The node.js snippet instead sets the expiration time dynamically based on the current timestamp:

```var fs = require('fs');
var jwt = require('./node_modules/jwt-simple');

var AdobeConfig = {
    payload: {
        'exp': Math.round(87000 + Date.now()/1000),
        'iss': '…@AdobeOrg',
        'sub': '…@techacct.adobe.com',
        'aud': 'https://ims-na1.adobelogin.com/c/…,
        'https://ims-na1.adobelogin.com/s/ent_smartcontent_sdk': true
    },
    clientid: 'API key here',
    clientsecret: 'client secret here',
    pem: '',
    algorithm: 'RS256'
};
 
AdobeConfig.pem = fs.readFileSync('./certificate/private-key.pem').toString('ascii');
 
var token = jwt.encode(AdobeConfig.payload, AdobeConfig.pem, AdobeConfig.algorithm);
```
