# Webhook Events

## On this page
- [Notification payload for events](#notificationpayloadforevents)
- [Receiving a webhook notification](#receivingawebhooknotification)
- [Reliable delivery of webhook notifications](#reliabledeliveryofwebhooknotifications)
- [Payload info](#payloadinfo)
- [Webhook uniqueness](#webhookuniqueness)
- [SuperSet events](#supersetevents)

Events are the reason for webhooks: webhooks are a way for your Adobe Sign integration to listen for and respond to Adobe Sign events. This page provides the info you need to determine what events your integration will listen for and respond to, and what details are included in event notifications so you can design your integration to extract the info you need from an event.

## Notification payload for events

When configuring a webhook, you can choose which events for which you would like to receive payloads. Subscribing only to the specific events is useful for limiting the number of HTTPS requests to the server. You can change the list of subscribed events through the API or UI anytime.

Currently webhooks can be created only for agreement, widget, and megasign events. Each event has a similar JSON schema based on the resource type, but a unique payload object that is determined by its event type. The payload will be returned in JSON format.

This table displays the events and payload which will be sent in webhook notifications.

| **Event type** | **Description and payload** |
| --- | --- |
| `AGREEMENT_ACTION_COMPLETED` | When an agreement is signed by the participant. [View Payload](webhook_events/agreement_action_completed.md) |
| `AGREEMENT_ACTION_DELEGATED` | When an agreement is delegated by the participant. [View Payload](webhook_events/agreement_action_delegated.md) |
| `AGREEMENT_ACTION_REPLACED_SIGNER` | When an agreement&rsquo;s signer is replaced. [View Payload](webhook_events/agreement_action_replaced_signer.md) |
| `AGREEMENT_ACTION_REQUESTED` | When an agreement action is requested. [View Payload](webhook_events/agreement_action_requested.md) |
| `AGREEMENT_AUTO_CANCELLED_CONVERSION_PROBLEM` | When an agreement is auto-cancelled due to a conversion problem. [View Payload](webhook_events/agreement_auto_cancelled_conversion_problem.md) |
| `AGREEMENT_CREATED` | When an agreement is created. [View Payload](webhook_events/agreement_created.md) |
| `AGREEMENT_DOCUMENTS_DELETED` | When agreement documents are deleted. [View Payload](webhook_events/agreement_documents_deleted.md) |
| `AGREEMENT_EMAIL_BOUNCED` | When an agreement email gets bounced. [View Payload](webhook_events/agreement_email_bounced.md) |
| `AGREEMENT_EMAIL_VIEWED` | When agreement signing email is viewed by the signer. [View Payload](webhook_events/agreement_email_viewed.md) |
| `AGREEMENT_EXPIRED` | When an agreement expires. [View Payload](webhook_events/agreement_expired.md) |
| `AGREEMENT_KBA_AUTHENTICATED` | When an agreement KBA is authenticated. [View Payload](webhook_events/agreement_kba_authenticated.md) |
| `AGREEMENT_MODIFIED` | When an agreement is modified. [View Payload](webhook_events/agreement_modified.md) |
| `AGREEMENT_OFFLINE_SYNC` | When an agreement is synced offline. [View Payload](webhook_events/agreement_offline_sync.md) |
| `AGREEMENT_RECALLED` | When an agreement is cancelled. [View Payload](webhook_events/agreement_recalled.md) |
| `AGREEMENT_REJECTED` | When an agreement is rejected by the participant. [View Payload](webhook_events/agreement_rejected.md) |
| `AGREEMENT_SHARED` | When an agreement is shared. [View Payload](webhook_events/agreement_shared.md) |
| `AGREEMENT_UPLOADED_BY_SENDER` | When an agreement is uploaded by the sender. [View Payload](webhook_events/agreement_uploaded_by_sender.md) |
| `AGREEMENT_USER_ACK_AGREEMENT_MODIFIED` | User acknowledgement when an agreement is modified. [View Payload](webhook_events/agreement_user_ack_agreement_modified.md) |
| `AGREEMENT_VAULTED` | When an agreement is vaulted. [View Payload](webhook_events/agreement_vaulted.md) |
| `AGREEMENT_WEB_IDENTITY_AUTHENTICATED` | When an agreement web identity is authenticated. [View Payload](webhook_events/agreement_web_identity_authenticated.md) |
| `AGREEMENT_WORKFLOW_COMPLETED` | When an agreement workflow is completed. [View Payload](webhook_events/agreement_workflow_completed.md) |
| `LIBRARY_DOCUMENT_AUTO_CANCELLED_CONVERSION_PROBLEM` | When a library document is auto-cancelled due to a conversion problem. [View Payload](webhook_events/library_document_auto_cancelled_conversion_problem.md) |
| `LIBRARY_DOCUMENT_CREATED` | When a library document is created. [View Payload](webhook_events/library_document_created.md) |
| `LIBRARY_DOCUMENT_MODIFIED` | When a library document is modified. [View Payload](webhook_events/library_document_modified.md) |
| `MEGASIGN_CREATED` | When a megaSign is created. [View Payload](webhook_events/megasign_created.md) |
| `MEGASIGN_RECALLED` | When a megaSign is recalled. [View Payload](webhook_events/megasign_recalled.md) |
| `MEGASIGN_SHARED` | When a megaSign is shared. [View Payload](webhook_events/megasign_shared.md) |
| `WIDGET_ENABLED` | When a widget is enabled. [View Payload](webhook_events/widget_enabled.md) |
| `WIDGET_DISABLED` | When a widget is disabled. [View Payload](webhook_events/widget_disabled.md) |
| `WIDGET_CREATED` | When a widget is created. [View Payload](webhook_events/widget_created.md) |
| `WIDGET_MODIFIED` | When a widget is modified. [View Payload](webhook_events/widget_modified.md) |
| `WIDGET_SHARED` | When a widget is shared. [View Payload](webhook_events/widget_shared.md) |
| `WIDGET_AUTO_CANCELLED_CONVERSION_PROBLEM` | When a widget is auto-cancelled due to a conversion problem. [View Payload](webhook_events/widget_auto_cancelled_conversion_problem.md) |

### Payload info

Webhook notification payloads will be delivered using the application/json content type.

The payload object contains all the relevant information about what just happened, including the type of event and the data associated with that event. Adobe Sign then sends the payload object, via an HTTP POST request, to any endpoint URLs that you have defined as webhook URLs. The payload size is restricted to 10 MB.

If an event generates a larger payload, a webhook will triggered but the conditional parameters attributes, if they’re in the request, will be removed to reduce the size of the payload. Also a **conditionalParametersTrimmed** array object will be included in the response for this case to tell the client which conditionalParameters info is removed.

The truncation will be done in the following order:

-   **includeSignedDocuments**
-   **includeParticipantsInfo**
-   **includeDocumentsInfo**
-   **includeDetailedInfo**

Signed Documents(base 64 encoded format) will be truncated first if size exceeds, followed by participant info, document info and finally detailed info will be truncated.

This may happen, for example, on an agreement completion event if it includes signed document (base 64 encoded format) as well or for an agreement with multiple form fields.

### Basic webhook payload for all notifications

All events will include the following common attributes in their payloads. Additional parameters returned along with basic agreement/widget/megasign info in the payload JSON object for particular keys are defined in the payload specificiations for each event; see [Notification payload for events](#notificationpayloadforevents) for links.

| Parameter name | Type | Description | Possible values |
| --- | --- | --- | --- |
| `webhookId` | String | Webhook identifier of the webhook for which the notification is being sent |   |
| `webhookname` | String | Name of the webhook which was provided while creating a webhook |   |
| `webhookNotificationId` | String | The unique identifier of the webhook notification. This will be helpful in identifying duplicate notifications, if any. |   |
| `webhookNotificationApplicableUsers` | Object | An array of the details of the users for which this notification is delivered. For example: Say User A and User B are in a Group G1. Say User C is in Group G2. Say both these groups and all 3 users are in Account A. Assume, group level &ldquo;webhook W1&rdquo; is registered on Group G1 and group level &ldquo;webhook W2&rdquo; is registered on Group G2. Now an agreement is sent by User A and to User B. And User B delegates the signing to User C. In the above case, the sign will generate only two notifications (corresponding to W1 and W2) for the delegation event. Current field for W1 notification will be an array of details of User A and User B. Current field for W2 notification will be an array of details of User C. |   |
| `webhookUrlInfo` | Object | URL on which this HTTPS POST notification is triggered. |   |
| `webhookScope` | String | Scope of the webhook | `ACCOUNT`,`GROUP`,`USER`,`RESOURCE` |
| `event` | String | Event for which the webhook notification is triggered | `AGREEMENT_CREATED` |
| `eventDate` | String | Timestamp of when the event happened. | Example value: 2018-08-09T12:01:00Z |
| `eventResourceParentType` | enum | In case of agreements, it is possible that the agreement is created by signing a widget or while creating a megasign. This field informs about such cases. _Only added in payloads of agreement type resources._ | `WIDGET`,`MEGASIGN` |
| `eventResourceParentId` | String | Unique identifier of the widget or megasign from which this agreement is created. _Only added in payloads of agreement type resources._ |   |
| `subEvent` | String | Subevent for which the webhook notification is triggered. _This field is event specific and returned with only few events, please look into individual event for the details_ |   |
| `eventResourceType` | String | The resource type on which the event is triggered. | `AGREEMENT`,`WIDGET`,`MEGASIGN` |
| `participantRole` | String | Role assumed by all participants in the participant set to which the participant belongs (signer, approver etc.). This is the role of the `participantUser`. This key will be returned only for the following events: `AGREEMENT_WORKFLOW_COMPLETED`, `AGREEMENT_ACTION_COMPLETED`, `AGREEMENT_ACTION_DELEGATED`, `AGREEMENT_ACTION_REQUESTED` | `SIGNER`, `DELEGATE_TO_SIGNER`, `APPROVER`, `DELEGATE_TO_APPROVER`, `ACCEPTOR`, `DELEGATE_TO_ACCEPTOR`, `FORM_FILLER`, `DELEGATE_TO_FORM_FILLER`, `CERTIFIED_RECIPIENT`, `DELEGATE_TO_CERTIFIED_RECIPIENT` or `SHARE` |
| `actionType` | String | This key will be returned only with the event `AGREEMENT_ACTION_COMPLETED`. |   |
| `participantUserId` | String | _This field is event-specific; please look into the individual event for details_ |   |
| `participantUserEmail` | String | _This field is event-specific; please look into the individual event for details_ |   |
| `actingUserId` | String | _This field is event-specific; please look into the individual event for details_ |   |
| `actingUserEmail` | String | _This field is event-specific; please look into the individual event for details_ |   |
| `initiatingUserId` | String | _This field is event-specific; please look into the individual event for details_ |   |
| `initiatingUserEmail` | String | _This field is event-specific; please look into the individual event for details_ |   |
| `actingUserIpAddress` | String | IP address of user that triggered the event | 
| `agreement` | Agreement | Information about the agreement on which the event occurred. This key will be returned only if the event is an agreement event. |   |
| `widget` | Widget | Information about the widget on which the event occurred. This key will be returned only if the event is a widget event. |   |
| `megasign` | MegaSign | Information about the megaSign on which the event occurred. This key will be returned only if the event is a megaSign event. |   |

### WebhookNotificationApplicableUsers
| Parameter name | Type | Description | Possible values |
|---|---|---|---|
| `id` | String	| The unique identifier of the user for which the notification is applicable. |   |
| `email` | String |Email address of the user for which the notification is applicable. |   |
| `role` | enum	| Role of the user in the workflow.	| `SIGNER`, `DELEGATE_TO_SIGNER`, `APPROVER`, `DELEGATE_TO_APPROVER`, `ACCEPTOR`, `DELEGATE_TO_ACCEPTOR`, `FORM_FILLER`, `DELEGATE_TO_FORM_FILLER`, `CERTIFIED_RECIPIENT`, `DELEGATE_TO_CERTIFIED_RECIPIENT`, or `SHARE`
| `payloadApplicable` | boolean | Indicates whether the payload attached to this notification is fetched in the context of this user or not. The boolean will be true for one and only one of the users in the `webhookNotificationApplicableUsers` array. |   |

### WebhookUrlInfo

| Parameter name | REST object | Description | Sample value |
| --- | --- | --- | --- |
| `Url` | String | HTTPS URL of the webhook | `https://example.com/callback?guid=test` |

### Agreement

This is the minimal info which will be returned for an agreement event if all the conditional parameters are set to false while creating webhooks.

| Parameter name | Type | Description | Possible enums |
| --- | --- | --- | --- |
| `id` | String | The unique identifier of agreement that can be used to query status and download signed documents. |   |
| `name` | String | The name of the agreement that will be used to identify it, in emails and on the website. |   |
| `status` | Enum | The current status of the agreement.  | `OUT_FOR_SIGNATURE`, `SIGNED`, `APPROVED`, `ACCEPTED`, `DELIVERED`, `FORM_FILLED`, `ABORTED`, `EXPIRED`, `OUT_FOR_APPROVAL`, `OUT_FOR_ACCEPTANCE`, `OUT_FOR_DELIVERY`, `OUT_FOR_FORM_FILLING`, or `CANCELLED` |

### Widget

This is the minimal info which will be returned for a widget event if all the conditional parameters are set to false while creating webhooks.

| Parameter name | Type | Description | Possible enums |
| --- | --- | --- | --- |
| `id` | String | The unique identifier of widget that can be used to retrieve the data entered by the signers |   |
| `name` | String | The name of the widget that will be used to identify it, in emails, website, and other places |   |
| `status` | Enum | The current status of the widget  | `DRAFT` or `ACTIVE` or `AUTHORING` |

### Library Document

This is the minimal info which will be returned for a Library Document  event if all the conditional parameters are set to false while creating webhooks.

| Parameter name | Type | Description | Possible enums |
| --- | --- | --- | --- |
| `id` | String | The unique identifier that is used to refer to the library template. |   |
| `name` | String | The name of the library template that will be used to identify it, in emails and on the website. |   |	
| `status` | Enum | The current status of the library document. | `AUTHORING`, `ACTIVE` or `REMOVED` |

### MegaSign

This is the minimal info which will be returned for a MegaSign event if all the conditional parameters are set to false while creating webhooks.

| Parameter name | Type | Description | Possible enums |
| --- | --- | --- | --- |
| `id` | String | The unique identifier of the agreement; it can be used to query status and download signed documents. |   |
| `name` | String | The name of the agreement that will be used to identify it, in emails and on the website. |   |
| `status` | Enum | The current status of the agreement. | `OUT_FOR_SIGNATURE`, `SIGNED`, `APPROVED`, `ACCEPTED`, `DELIVERED`, `FORM_FILLED`, `ABORTED`, `EXPIRED`, `OUT_FOR_APPROVAL`, `OUT_FOR_ACCEPTANCE`, `OUT_FOR_DELIVERY`, `OUT_FOR_FORM_FILLING`, or `CANCELLED` |
| `payloadApplicable` | boolean | Indicates whether the payload attached to this notification is fetched in the context of this user or not. The boolean will be true for one and only one of the users in the webhookNotificationApplicableUsers array. |   |

For different MegaSign events, the detailed agreement info, participant info, document info, and the signed document will be returned based on the conditional parameters specified during webhook creation.

## Receiving a webhook notification

Once your webhook URL is added, your app will start receiving "notification requests" every time a subscribed event is triggered in Sign. A notification request is an HTTPS POST request with a JSON body. The request&rsquo;s POST parameters will contain JSON data relevant to the event that triggered the request.

Additionally, we perform an implicit verification of intent in each webhook notification request that we send to the webhook URL. Thus, every notification request will also include a header called `X-AdobeSign-ClientId` that includes client id of the application that created the webhook. We will consider the webhook notification successfully delivered, if an only if success response **(2XX response code)** is returned  and  the header **(X-AdobeSign-ClientId)** is echoed back in response HTTP header or in a JSON response body with key as **xAdobeSignClientId** and value as the same client id, otherwise we will retry to deliver the notification to the webhook URL until the retries are exhausted.

## Reliable delivery of webhook notifications

Adobe Sign incorporates an advanced, reliable strategy for delivery of webhook notifications. If there is an outage on the receiving end (suppose, for instance, the customer’s server is down), Adobe Sign will retry the notification at a later time—minutes or hours later.

Undelivered events will be persisted in a retry queue and a best effort will be made over the next 72 hours to deliver the notifications in the order they occurred to the client-provided endpoints. The strategy for retrying delivery of notifications is a doubling of time between attempts, starting with a one-minute interval and doubling the interval with each attempt, up to a maximum interval of 12 hours, resulting in 15 retries in the space of 72 hours. If the webhook fails to respond and either the maximum retry time or the maximum retry interval is exceeded, the webhook will be disabled. No notifications will be sent to the webhook URL until the webhook is activated again; all the notifications between the time the webhook is disabled and enabled again will be lost.

## Webhook uniqueness 

Adobe Sign does not allow the creation of duplicate webhooks. The uniqueness of a webhook is based on a combination of the following attributes:

-   Subscription event
-   URL
-   Scope/ResoureType
-   Resource ID
-   Application ID/Client ID that you are using for the API call
-   Creator of webhook (not considered in the cases of group level and account
    level webhooks)

If a webhook’s status is changed from INACTIVE to ACTIVE and another webhook with a similar configuration already exists in the ACTIVE state, the activation call will fail.

### SuperSet events

If you want to subscribe for all the agreement, widget or megaSign events, you can subscribe to the following events:

| Event type | Description |
| --- | --- |
| `AGREEMENT_ALL` | All the supported agreement events. If new agreement events are added in future, those events will be taken care automatically. |
| `LIBRARY_ALL` | For all the supported library document events. If new library document events are added in future that are supported for webhooks, those events will be taken care of automatically. |
| `WIDGET_ALL`  | All the supported widget events. If new widget events are added in future, those events will be taken care automatically. |
| `MEGASIGN_ALL` | All the supported megasign events. |
