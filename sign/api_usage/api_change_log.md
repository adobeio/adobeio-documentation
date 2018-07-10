:navorder: 8

# API Change Log

Adobe Sign version 6 includes many changes to the API model. 

On this page:
[New APIs](#newapis)
[Updated APIs](#updatedapis)
[Removed APIs](#removedapis)

## New APIs

[GET /agreements/{agreementId}/events](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getEvents)  
This is a separate API to extract all the agreement events.

[GET /agreements/{agreementId}/formFields](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getFormFields)  
Lists all the form fields (along with location, conditions, and default values) associated with the given agreement.

[GET /agreements/{agreementId}/members](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAllMembers)  
Returns all the users associated with an agreement: participant set, cc&rsquo;s, shared participants, and sender.

[GET /agreements/{agreementId}/members/participantSets/{participantSetId}](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getParticipantSet)  
Returns a detailed participant set object.

[GET /agreements/{agreementId}/reminders](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAgreementReminders)  
Lists all the reminders on an agreement.

[GET /agreements/{agreementId}/views](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAgreementView)  
Returns all the views associated with an agreement, such as the manage page view, the agreement view, and the post send page view.

[GET /libraryDocuments/{libraryDocumentId}/events](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/getEvents)  
This is a dedicated API to retrieve all the events of a library template.

[GET /libraryDocuments/{libraryDocumentId}/me/note](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/getLibraryDocumentNoteForApiUser)  
Retrieves the latest note on a library template for the user.

[GET /users/{userId}/groups](https://secure.echosign.com/public/docs/restapi/v6#!/users/getGroupsOfUser)  
This is a new endpoint to list all the groups associated with users. Functionalities from groups were moved to this API.

[GET /users/{userId}/signatures/{signatureId}](https://secure.echosign.com/public/docs/restapi/v6#!/users/getUserSignature)  
Returns the default signature of a user. Signature here includes initials, stamps, and signatures associated with the user.

[GET /users/{userId}/views](https://secure.echosign.com/public/docs/restapi/v6#!/users/getUserViews)  
Returns all the views related to a user; for example, account view, manage view and user profile view.

[POST/agreements/{agreementId}/formFields](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/postFormFields)  
Part of the authoring API set: helps in creating a form field in the agreement document.

[POST/agreements/{agreementId}/members/share](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/createShareOnAgreement)  
Allows users to share agreements with other users.

[POST /agreements/{agreementId}/views](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAgreementView)  
Returns the requested views such as manage page view, agreement documents view, post send page view associated with an agreement in the requested configuration.

[POST /libraryDocuments/{libraryDocumentId}/views](https://corporate.na1.echosign.com/public/docs/restapi/v6#!/libraryDocuments/createLibraryDocumentView)  
Returns the requested views, such as manage page view, library documents view, and send page view of this library document in the requested configuration.

[POST /users/{userId}/views](https://secure.echosign.com/public/docs/restapi/v6#!/users/getUserViews)  
Returns the requested views of a user in the asked configuration; for example, account view, manage view and user profile view.

[POST /widgets/{widgetId}/views](https://secure.echosign.com/public/docs/restapi/v6#!/widgets/getWidgetView)  
Returns the requested views, such as manage page view, widget documents view, and post send page view associated with a widget in the requested configuration.

[PUT /agreements/{agreementId}](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreement)  
Enables the user to build up an agreement incrementally rather than as a one-shot creation process. This maintains a consistent request model with its POST and GET counterparts.

[PUT /agreements/{agreementId}/formFields](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateFormFields)  
Edit or modify an existing form field on an agreement document.

[PUT /agreements/{agreementId}/me/visibility](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreementVisibility)  
Manage the visibility of an agreement in GET /agreements.

[PUT /agreements/{agreementId}/members/participantSets/{participantSetId}](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateParticipantSet)  
Adds the capability to modify an existing participant set:

- The sender can replace a participant with another participant or add a new one.
- The API now can replace a single participant instead of choosing between either replacing all participants or no one.
- The &quot;someone else should sign&quot; functionality on the signing page is provided in this API.
- The request body is consistent with the GET response.

[PUT /libraryDocuments/{libraryDocumentId}](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/updateLibraryDocument) 
A new API to update the _name, template type_ and _sharing scope._ Allows incremental construction of a library template in the authoring state.

[PUT /libraryDocuments/{libraryDocumentId}/me/visibility](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/updateLibraryDocumentVisibility)  
A new API to control visibility of an agreement in the `GET /libraryDocuments` response.

[PUT /libraryDocuments/{libraryDocumentId}/state](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/updateLibraryDocumentState)  
A new API which offers an action-based semantic to transit between states of a library template.

[PUT /users/{userId}/groups](https://secure.echosign.com/public/docs/restapi/v6#!/users/updateGroupsOfUser)  
Migrates the user to a different group or updates their role in the existing group.

## Updated APIs

[GET /baseUris](https://secure.echosign.com/public/docs/restapi/v6#!/base_uris/getBaseUris)

- The endpoint path is now changed to be consistent with the camel casing convention in Adobe Sign API (base_uri is now baseUri).
- The response parameter is also renamed to have camel casing.

[GET /agreements](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAgreements)

- The response is paginated.
- The response also lists all the user-created drafts.
- There is a query parameter to view/unview all the hidden agreements.
- The response structure is equivalent to v5.
- Filters are not available.

[GET /agreements/{agreementId}](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAgreementInfo)

- The model is consistent with the corresponding POST and PUT APIs.
- Participants and events information are retrieved through separate granular APIs.
- &ldquo;Is agreement modifiable&rdquo; information (the Modify-in-Flight feature) is dropped, as modify-in-flight is not supported through version 6 APIs.

[GET /agreements/{agreementId}/auditTrail](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAuditTrail)  
Includes audit reports for draft creation.

[GET /agreements/{agreementId}/combinedDocument](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getCombinedDocument)  
Uses participantId instead of participantEmail as filter.

[GET /agreements/{agreementId}/documents](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAllDocuments)

- Uses participantId instead of participantEmail as a filter.
- There are minor changes in the field names.

[GET /agreements/{agreementId}/documents/imageUrls](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAllDocumentsImageUrls)

- Uses participantId instead of participantEmail as a filter.
- There are minor changes in the field names.
- Provides annotated image URLs with documentId and page number.

[GET /agreements/{agreementId}/documents/{documentId}/imageUrls](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getDocumentImageUrls)

- Uses participantId instead of participantEmail as a filter.
- There are minor changes in the field names.

[GET /agreements/{agreementId}/signingUrls](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/getSigningUrl)  
Fixed existing issues in scenarios where the sender is also a signer.

[GET /libraryDocuments](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/getLibraryDocuments)  

- Response is paginated.
- Creator email and status is in the response.
- We've added a query parameter to view/un-view all the hidden agreements.
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
- A `versionId` of the documents added as filter.

[GET /libraryDocuments/{libraryDocumentId}/documents/{documentId}](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/getLibraryDocument)  
A base64 encoding option is available for the generated PDF.

[GET /libraryDocuments/{libraryDocumentId}/documents/{documentId}/imageUrls](https://secure.echosign.com/public/docs/restapi/v6#!/libraryDocuments/getLibraryDocumentImageUrls)  
Minor restructuring in the response.

[GET /users](https://secure.echosign.com/public/docs/restapi/v6#!/users/getUsers)

- Paginated response.
- The groupId is no longer returned through this API.
- Returns the new account admin information.

[GET /users/{userId}](https://secure.echosign.com/public/docs/restapi/v6#!/users/getUserDetail)

- A few unusable fields were dropped.
- We have removed capability flags from here. Clients now need to call [GET /users/{userId}/settings](https://secure.echosign.com/public/docs/restapi/v6#!/users/getUserSettings) to compute the capability flags.

[GET /widgets](https://secure.echosign.com/public/docs/restapi/v6#!/widgets/getWidgets)  
Paginated response.

[GET /widgets/{widgetId}](https://secure.echosign.com/public/docs/restapi/v6#!/widgets/getWidgetInfo)  
The model is consistent with the POST and PUT operations.

[POST /agreements](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/createAgreement)

- The request body is now consistent with its GET/PUT counterpart. A common agreement model is used across all these APIs.
- Interactive options have been removed from the request body and is available through the separate [GET /agreements/{agreementId}/views](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAgreementView) API.
- Support for form fields, form fields layer template, amd merge fields has been removed from here and will now be available through the authoring APIs.
- The document visibility feature is available in v6.
- This API is more responsive as it is now asynchronous.
- Clients can build up an agreement sequentially using draft functionality.
- The signatureFlow parameter is dropped from v6 and is now implicitly inferred through the sequence that the values of order parameter are given in all participant sets.
- The Suppress Email feature is available in v6.
- You can create an agreement in different states using the transistionState field.
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

- Returns an UNSUPPORTED\MEDIA\TYPE error for unsupported media type in Adobe Sign

[POST /widgets](https://secure.echosign.com/public/docs/restapi/v6#!/widgets/createWidget)  

- The request body is now consistent with this API&rsquo;s GET/PUT counterpart. A common agreement model is used across all these APIs.
- Form fields layer template and Merge Fields support removed in v6 for widget creation.
- The signatureFlow parameter has been dropped and the workflow is inferred through the order in the additionalParticipantSetsInfo parameter.

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
This is functionally the ame as before, but the API structure is revamped to make it consistent with other state transition APIs in v6.

## Removed APIs

[DELETE /agreements/{agreementId}](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/deleteAgreement)

- Deleting agreements is not available, as we never used to delete agreements from system.
- The equivalent functionality of removing an agreement permanently from a user&rsquo;s manage page can be achieved through the combination of [DELETE /agreements/{agreementId}/documents](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/deleteDocuments) and [PUT /visibility](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreementVisibility).

[GET /agreements/{agreementId}/combinedDocument/url](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/getCombinedDocumentUrl)

This API was removed, as it had redundant functionality for providing combined agreement docs, which could be achieved through the [GET /combinedDocuments](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getCombinedDocument) API.

[GET /agreements/{agreementId}/documents/{documentId}/url](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/getDocumentUrl)  
The v5 API had the redundant functionality of providing combined agreement docs, which can be achieved through the [GET /document](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getDocument) API.

[GET /users/{userId}/signatures/profile](https://secure.echosign.com/public/docs/restapi/v5#!/users/getProfileSignature)  
[GET /users/{userId}/signatures/profile/image](https://secure.echosign.com/public/docs/restapi/v5#!/users/getProfileSignatureImage)  
[GET /users/{userId}/initials/profile](https://secure.echosign.com/public/docs/restapi/v5#!/users/getProfileInitials)  
[GET /users/{userId}/initials/profile/image](https://secure.echosign.com/public/docs/restapi/v5#!/users/getProfileInitialsImage)  
The information returned through these APIs is available via [GET /users/{userId}/signatures/{signatureId}](https://secure.echosign.com/public/docs/restapi/v6#!/users/getUserSignature) in v6.

[POST /users](https://secure.na1.echosign.com/public/docs/restapi/v5#!/users/createUser)  
This API has been dropped from version 6, as user creation will now be handled through Adobe Identity Management Service (IMS) and not Sign.

