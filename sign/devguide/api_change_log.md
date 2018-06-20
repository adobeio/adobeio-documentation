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

[GET /users/{userId}/groups](https://secure.echosign.com/public/docs/restapi/v6#!/users/getGroupsOfUser)  
This is a new endpoint to list all the groups associated with users. Functionalities from groups were moved to this API.

[GET /users/{userId}/views](https://secure.echosign.com/public/docs/restapi/v6#!/users/getUserViews)  
Returns all the views related to a user; for example, account view, manage view and user profile view.

[GET /users/{userId}/signatures/{signatureId}](https://secure.echosign.com/public/docs/restapi/v6#!/users/getUserSignature)  
Returns the default signature of a user. Signature here includes initials, stamps, and signatures associated with the user.

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

[POST/agreements/{agreementId}/formFields](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/postFormFields)  
Part of the authoring API set: helps in creating a form field in the agreement document.

[POST/agreements/{agreementId}/members/share](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/createShareOnAgreement)  
Allows users to share agreements with other users.

[POST /agreements/{agreementId}/members/participantSets/{participantSetId}/participants/{participantId}/signingTokens](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/createSigningToken)  
Performs both participant security authentication and client capability validation:

- The response is a signing token. All other APIs that are used in an agreement-signing context require a signing token.
- The signing token can be accessed and modified through a pair of GET/PUT APIs.
- The signing token generated has a configured lifecycle setting and expires after that.

[PUT /agreements/{agreementId}/members/participantSets/{participantSetId}/participants/{participantId}/signingTokens/{signingTokenId}](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreementSigningToken)  
Updates the consent information associated with an agreement. Having a separate API to record user consent allows us to emulate cases where these need to be accepted before viewing an agreement. The above case was not possible through v5 APIs, as this operation was tied to the final signing API.

## Updated APIs

[DELETE /users/{userId}/signatures/{signatureId}](https://secure.echosign.com/public/docs/restapi/v6#!/users/deleteUserSignature)

Was:

- [DELETE /users/{userId}/signatures/profile](https://secure.echosign.com/public/docs/restapi/v5#!/users/deleteProfileSignature)
- [DELETE /users/{userId}/initials/profile](https://secure.echosign.com/public/docs/restapi/v5#!/users/deleteProfileInitials)

In version 6 we have a single API to delete signatures and user initials.

[GET /baseUris](https://secure.na1.echosignawspreview.com/public/docs/restapi/v6#!/base_uris/getBaseUris)

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

[GET /agreements/{agreementId}/members/participantSets/{participantSetId}/participants/{participantId}/formFieldValues](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAgreementFormFieldsValues)

Was: [GET /agreements/{agreementId}/participantSets/{participantSetId}/participants/{participantId}/formFieldsData](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/getAgreementFormFieldsData)

- Returns all fields (along with identity fields) assigned to the participant with their default values.
- Can auto-save identity fields.
- Compatible with Web App behavior.
- Consumes the signing token in the header.
- Conversion status of the uploaded attachment is dropped, and is handled through not allowing any update on the signing page while conversion is still under process.

[GET /agreements/{agreementId}/members/participantSets/{participantSetId}/participants/{participantId}/signingTokens/{signingTokenId}](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAgreementSigningToken)

Was: [GET /agreements/{agreementId}/participantSets/{participantSetId}/participants/{participantId}/signingInfo](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/getAgreementSigningInfo)

- The [signingInfo API](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/getAgreementSigningInfo) is dropped in version 6 and the corresponding data are returned through [GET /{signingTokenId}](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getAgreementSigningToken)
- Along with the terms of use URL and consumer disclosure URL, we also provide their present acceptance status.

[GET /agreements/{agreementId}/signingUrls](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/getSigningUrl)  
Fixed existing issues in scenarios where the sender is also a signer.

[GET /users](https://secure.echosign.com/public/docs/restapi/v6#!/users/getUsers)

- Paginated response.
- The groupId is no longer returned through this API.
- Returns the new account admin information.

[GET /users/{userId}](https://secure.echosign.com/public/docs/restapi/v6#!/users/getUserDetail)

- A few unusable fields were dropped.
- We have removed capability flags from here. Clients now need to call [GET /users/{userId}/settings](https://secure.echosign.com/public/docs/restapi/v6#!/users/getUserSettings) to compute the capability flags.

[POST /transientDocuments](https://secure.na1.echosignawspreview.com/public/docs/restapi/v6#!/transientDocuments/createTransientDocument)

- Returns an UNSUPPORTED\MEDIA\TYPE error for unsupported media type in Adobe Sign

[POST /agreements](https://secure.na1.echosignawspreview.com/public/docs/restapi/v6#!/agreements/createAgreement)

- The request body is now consistent with its GET/PUT counterpart. A common agreement model is used across all these APIs.
- Interactive options have been removed from the request body and is available through the separate [GET /agreements/{agreementId}/views](https://secure.na1.echosignawspreview.com/public/docs/restapi/v6#!/agreements/getAgreementView) API.
- Support for form fields, form fields layer template, amd merge fields has been removed from here and will now be available through the authoring APIs.
- The document visibility feature is available in v6.
- This API is more responsive as it is now asynchronous.
- Clients can build up an agreement sequentially using draft functionality.
- The signatureFlow parameter is dropped from v6 and is now implicitly inferred through the sequence that the values of order parameter are given in all participant sets.
- The Suppress Email feature is available in v6.
- You can create an agreement in different states using the transistionState field.
- There is a separate state transitioning (from draft to agreement) API.

[POST /agreements/{agreementId}/members/participantSets/{participantSetId}/delegatedParticipantSets](https://secure.na1.echosignawspreview.com/public/docs/restapi/v6#!/agreements/createDelegatedParticipantSets)

- You should specify an agent delegation role for a successful call.
- You can delegate to multiple participants, who constitute a newly created participant set.
- You are not allowed to perform &ldquo;Someone else should sign&rdquo; with this API, as it will now be done through [PUT /agreements/{agreementId}/members/participantSets/{participantSetId}](https://secure.na1.echosignawspreview.com/public/docs/restapi/v6#!/agreements/updateParticipantSet)

[POST /agreements/{agreementId}/reminders](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/createReminderOnParticipant)

Was: [POST /reminders](https://secure.na1.echosign.com/public/docs/restapi/v5#!/reminders/createReminder)

- Creates reminders for multiple participants.
- Lets you set the next send date, note, frequency, and other parameters.
- You can track each reminder through the ID returned from here.

[PUT /agreements/{agreementId}/members/participantSets/{participantSetId}/participants/{participantId}/acknowledgement](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreementAcknowledgement)

Was: [POST /agreements/{agreementId}/participantSets/{participantSetId}/participants/{participantId}/modificationAcknowledgement](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/agreementModificationAck)

- This API is also largely the same, but instead of create, it is treated as updating an agreement with an acknowledgement.
- Requires an ETag in the request header.

[POST /users/{userId}/signatures](https://secure.echosign.com/public/docs/restapi/v6#!/users/createSignature)

Was:

- [PUT /users/{userId}/signatures/profile](https://secure.echosign.com/public/docs/restapi/v5#!/users/updateProfileSignature)
- [PUT /users/{userId}/initials/profile](https://secure.echosign.com/public/docs/restapi/v5#!/users/updateProfileInitials)

In v6 we have a single API to create all signatures and initials.

[PUT /agreements/{agreementId}/members/participantSets/{participantSetId}/participants/{participantId}/formFieldValues](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreementFormFieldValues)

Was: [PUT /agreements/{agreementId}/participantSets/{participantSetId}/participants/{participantId}/formFieldsData](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/updateAgreementFormFields)

This endpoint is largely kept the same except for the following:

- ETag is now introduced to take care of concurrent updates in the fields.
- The request structure has changed.
- Identity fields such as the signer&rsquo;s full name, title, company, and more can be updated in this API itself instead of handling in a separate API.
- All the fields that are assigned to a participant are discoverable through this API&rsquo;s GET counterpart, so update operations do not need any PDF parsing for field names.

[PUT /agreements/{agreementId}/members/participantSets/{participantSetId}/participants/{participantId}/status](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/signAgreementForParticipation)

Was: [PUT /agreements/{agreementId}/participantSets/{participantSetId}/participants/{participantId}/status](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/signAgreementForParticipation)

- This API in V6 only has signature data, participant status (intended final status), and device info.
- User consent update are handled by [PUT /agreements/{agreementId}/members/participantSets/{participantSetId}/participants/{participantId}/signingTokens/{signingTokenId}](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreementSigningToken)
- Earlier, for updating identity field values, only this API was used, but now the following API would be used:  
[PUT /agreements/{agreementId}/members/participantSets/{participantSetId}/participants/{participantId}/formFieldValues](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreementFormFieldValues)

[PUT /agreements/{agreementId}/participantSets/{participantSetId}/participants/{participantId}/reject](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/rejectAgreementForParticipation)  
Enables a participant to reject an agreement.

[PUT /agreements/{agreementId}/state](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreementState)

Was: [PUT /agreements/{agreementId}/status](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/updateStatus)

- Dropped the [PUT /agreements/{agreementId}/status](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/updateStatus) API, as it was offering a dedicated endpoint for modifying a property of the agreement resource.
- The new API offers an action-based semantic to transition between states of an agreement.
- The intended final state of the agreement is provided in the request body and the response indicates if the agreement was successfully transitioned into the intended state.
- This API can be used by the sender to cancel the agreement.

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

[POST /agreements/{agreementId}/participantSets/{participantSetId}/participants/{participantId}/signingKeys](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/getAgreementSigningKey)  
[POST /agreements/{agreementId}/participantSets/{participantSetId}/participants/{participantId/signingSecurityTokens/password](https://secure.echosign.com/public/docs/restapi/v5#!/agreements/createSigningSecurityToken)  
These endpoints are now dropped due to the following reasons:

- The participation validation (along with any other signing validations) will be performed on all the individual signing APIs and is kept implicit like all other REST APIs in v6.
- In version 6, the supported features and participant security validation are now achieved through a single API, [POST /signingTokens](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/createSigningToken), which provides a signing token that is necessary to all other signing APIs.

[POST /users](https://secure.na1.echosign.com/public/docs/restapi/v5#!/users/createUser)  
This API has been dropped from version 6, as user creation will now be handled through Adobe Identity Management Service (IMS) and not Sign.

