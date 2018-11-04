# Creating a JWT for a Node.js App

This example illustrates how to create a JSON Web Token for a node.js application using the library jwt-simple. The node.js snippet instead sets the expiration time dynamically based on the current timestamp:

```javascript
var fs = require('fs');
var jwt = require('jsonwebtoken');

//replace with actual values
var jwtPayload = {
    "exp": Math.round(600 + Date.now() / 1000),             // Expiration set to 10 minutes
    "iss": "...000101@AdobeOrg",                            //org_id
    "sub": "...34033@techacct.adobe.com",                   //technical_account_id
    "https://ims-na1.adobelogin.com/s/ent_user_sdk": true,
    "aud": "https://ims-na1.adobelogin.com/c/ec9a209..."
};

var private_key = fs.readFileSync('./path/to/private_key').toString('ascii');

var token = jwt.sign(jwtPayload, private_key, {
    algorithm: 'RS256'
});
```
