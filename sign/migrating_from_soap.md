# Migrating From SOAP

If you are using the legacy Adobe Sign SOAP APIs, we highly recommend migrating your apps to consume the REST APIs. The table below will help you migrate by providing the REST equivalents for the legacy SOAP methods.

| **SOAP Endpoint** | **REST Endpoint** | **Description** |
| --- | --- | --- |
| `getBaseUris` | /baseUris, GET | Base URIs: API calls starting v5 of REST API must be made on a specific base URL obtained either from the api\_access\_point returned from the OAuth workflow or by making a call to the `GET /baseUris` endpoint. |
| `sendDocument` | /agreements, POST | SenderInfo is represented through `x-api-user`. Files are specified through /transientDocuments. |
| `sendDocumentInteractive` | /agreements/{agrId}/views, POST | From v6 onwards, the interactive views can be specified and obtained from the `POST /agreements/{agrId}/views` endpoint for the interactive behavior. |
| `sendDocumentMegaSign` | /megaSigns, POST | MegaSign allows sending the same agreement to multiple recipients and creating a separate instance of agreement for each recipient. |
| `createLibraryDocument` | /libraryDocuments, POST |   |
| `createLibraryDocumentInteractive` | /libraryDocuments/{libraryDocumentId}, POST | From v6 onwards, the interactive views can be specified and obtained from the `POST /agreements/{agrId}/views` endpoint for the interactive behavior. |
| `sendReminder` | agreements/{agrId}/reminders, POST |   |
| `removeDocument` | /agreements/{agrId}/documents, DELETE | To delete the documents of agreements, use the `DELETE /agreements/{agrId}/documents` endpoint; and to remove it from Manage Page(GET /agreements), use `PUT /agreements/{agrId}/visibility` |
| `cancelDocument` | /agreements/{agrId}/state, PUT | Cancel: Called by sender. |
| `rejectDocument` | /agreements/{agrId}/members/participantSets/{psId}/participants/{pId}/reject, PUT | Reject: Called by current signer. |
| `replaceSigner` | /agreements/{agreementId}/members/participantSets/{participantSetId}, PUT | Replace: Called by sender. Both the original signer and new one can sign, |
| `delegateSigning` | /agreements/{agrId}/participantSets/{psId}/ participants/{pId}/delegatedParticipantSet, POST | Delegate: Called by signer. Both the delegator and delegatee can sign, |
| `getDocumentInfo` | /agreements/{agrId}, GET | In SOAP API, `getDocumentInfo`, `getDocuments`, `getAuditTrail` etc. work on `documentKeys`, which can be an ID for an agreement, widget, or library document. The REST API demarcates these as separate resources (cleaner design and strongly typed) and hence, based on the kind of resource you are working on, there is a corresponding /libraryDocuments, /widgets to these. Example: `/widgets/{widgetId}, GET` will getDocumentInfo for `widgetId`, and similarly for documents, audit trail, etc. |
| `getDocumentInfosByExternalId` | /agreements, GET query = externalId | `externalId` can be used to map your internal IDs to eSign IDs. |
| `getDocuments` | /agreements/{agrId}/documents, GET | REST returns a list of document IDs that can be provided to the following endpoint to get a document stream. |
| `getDocumentUrls` | /agreements/{agrId}/combinedDocument/url, GET | Retrieve the URL of the combined document. |
| `getDocumentUrls` | /agreements/{agrId}/documents/{docId}/url, GET | Retrieve the URL of an individual document. |
| `getDocumentImageUrls` | /agreements/{agrId}/documents/imageUrls, GET | Retrieve the image URLs of all the visible pages of an agreement. |
| `getDocumentImageUrls` | /agreements/{agrId}/documents/{docId}/imageUrls, GET | Retrieve image URLs of all the visible pages of an agreement&rsquo;s document. |
| `getSupportingDocuments` | /agreements/{agrId}/documents, GET | Can also specify the content format. |
| `getFormData` | /agreements/{agrId}/formData, GET | Returns a CSV file stream. |
| `getAuditTrail` | /agreements/{agrId}/auditTrail, GET |   |
| `getSigningUrl` | /agreements/{agrId}/signingUrls, GET |   |
| `getEmbeddedView` | /agreements/{agrId}/views, POST | Use the name = DOCUMENT to get the embedded view of an agreement. |
| `getMyDocuments` | /agreements, GET |   |
| `getUserDocuments` | /agreements, GET | Use `x-api-user` for specifying the user whose agreements are to be retrieved. |
| `getMyLibraryDocuments` | /libraryDocuments, GET |   |
| `getLibraryDocumentsForUser` | /libraryDocuments, GET | Use `x-api-user` for specifying the user whose library documents are to be retrieved. |
| `getMyWidgets` | /widgets, GET |   |
| `getWidgetsForUser` | /widgets, GET | Use `x-api-user` for specifying the user whose widgets are to be retrieved. |
| `getMegaSignDocument` | /megaSigns/{megaSignId}/agreements, GET | Get all child agreement IDs of the parent MegaSign. |
| `getUsersInAccount` | /users, GET |   |
| `getGroupsInAccount` | /groups, GET |   |
| `getUsersInGroups` | /groups/{groupId}/users, GET |   |
| `moveUsersToGroup` | /users/{userId}/groups, PUT | Specify the new `groupId` in the request. |
| `getUserInfo` | /users/{userId}, GET |   |
| `createEmbeddedWidget` | /widgets, POST |   |
| `createUrlWidget` | /widgets, POST |   |
| `disableWidget` | /widgets/{widgetId}/state, PUT | Use status value as `INACTIVE`. |
| `enableWidget` | /widgets/{widgetId}/state, PUT | Use status value as `ACTIVE`. |

