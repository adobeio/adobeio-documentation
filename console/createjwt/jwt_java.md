# Creating a JWT for a Java App

This example illustrates how to create a JSON Web Token for a Java-language application, using the library io.jsonwebtoken.jjwt. The library is available from https://github.com/jwtk/jjwt.

## Encode Private Key

The private key from your signing certificate must be DER-encoded before using it to sign the JWT. The following commands create a self-signed certificate, and encode the resulting private key for use with the example:

```shell
# create the certificate and private key using openssl
$ openssl req -nodes -text -x509 -newkey rsa:2048 -keyout secret.pem -out certificate.pem -days 356

# convert private key to DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in secret.pem  -nocrypt > secret.key
```

## Create JWT

```java
/**
 * Copyright 2016 Adobe Systems Incorporated
 * All Rights Reserved.
 */
 package com.adobe.apip.tools.jwt.service;

import io.jsonwebtoken.Jwts;
import java.io.IOException;
import java.nio.file.*;
import java.security.*;
import java.security.interfaces.RSAPrivateKey;
import java.security.spec.*;
import java.util.*;

import static io.jsonwebtoken.SignatureAlgorithm.RS256;
import static java.lang.Boolean.TRUE;

public class SampleJwtTest {

    public static void main(String[] args)
            throws NoSuchAlgorithmException, InvalidKeySpecException, IOException {

        // Sample JWT creation. The example uses SHA256withRSA signature algorithm.

        // API key information (substitute actual credential values)
        String orgId = "...000101@AdobeOrg";
        String technicalAccountId = "...801234033@techacct.adobe.com";
        String apiKey = "...0492c612344700abcd";

        // Set expirationTime in milliseconds since epoch to 10 minutes ahead of now
        Long expirationTime = System.currentTimeMillis() / 1000 + 600;

        // Metascopes associated to key
        String metascopes[] = new String[]{"ent_marketing_sdk"};

        // Secret key as byte array. Secret key file should be in DER encoded format.
        byte[] privateKeyFileContent = Files.readAllBytes(Paths.get("/path/to/secret/key"));

        String imsHost = "ims-na1.adobelogin.com";

        // Create the private key
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        KeySpec ks = new PKCS8EncodedKeySpec(privateKeyFileContent);
        RSAPrivateKey privateKey = (RSAPrivateKey) keyFactory.generatePrivate(ks);

        // Create JWT payload
        Map jwtClaims = new HashMap<>();
        jwtClaims.put("iss", orgId);
        jwtClaims.put("sub", technicalAccountId);
        jwtClaims.put("exp", expirationTime);
        jwtClaims.put("aud", "https://" + imsHost + "/c/" + apiKey);
        for (String metascope : metascopes) {
            jwtClaims.put("https://" + imsHost + "/s/" + metascope, TRUE);
        }

        // Create the final JWT token
        String jwtToken = Jwts.builder().setClaims(jwtClaims).signWith(RS256, privateKey).compact();

        System.out.println(jwtToken);
    }
}
```
