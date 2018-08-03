# Get an Agreement’s Status

This sample client demonstrates how to to get status of all agreements of an account and display their agreement ID, name and status.

**IMPORTANT:**

1. Before running this sample, check that you have modified the JSON file OAuthCredentials.json with appropriate values. Which values need to be specified is indicated in the file.
2. You can also provide your OAuth access token in the `OAUTH_ACCESS_TOKEN` constant in the `RestApiOAuthTokens` class, which will then be used as the OAuth access token for making API calls.
3. You can also provide a refresh token in the `OAUTH_REFRESH_TOKEN constant` in the `RestApiOAuthTokens` class to refresh the OAuth access token.

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

import adobesign.api.rest.sample.util.RestApiAgreements;
import adobesign.api.rest.sample.util.RestApiOAuthTokens;
import org.json.simple.JSONArray;
import org.json.simple.JSONObject;

public class GetStatusOfAgreements {
  // File containing request body to get an access token.
  private static final String authRequestJSONFileName = "OAuthCredentials.json";

  /**
   * Entry point for this sample client program.
   */
  public static void main(String args[]) {
    try {
      GetStatusOfAgreements client = new GetStatusOfAgreements();
      client.run();
    }
    catch (Exception e) {
      System.err.println("Failure in getting status of the agreements.");
      e.printStackTrace();
    }
  }

  /**
   * Main work function. See the class comment for details.
   */
  private void run() throws Exception {
    // Get oauth access token to make further API calls.
    String accessToken = RestApiOAuthTokens.getOauthAccessToken(authRequestJSONFileName);

    JSONArray agreementList = RestApiAgreements.getMyAgreements(accessToken);

    if (agreementList != null) {
      for (Object eachAgreement : agreementList) {
        JSONObject agreement = (JSONObject) eachAgreement;
        System.out.println("AgreementName: " + agreement.get("name"));
        System.out.println("AgreementID: " + agreement.get("id"));
        System.out.println("AgreementStatus: " + agreement.get("status"));
        System.out.println();
      }
    }
  }
}
```