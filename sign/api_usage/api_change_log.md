# API Change Log

Adobe Sign version 6 includes many changes to the API model. 

On this page:
[New APIs](#newapis)
[Updated APIs](#updatedapis)
[Removed APIs](#removedapis)

## New APIs

[GET /agreements/{agreementId}/me/note](https://secure.na1.echosign.com/public/docs/restapi/v6#!/agreements/getAgreementNoteForApiUser)  
Retrieves the latest note on an agreement for the user.

[GET /agreements/{agreementId}/members](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAllMembers)  
Returns all the users associated with an agreement: participant set, cc&rsquo;s, shared participants, and sender.

[GET /agreements/{agreementId}/members/participantSets/{participantSetId}](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getParticipantSet)  
Returns a detailed participant set object.

[GET /agreements/{agreementId}/reminders](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAgreementReminders)  
Lists all the reminders on an agreement.

[GET /libraryDocuments/{libraryDocumentId}/me/note](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/getLibraryDocumentNoteForApiUser)  
Retrieves the latest note on a library template for the user.

[GET /megaSigns/{megaSignId}/childAgreementsInfo/{childAgreementsInfoFileId}](https://secure.na1.echosign.com/public/docs/restapi/v6#!/megaSigns/getChildAgreementsInfoFile)  
Retrieves the file stream of the original CSV file that was uploaded by the sender while creating the MegaSign.

[GET /megaSigns/{megaSignId}/events](https://secure.na1.echosign.com/public/docs/restapi/v6#!/megaSigns/getEvents)  
Lists all the events of a MegaSign.

[GET /users/{userId}/groups](https://secure.echosign.com/public/docs/restapi/v6#!/users/getGroupsOfUser)  
Lists all the groups to which the user identified by the userId belongs.

[POST/agreements/{agreementId}/formFields](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/postFormFields)  
Creates form fields in an agreement using a library template.

[POST/agreements/{agreementId}/members/share](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/createShareOnAgreement)  
Allows users to share agreements with other users.

[POST /agreements/{agreementId}/views](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAgreementView)  
Returns the requested views such as manage page view, agreement documents view, post send page view associated with an agreement in the requested configuration.

[POST /libraryDocuments/{libraryDocumentId}/views](https://corporate.na1.echosign.com/public/docs/restapi/v6#!/libraryDocuments/createLibraryDocumentView)  
Returns the requested views, such as manage page view, library documents view, and send page view of this library document in the requested configuration.

[POST /megaSigns/{megaSignId}/views](https://secure.echosign.com/public/docs/restapi/v6#!/megaSigns/getMegaSignView)  
Provides all the views associated with a megaSign, such as manage page view, documents view, etc.

[POST /users/{userId}/views](https://secure.echosign.com/public/docs/restapi/v6#!/users/getUserViews)  
Provides all the views associated with a user, like profile page view, account page view, or manage page view.

[POST /widgets/{widgetId}/views](https://secure.echosign.com/public/docs/restapi/v6#!/widgets/getWidgetView)  
Returns the requested views, such as manage page view, widget documents view, and post send page view associated with a widget in the requested configuration.

[PUT /agreements/{agreementId}](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreement)  
Updates the data of an agreement, such as name, participants, etc.

[PUT /agreements/{agreementId}/formFields](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateFormFields)  
Edit or modify an existing form field on an agreement document.

[PUT /agreements/{agreementId}/me/visibility](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreementVisibility)  
Manage the visibility of an agreement in `GET /agreements`.

[PUT /agreements/{agreementId}/members/participantSets/{participantSetId}](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateParticipantSet)  
Updates an existing participant set of an agreement. Adds some more capability to the existing recipient update feature:

1. Allows replacing a specific participant in the set instead of choosing between either replacing all participants or no one.
2. Allows sender to replace participants who are not the current signer as well.

[PUT /agreements/{agreementId}/state](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreementState)  
Transitions an agreement from one state to another: for example, `DRAFT` to `IN_PROCESS`. Note that not all transitions are allowed. An allowed transition would follow the following sequence: `DRAFT` -&gt; `AUTHORING` -&gt; `IN_PROCESS` -&gt; `CANCELLED`.

[PUT /libraryDocuments/{libraryDocumentId}](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/updateLibraryDocument) 
Updates the data of a library document, such as name, type, scope, etc.

[PUT /libraryDocuments/{libraryDocumentId}/me/visibility](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/updateLibraryDocumentVisibility)  
A new API to control visibility of an agreement in the `GET /libraryDocuments` response.

[PUT /libraryDocuments/{libraryDocumentId}/state](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/updateLibraryDocumentState)  
Transitions a library document from one state to another: for example, `AUTHORING` to `ACTIVE`. Note that not all transitions are allowed. An allowed transition would follow the following sequence: `AUTHORING` -&gt; `ACTIVE`.

[PUT /megaSigns/{megaSignId}/state](https://secure.echosign.com/public/docs/restapi/v6#!/megaSigns/updateMegaSignState)  
Transitions a MegaSign from one state to another: for example, `IN_PROCESS` to `CANCELLED`. Note that not all transition are allowed. An allowed transition would follow the following sequence: `IN_PROCESS` -&gt; `CANCELLED`.

[PUT /users/{userId}/groups](https://secure.echosign.com/public/docs/restapi/v6#!/users/updateGroupsOfUser)  
Migrates the user to a different group or updates their role in the existing group.

[PUT /widgets/{widgetId}/state](https://secure.echosign.com/public/docs/restapi/v6#!/widgets/updateWidgetState)  
Transitions a widget from one state to another: for example, `DRAFT` to `IN_PROCESS`. Note that not all transition are allowed. An allowed transition would follow one of the following sequences: `DRAFT` -&gt; `AUTHORING` -&gt; `ACTIVE`,  `ACTIVE` &lt;-&gt; `INACTIVE`,  `DRAFT` -&gt; `CANCELLED`.

## Updated APIs

[GET /baseUris](https://secure.echosign.com/public/docs/restapi/v6#!/base_uris/getBaseUris)

- The endpoint path is now changed to be consistent with the camel casing convention in Adobe Sign API (`base_uri` is now `baseUri`).
- The response parameter is also renamed to have camel casing.

[GET /agreements](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAgreements)

- The response is paginated.
- The response also lists all the user-created drafts.
- Filter to show/hide hidden agreements.
- Data returned is same as v5.

[GET /agreements/{agreementId}](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAgreementInfo)

- The model is consistent with the corresponding POST and PUT APIs.
- Participants and events information are retrieved through separate granular APIs.
- Detailed participants and events information are available through separate endpoints.

[GET /agreements/{agreementId}/auditTrail](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAuditTrail)  
Includes audit reports for draft creation.

[GET /agreements/{agreementId}/combinedDocument](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getCombinedDocument)  
Uses `participantId` instead of `participantEmail` as a filter.

[GET /agreements/{agreementId}/documents](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAllDocuments)

- Uses `participantId` instead of `participantEmail` as a filter.
- There are minor changes in the field names.

[GET /agreements/{agreementId}/documents/imageUrls](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAllDocumentsImageUrls)

- Uses `participantId` instead of `participantEmail` as a filter.
- There are minor changes in the field names.
- Provides annotated image URLs with `documentId` and page number.

[GET /agreements/{agreementId}/documents/{documentId}/imageUrls](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getDocumentImageUrls)

- Uses `participantId` instead of `participantEmail` as a filter.
- There are minor changes in the field names.

[GET /libraryDocuments](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/getLibraryDocuments)  

- Response is paginated.
- Creator email and status is in the response.
- We&rsquo;ve added a query parameter to view/un-view all the hidden agreements.
- The `scope` parameter in v5 is mapped to `sharingMode` in v6.
- `PERSONAL` in v5 is now `USER` in v6.
- `SHARED` in v5 is now `GROUP in v6.
- The `libraryTemplateType` filter is dropped from this API. This will be available along with other filtering through search services.

[GET /libraryDocuments/{libraryDocumentId}](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/getLibraryDocumentInfo)  

- Events have been removed from the response of this endpoint and are now returned through a dedicated events endpoint.
- The `latestVersionId` parameter is now removed from here and will be available in `GET /libraryDocuments`.
- We have removed obsolete and unnecessary parameters: `locale`, `participants`, `message` and `securityOptions`.
- The model is consistent with the corresponding POST and PUT operations.

[GET /libraryDocuments/{libraryDocumentId}/documents](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/getDocuments)  

- Added a `label` parameter for using in the custom workflow.
- A `versionId` of the documents has been added as a filter.

[GET /libraryDocuments/{libraryDocumentId}/documents/{documentId}](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/getLibraryDocument)  
A base64 encoding option is available for the generated PDF.

[GET /libraryDocuments/{libraryDocumentId}/documents/{documentId}/imageUrls](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/getLibraryDocumentImageUrls)  
Minor restructuring in the response.

[GET /users](https://secure.echosign.com/public/docs/restapi/v6#!/users/getUsers)

- Paginated response.
- The `groupId` is no longer returned through this API.
- Returns the new account admin information.

[GET /users/{userId}](https://secure.echosign.com/public/docs/restapi/v6#!/users/getUserDetail)

- A few unusable fields were dropped.
- We have removed capability flags from here. 

[GET /widgets](https://secure.echosign.com/public/docs/restapi/v6#!/widgets/getWidgets)  
Paginated response.

[GET /widgets/{widgetId}](https://secure.echosign.com/public/docs/restapi/v6#!/widgets/getWidgetInfo)  
The model is consistent with the POST and PUT operations.

[POST /agreements](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/createAgreement)

- The request body is now consistent with its GET/PUT counterpart. A common agreement model is used across all these APIs.
- Interactive options have been removed from the request body and is available through the separate [POST /agreements/{agreementId}/views](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAgreementView) API.
- Support for form fields, form fields layer template, amd merge fields has been removed from here and will now be available through the authoring APIs.
- The document visibility feature is available in v6.
- This API is more responsive as it is now asynchronous.
- Clients can build up an agreement sequentially using draft functionality.
- The `signatureFlow` parameter is dropped from v6 and is now implicitly inferred through the sequence that the values of other parameters are given in all participant sets.
- The Suppress Email feature is available in v6.
- You can create an agreement in different states using the `transistionState` field.
- There is a separate state transitioning (from draft to agreement) API.

[POST /agreements/{agreementId}/members/participantSets/{participantSetId}/delegatedParticipantSets](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/createDelegatedParticipantSets)

- You should specify an agent delegation role for a successful call.
- You can delegate to multiple participants, who constitute a newly created participant set.
- You are not allowed to perform &ldquo;Someone else should sign&rdquo; with this API, as it will now be done through [PUT /agreements/{agreementId}/members/participantSets/{participantSetId}](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateParticipantSet)

[POST /agreements/{agreementId}/reminders](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/createReminderOnParticipant)

Was: [POST /reminders](https://secure.na1.echosign.com/public/docs/restapi/v5#!/reminders/createReminder)

- Creates reminders for multiple participants.
- Lets you set the next send date, note, frequency, and other parameters.
- You can track each reminder through the ID returned from here.

[POST /libraryDocuments](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/createLibraryDocument)

- The model is consistent with the corresponding GET & PUT operations.
- Obsolete parameters `libraryDocumentId` and `libraryDocumentName` have been removed from `fileInfosstructure`. This was present in prior versions, but was unusable, because we did not allow library template creation using an existing template through the API.
- Interactive options have been removed from the request body; the equivalent functionality is now available through the `POST /libraryDocuments/{libraryDocumentId}/views` API.
- You can create a library template in different states using the transition state field.
- There is a separate state transitioning (Authoring to Active) API.

[POST /transientDocuments](https://secure.echosign.com/public/docs/restapi/v6#!/transientDocuments/createTransientDocument)

- Returns an `UNSUPPORTED_MEDIA_TYPE` error for unsupported media types in Adobe Sign.

[POST /widgets](https://secure.echosign.com/public/docs/restapi/v6#!/widgets/createWidget)  

- The request body is now consistent with this API&rsquo;s GET/PUT counterpart. A common agreement model is used across all these APIs.
- Form fields layer template and Merge Fields support have been removed in v6 for widget creation.
- The `signatureFlow` parameter has been dropped and the workflow is inferred through the order in the `additionalParticipantSetsInfo` parameter.

[PUT /agreements/{agreementId}/participantSets/{participantSetId}/participants/{participantId}/reject](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/rejectAgreementForParticipation)  
Enables a participant to reject an agreement.

[PUT /agreements/{agreementId}/state](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreementState)

Was: [PUT /agreements/{agreementId}/status](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/updateStatus)

- Dropped the [PUT /agreements/{agreementId}/status](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/updateStatus) API, as it was offering a dedicated endpoint for modifying a property of the agreement resource.
- The new API offers an action-based semantic to transition between states of an agreement.
- The intended final state of the agreement is provided in the request body and the response indicates if the agreement was successfully transitioned into the intended state.
- This API can be used by the sender to cancel the agreement.

[PUT /users/{userId}](https://secure.echosign.com/public/docs/restapi/v6#!/users/modifyUser)  
Updating the group of the user is now handled via the `PUT /users/{userId}/groups` API.

[PUT /users/{userId}/state](https://secure.echosign.com/public/docs/restapi/v6#!/users/modifyUserState)  
This is functionally the same as before, but the API structure is revamped to make it consistent with other state transition APIs in v6.

## Removed APIs

[DELETE /agreements/{agreementId}](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/deleteAgreement)  
The equivalent functionality of removing an agreement permanently from a user&rsquo;s manage page can be achieved through the combination of [DELETE /agreements/{agreementId}/documents](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/deleteDocuments) and [PUT /visibility](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreementVisibility).

[GET /agreements/{agreementId}/documents/{documentId}/url](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/getDocumentUrl)  
The v5 API had the redundant functionality of providing combined agreement docs, which can be achieved through the [GET /document](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getDocument) API.