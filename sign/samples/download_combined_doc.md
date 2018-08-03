# Download the Combined Document for an Agreement

This sample client demonstrates how to to download the combined document for the specified agreement.

**IMPORTANT:**

1. Before running this sample, check that you have modified the JSON file OAuthCredentials.json with appropriate values. Which values need to be specified is indicated in the file.
2. You can also provide your OAuth access token in the 'OAUTH_ACCESS_TOKEN` constant in the `RestApiOAuthTokens` class, which will then be used as the OAuth access token for making API calls.
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

import adobesign.api.rest.sample.util.FileUtils;
import adobesign.api.rest.sample.util.RestApiAgreements;
import adobesign.api.rest.sample.util.RestApiOAuthTokens;
import adobesign.api.rest.sample.util.RestError;
import org.json.simple.JSONObject;

public class DownloadCombinedDocumentOfAgreement {
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
      DownloadCombinedDocumentOfAgreement client = new DownloadCombinedDocumentOfAgreement();
      client.run();
    }
    catch (Exception e) {
      System.err.println("Error while retrieving combined document of agreement");
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

    String fileName = agreementName + "_" + System.currentTimeMillis() + ".pdf";
    System.out.println("Saving combined document of agreement '" + fileName + "'");

    // Make API call to get audit trail of this agreement.
    byte auditStream[] = RestApiAgreements.getAgreementCombinedBytes(accessToken, agreementId);

    if (auditStream != null) {
      boolean success = FileUtils.saveToFile(auditStream, FileUtils.AGREEMENT_COMBINED_DOC_OUTPUT_PATH, fileName);
      if (success)
        System.out.println("Successfully saved combined document of agreement in '" + FileUtils.AGREEMENT_COMBINED_DOC_OUTPUT_PATH + "'.");
      else
        System.err.println(RestError.FILE_NOT_SAVED.errMessage);
    }
    else {
      System.err.println("Error while retrieving combined document of agreement: " + agreementName);
    }
  }
}
```
