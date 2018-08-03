# Send an Agreement Using a Transient Document

This sample client demonstrates how to send an agreement using a transient document ID. See the postTransientDocument method in the file /util/RestApiAgreements.java for a definition of transient documents.

**IMPORTANT:**

1. Before running this sample, check that you have modified the JSON files OAuthCredentials.json and SendAgreement.json with appropriate values. Which values need to be specified is indicated in the files.
2. You can also provide your OAuth access token in the `OAUTH_ACCESS_TOKEN` constant in the `RestApiOAuthTokens` class, which will then be used as the OAuth access token for making API calls.
3. You can also provide a refresh token in the `OAUTH_REFRESH_TOKEN constant` in the `RestApiOAuthTokens` class to refresh the OAuth access token.
4. Check that the default input file in the field `fileToBeUploaded` below is suitable.
5. Check that the name to be given to the uploaded file in the field `uploadedFileName` below is suitable. This name can be different from the original file name.

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

import org.json.simple.JSONObject;

import adobesign.api.rest.sample.util.RestApiAgreements;
import adobesign.api.rest.sample.util.RestApiOAuthTokens;
import adobesign.api.rest.sample.util.RestApiUtils;
import adobesign.api.rest.sample.util.RestApiAgreements.DocumentIdentifierName;

public class SendAgreementUsingTransientDocument {

  // File containing the request JSON for fetching access token.
  private static final String authRequestJSONFileName = "OAuthCredentials.json";

  // File containing the request JSON for sending an agreement.
  private static final String sendAgreementJSONFileName = "SendAgreement.json";

  // Name of the file to be uploaded for sending an agreement.
  // TODO: Specify file name of choice here. The file must exist in the "requests" sub-package.
  private static final String fileToBeUploaded = "Sample.pdf";

  // Mime-type of the file being uploaded.
  // TODO: Change this depending on actual file used.
  private static final String mimeType = RestApiUtils.MimeType.PDF.toString();

  // Name to be given to the file after uploading it.
  // TODO: Specify a file name of choice, ensuring that its name consists only of characters in the ASCII character set (given this basic
  // sample implementation).
  private static final String uploadedFileName = "UploadedSample.pdf";

  /**
   * Entry point for this sample client program.
   */
  public static void main(String args[]) {
    try {
      SendAgreementUsingTransientDocument client = new SendAgreementUsingTransientDocument();
      client.run();
    }
    catch (Exception e) {
      System.err.println("Failure in sending the agreement using the transient document ID.");
      e.printStackTrace();
    }
  }

  /**
   * Main work function. See the class comment for details.
   */
  private void run() throws Exception {
    // Fetch oauth access token to make further API calls.
    String accessToken = RestApiOAuthTokens.getOauthAccessToken(authRequestJSONFileName);

    // Upload a transient document and retrieve transient document ID from the response.
    JSONObject uploadDocumentResponse = RestApiAgreements.postTransientDocument(accessToken, mimeType, fileToBeUploaded, uploadedFileName);
    String transientDocumentId = (String) uploadDocumentResponse.get("transientDocumentId");

    // Send an agreement using the transient document ID derived from above.
    DocumentIdentifierName idName = DocumentIdentifierName.TRANSIENT_DOCUMENT_ID;
    JSONObject sendAgreementResponse = RestApiAgreements.sendAgreement(accessToken, sendAgreementJSONFileName, transientDocumentId, idName);

    // Parse and read response.
    System.out.println("Agreement Sent. Agreement ID = " + sendAgreementResponse.get("id"));
  }
}
```