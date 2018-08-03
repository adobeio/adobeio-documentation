# Download Documents from an Agreement

This sample client demonstrates how to to download documents from a specified agreement.

**IMPORTANT:**

1. Before running this sample, check that you have modified the JSON file OAuthCredentials.json with appropriate values. Which values need to be specified is indicated in the file.
2. You can also provide your OAuth access token in the `OAUTH_ACCESS_TOKEN` constant in the `RestApiOAuthTokens` class, which will then be used as the OAuth access token for making API calls.
3. You can also provide a refresh token in the `OAUTH_REFRESH_TOKEN constant` in the `RestApiOAuthTokens` class to refresh the OAuth access token.
4. Make sure that you have specified a valid agreement ID in `agreementId` below.
5. Check that the default output location in the field `OUTPUT_PATH` of FileUtils.java is suitable.

```java
/*************************************************************************
 * ADOBE SYSTEMS INCORPORATED
 * Copyright 2018 Adobe Systems Incorporated
 * All Rights Reserved.
 * 
 * NOTICE: Adobe permits you to use, modify, and distribute this file in accordance with the
 * terms of the Adobe license agreement accompanying it. If you have received this file from a
 * source other than Adobe, then your use, modification, or distribution of it requires the prior
 * written permission of Adobe.
 **************************************************************************/

package adobesign.api.rest.sample;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;

import adobesign.api.rest.sample.util.FileUtils;
import adobesign.api.rest.sample.util.RestApiAgreements;
import adobesign.api.rest.sample.util.RestApiOAuthTokens;
import adobesign.api.rest.sample.util.RestError;

 ```java
public class DownloadDocumentsOfAgreement {
  // TODO: Enter a valid agreement ID. Please refer to the "agreements" end-point in the API documentation to learn how to obtain IDs of
  // agreements present in Adobe Sign.
  private static String agreementId = "a valid agreement ID";

  // File containing request body to get an access token.
  private static final String authRequestJSONFileName = "OAuthCredentials.json";

  /**
   * Entry point for this sample client program.
   */
  public static void main(String args[]) {
    try {
      DownloadDocumentsOfAgreement client = new DownloadDocumentsOfAgreement();
      client.run();
    }
    catch (Exception e) {
      System.err.println("Error while retrieving documents of the agreement");
      e.printStackTrace();
    }
  }

  /**
   * Main work function. See the class comment for details.
   */
  private void run() throws Exception {
    // Get oauth access token to make further API calls.
    String accessToken = RestApiOAuthTokens.getOauthAccessToken(authRequestJSONFileName);

    // Display name of the agreement associated with the specified agreement ID.
    JSONObject agreementInfo = RestApiAgreements.getAgreementInfo(accessToken, agreementId);
    String agreementName = (String) agreementInfo.get("name");
    System.out.println("Agreement name: " + agreementName);

    // Fetch list of documents associated with the specified agreement.
    JSONObject docJson = RestApiAgreements.getAgreementDocuments(accessToken, agreementId);
    JSONArray documentList = (JSONArray) docJson.get("documents");

    for (Object eachDocument : documentList) {
      JSONObject document = (JSONObject) eachDocument;

      // Get ID and name of each document.
      String documentId = (String) document.get("id");
      String documentName = (String) document.get("name");

      // Generate a running file name for storing locally. Note that in practice the document may not be a PDF file (e.g. the API call 
      // requested the document in its original format) and document name itself might contain an extension. For simplicity we ignore 
      // these possibilities.
      String fileName = documentName + "_" + System.currentTimeMillis() + ".pdf";
      System.out.println("Saving document '" + fileName + "'");

      // Download and save these documents.
      byte docStream[] = RestApiAgreements.getDocumentBytes(accessToken, agreementId, documentId);
      if (docStream != null) {
        boolean success = FileUtils.saveToFile(docStream, FileUtils.AGREEMENT_DOCS_OUTPUT_PATH, fileName);
        if (success)
          System.out.println("Successfully saved document in '" + FileUtils.AGREEMENT_DOCS_OUTPUT_PATH + "'.");
        else
          System.err.println(RestError.FILE_NOT_SAVED.errMessage);
      }
      else {
        System.err.println("Error while retrieving documents of the agreement: " + agreementName);
      }
    }
  }

}
```