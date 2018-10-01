# REST API Samples

The samples included in this section are Java clients of the Adobe Sign REST API that demonstrate how to use the API as well as some of its key capabilities.

## Samples
- [Create a new widget with countersigners](samples/create_new_widget.md)
- [Get signing URLs for an agreement](samples/get_signing_url.md)
- [Send an agreement using a library ID](samples/send_using_library_doc.md)   
- [Send an agreement using a transient document](samples/send_using_transient_doc.md)
- [Get the status of an agremeent](samples/get_agreement_status.md)
- [Send reminders to participants](samples/send_reminders.md)
- [Download the documents from an agreement](samples/download_documents.md)
- [Download a combined document](samples/download_combined_doc.md)
- [Download an audit trail](samples/download_audit_trail.md)

## The package
All sources are under the *adobesign.api.rest.sample* package (and sub packages) as published in the [Adobe Sign GitHub repo](https://www.adobe.com/go/esign-api-samples) and are laid out as follows:

* **adobesign.api.rest.sample**

Contains individual sample clients, each demonstrating a specific capability. Each client is named according to the capability it demonstrates. For example, the client GetUsersInAccount.java shows how to retrieve a list of users from the account of the user on whose behalf the API call is made (also called **API user** in this document).

* **adobesign.api.rest.sample.util**

Contains helper classes that encapsulate the REST calls required by the sample clients. Of particular note is RestApiUtils.java, which contains methods that make the actual low-level REST API calls.

* **adobesign.api.rest.sample.requests**

Contains input files used by the sample clients. These include JSON objects that specify the input data and arguments required by some of the API calls.

## Getting started

These instructions will help you run the samples provided here.

### Prerequisites

Before using the samples, you need to obtain either an OAuth access token or an integration key. Please refer to the AdobeSign API page (https://secure.echosign.com/account/echosignApi) for information on how to do this for your account.

You can provide this token or key as a value to the `OAUTH_ACCESS_TOKEN` constant in RestApiOAuthTokens.java, or you can provide a refresh token as a value to the `OAUTH_REFRESH_TOKEN` constant (in the same class) which will be used to refresh the OAuth access token.

If neither is provided, then a new OAuth access token will be requested from AdobeSign based on credentials provided in the OAuthCredentials.json file.
Please refer to the AdobeSign OAuth page (https://secure.echosign.com/public/static/oauthDoc.jsp) for information on how to obtain OAuth credentials for your account.

### Using the Samples

Each sample client has a set of instructions (provided as class comments) that need to be followed. In particular, look for "TODO" comments in the client code and in  the JSON input files for values that need to be updated before the client can work properly. Once the clients and support files are updated with appropriate values, they can be compiled and run. 

The following steps outline one way this can be done using the command line on Windows.

>Note that on Linux and Mac OS, the path separator for the  `-cp` and `-sourcepath` options is &ldquo;:&rdquo; (colon) instead of &ldquo;;&rdquo; (semicolon).

- Navigate to the top-most folder (the one containing "adobesign" and "lib") so that it becomes the current directory.

- Compile the sources using the following command:  
    `javac -sourcepath .;adobesign -cp lib/json_simple-1.1.jar adobesign/api/rest/sample/*.java`

- To run a specific client, use the following command:  
    `java -cp .;lib/json_simple-1.1.jar adobesign.api.rest.sample.{name of client class}`

For example, to run GetUsersInAccount, use:  
    `java -cp .;lib/json_simple-1.1.jar adobesign.api.rest.sample.GetUsersInAccount`

You may also use an IDE of your choice. In that case, you will need to create a new project with sources taken from the adobesign folder and lib/json.jar set up as an input library.


### Output Path

The default output path used in the sample clients is the user temp directory. If needed, this can be changed by updating the method `adobesign.api.rest.sample.util.FileUtils.getDefaultOutputPath()`.