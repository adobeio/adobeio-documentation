# Managing Webhooks and Subscriptions (Webhook APIs)

## On this page
- [Endpoints](#endpoints)
- [POST /webhooks ](#postwebhooks) Creates a new webhook.
- [GET /webhooks](#getwebhooks) Retrieves webhooks for a user.
- [GET /webhooks/{webhookId}](#getwebhookswebhookid) Retrieves details of a webhook.
- [PUT /webhooks/{webhookId}](#putwebhookswebhookid) Modifies an existing webhook.
- [PUT /webhooks/{webhookId}/state](#putwebhookswebhookidstate) Modifies an existing webhook&rsquo;s status(ACTIVE/INACTIVE).
- [DELETE /webhooks/{webhookId}](#deletewebhookswebhookid) Deletes a webhook.
- [Standard error codes in every API request](#standarderrorcodesineveryapirequest)
- [Standard headers in every API request](#standardheadersineveryapirequest)

Webhook APIs are the means by which your integration communicates with the Adobe Sign service about webhooks. Use the various API endpoints to create, delete, modify, and retrieve status information about your webhooks.

## Endpoints
Adobe Sign APIs include the following endpoints:

### POST /webhooks

<table>
  <thead>
    <tr>
      <th>Entity</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Description</td>
      <td>Creates a webhook</td>
    </tr>
    <tr>
      <td>Endpoint operation</td>
      <td><code>/webhooks</code></td>
    </tr>
    <tr>
      <td>OAuth scopes</td>
      <td><code>webhook_write</code></td>
    </tr>
    <tr>
      <td>Request object</td>
      <td><code>WebhookInfo:</code>
        <pre><code class="javascript language-javascript hljs">
{
   "name": "",
   "scope": "",
   "state": "",
   "webhookSubscriptionEvents": [
      ""
   ],
   "webhookUrlInfo": {
      "url": ""
   },
   "applicationDisplayName": "",
   "applicationName": "",
   "created": "",
   "id": "",
   "lastModified": "",
   "resourceId": "",
   "resourceType": "",
   "status": "",
   "webhookConditionalParams": {
      "webhookAgreementEvents": {
         "includeDetailedInfo": false,
         "includeDocumentsInfo": false,
         "includeParticipantsInfo": false,
         "includeSignedDocuments": false
      },
      "webhookMegaSignEvents": {
         "includeDetailedInfo": false
      },
      "webhookWidgetEvents": {
         "includeDetailedInfo": false,
         "includeDocumentsInfo": false,
         "includeParticipantsInfo": false
      }
   }
}</code></pre></td>
    </tr>
    <tr>
      <td>Response header</td>
      <td>Location Header specifying the resource location of the webhook</td>
    </tr>
    <tr>
      <td>Response content type</td>
      <td><code>application/json</code></td>
    </tr>
    <tr>
      <td>Response object </td>
      <td><code>WebhookCreationResponse</code>
        <pre><code class="javascript language-javascript hljs">{
   "id" : ""
}</code></pre></td>
    </tr>
    <tr>
      <td>HTTPS status code</td>
      <td>201</td>
    </tr>
    <tr>
      <td>Error code</td>
      <td>Please note that new errors could be returned from APIs or existing error codes can be evolved. Clients are expected to be prepared to do default handling for error scenarios which they do not understand.
        <table>
          <thead>
            <tr>
              <th>HTTPS status code</th>
              <th>Error code</th>
              <th>Message</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>400</td>
              <td><code>INVALID_ARGUMENTS</code></td>
              <td>One or more arguments to the method are invalid.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>INVALID_WEBHOOK_URL</code></td>
              <td>The Webhook URL specified is invalid.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>INVALID_RESOURCE_ID</code></td>
              <td>Resouce Id specified is invalid.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>INVALID_RESOURCE_TYPE</code></td>
              <td>The resource type is invalid.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>INVALID_WEBHOOK_CONDITIONAL_PARAMS</code></td>
              <td>The conditional parameters specified are invalid.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>MISSING_REQUIRED_PARAM</code></td>
              <td>The required parameters are missing. </td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>WEBHOOK_LIMIT_EXCEEDED</code></td>
              <td>This webhook can&rsquo;t be created. The events array <code>{events}</code> has reached the maximum number of active webhooks.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>DUPLICATE_WEBHOOK_CONFIGURATION</code></td>
              <td>There is already a webhook registered with same configuration.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>INVALID_WEBHOOK_STATE</code></td>
              <td>The webhook state specified is invalid.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>INVALID_WEBHOOK_SUBSCRIPTION_EVENTS</code></td>
              <td>One or more webhook subscription events specified are invalid.</td>
            </tr>
            <tr>
              <td>403</td>
              <td><code>WEBHOOK_CREATION_NOT_ALLOWED</code></td>
              <td>Webhook creation is not allowed.</td>
            </tr>
          </tbody>
        </table></td>
    </tr>
  </tbody>
</table>

This API will be used to create a webhook on a particular resouce in Adobe Sign.

- You need to register the webhook with your application and a user token using this API. 
- Group level webhooks can only be created by a Group admin and Account level webhooks can only be created by an Account admin.
- The user can customize the events for which the webhook is triggered in this call.

The `HTTP Location` header field is returned in the response to provide information about the location of a newly created resource. Multiple webhooks can be created on a single resource. _Also, multiple webhooks can share the same URL._

### GET /webhooks

<table>
  <thead>
    <tr>
      <th>Entity</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Description</td>
      <td>Get a list of webhooks created by the access token user.</td>
    </tr>
    <tr>
      <td>OAuth scopes</td>
      <td><code>webhook_read</code></td>
    </tr>
    <tr>
      <td>Query parameters</td>
      <td><p><code>showInactiveWebhooks: boolean</code>: A query parameter to fetch all the inactive webhooks along with the active webhooks.</p>
      <p><code>scope</code>: Scope of the webhook. The possible values are <code>ACCOUNT</code>, <code>GROUP</code>, <code>USER</code>, or <code>RESOURCE</code>.</p>
      <p><code>resourceType</code>: The type of resource on which webhook was created. The possible values are <code>AGREEMENT</code>, <code>WIDGET</code>, and <code>MEGASIGN</code>.</p>
</td>
    </tr>
    <tr>
      <td>Response content type</td>
      <td><code>application/json</code></td>
    </tr>
    <tr>
      <td>Response object </td>
      <td><code>WebhooksInfo</code>
         <pre><code class="javascript language-javascript hljs">{
   "userWebhookList": [
      {
         "applicationName": "Application for REST Swagger Documentation",
         "applicationId": "pUQL757H2R2",
         "id": "CBJCHBCAABAAtW5qb_gRDgssqn6tvLvCcav0VqYi0WTR",
         "name": "user level webhook",
         "lastModified": "2018-02-06T22:52:27-08:00",
         "scope": "USER",
         "webhookSubscriptionEvents": [
            "AGREEMENT_RECALLED"
         ],
         "webhookUrlInfo": {
            "url": "https://testUrl"
         },
         "status": "ACTIVE"
      },
      {
         "applicationName": "Application for REST Swagger Documentation",
         "applicationDisplayName ": "REST Swagger",
         "id": "CBJCHBCAABAAtW5qb_gRDgssqn6tvLvCcav0VqYi0WTR",
         "name": "",
         "lastModified": "2018-02-06T22:52:27-08:00",
         "scope": "RESOURCE",
         "resourceType": "AGREEMENT",
         "resourceId": "3dffwifvgvfguierfreokfperfprfppr",
         "webhookSubscriptionEvents": [
            "AGREEMENT_RECALLED"
         ],
         "webhookUrlInfo": {
            "url": "https://testResource"
         },
         "status": "ACTIVE"
      }
   ],
   "page": {
      "nextCursor": " "
   }
}
        </code></pre></td>
    </tr>
    <tr>
      <td>HTTPS status code</td>
      <td>200</td>
    </tr>
    <tr>
      <td>Error code</td>
      <td>Please note that new errors could be returned from APIs or existing error codes can be evolved. Clients are expected to be prepared to do default handling for error scenarios which they do not understand. 
        <table>
          <thead>
            <tr>
              <th>HTTPS status code</th>
              <th>Error code</th>
              <th>Message</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>400</td>
              <td><code>INVALID_ARGUMENTS</code></td>
              <td>One or more arguments to the method are invalid.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>INVALID_CURSOR<code></td>
              <td>The page cursor provided is invalid.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>INVALID_PAGE_SIZE<code></td>
              <td>Page size is either invalid or not within the permissible range.</td>
            </tr>
          </tbody>
        </table></td>
    </tr>
  </tbody>
</table>

### GET /webhooks/{webhookId}

<table>
  <thead>
    <tr>
      <th>Entity</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Description</td>
      <td>List details of a webhook.</td>
    </tr>
    <tr>
      <td>Endpoint operation</td>
      <td><code>/webhhooks/{webhookId}</code></td>
    </tr>
    <tr>
      <td>OAuth scopes</td>
      <td><code>webhook_read</code></td>
    </tr>
    <tr>
      <td>Response content type</td>
      <td><code>application/json</code></td>
    </tr>
    <tr>
      <td>Response object</td>
      <td><code>WebhookInfo</code>
        <pre><code class="javascript language-javascript hljs">{
    "scope": "",
    "webhookSubscriptionEvents": [
        ""
    ],
    "webhookUrlInfo": {
        "url": ""
    },
    "name": "",
    "status": "",
    "applicationDisplayName ": "",
    "applicationName": "",
    "created": "date",
    "id": "",
    "lastModified": "date",
    "resourceId": "",
    "resourceType": "",
    "webhookConditionalParams": {
        "webhookAgreementEvents": {
            "includeDetailedInfo": false,
            "includeDocumentsInfo": false,
            "includeParticipantsInfo": false,
            "includeSignedDocuments": false
        },
        "webhookMegaSignEvents": {
            "includeDetailedInfo": false
        },
        "webhookWidgetEvents": {
            "includeDetailedInfo": false,
            "includeDocumentsInfo": false,
            "includeParticipantsInfo": false
        }
    }
}</code></pre></td>
    </tr>
    <tr>
      <td>HTTPS status code</td>
      <td>200</td>
    </tr>
    <tr>
      <td>Error code</td>
      <td>Please note that new errors could be returned from APIs or existing error codes can be evolved. Clients are expected to be prepared to do default handling for error scenarios which they do not understand. 
        <table>
          <thead>
            <tr>
              <th><strong>HTTPS status code</strong></th>
              <th><strong>Error code</strong></th>
              <th><strong>Message</strong></th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>304</td>
              <td><code>RESOURCE_NOT_MODIFIED</code></td>
              <td>The resource is not modified.</td>
            </tr>
            <tr>
              <td>404</td>
              <td><code>INVALID_WEBHOOK_ID</code></td>
              <td>The <code>webhookId</code> specified is invalid.</td>
            </tr>
          </tbody>
        </table></td>
    </tr>
  </tbody>
</table>

### PUT /webhooks/{webhookId}

<table>
  <thead>
    <tr>
      <th>Entity</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Description</td>
      <td>This endpoint is used to update the webhook resource.</td>
    </tr>
    <tr>
      <td>Endpoint operation</td>
      <td><code>/webhooks/{webhookId}</code></td>
    </tr>
    <tr>
      <td>OAuth scopes</td>
      <td><code>webhook_write</code></td>
    </tr>
    <tr>
      <td>Request header</td>
      <td>Standard header. Additionally, If-Match headers, which will be processed as per the Concurrency section of the DC API Guidelines</td>
    </tr>
    <tr>
      <td>Request body</td>
      <td><code>WebhookInfo</code>
        <pre><code class="javascript language-javascript hljs">{
   "name": "",
   "scope": "",
   "state": "",
   "webhookSubscriptionEvents": [
      ""
   ],
   "webhookUrlInfo": {
      "url": ""
   },
   "applicationDisplayName": "",
   "applicationName": "",
   "created": "",
   "id": "",
   "lastModified": "",
   "resourceId": "",
   "resourceType": "",
   "status": "",
   "webhookConditionalParams": {
      "webhookAgreementEvents": {
         "includeDetailedInfo": false,
         "includeDocumentsInfo": false,
         "includeParticipantsInfo": false,
         "includeSignedDocuments": false
      },
      "webhookMegaSignEvents": {
         "includeDetailedInfo": false
      },
      "webhookWidgetEvents": {
         "includeDetailedInfo": false,
         "includeDocumentsInfo": false,
         "includeParticipantsInfo": false
      }
   }
}</code></pre></td>
    </tr>
    <tr>
      <td>Response content type</td>
      <td><code>application/json</code></td>
    </tr>
    <tr>
      <td>Response object </td>
      <td>Empty response</td>
    </tr>
    <tr>
      <td>HTTPS status code</td>
      <td>204</td>
    </tr>
    <tr>
      <td>Error code</td>
      <td>Please note that new errors could be returned from APIs or existing error codes can be evolved. Clients are expected to be prepared to do default handling for error scenarios which they do not understand.
        </p>
        <table>
          <thead>
            <tr>
              <th>HTTPS status code </th>
              <th>Error code</th>
              <th>Message</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>400</td>
              <td><code>MISSING_REQUIRED_PARAM</code></td>
              <td>Required parameters are missing.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>DUPLICATE_WEBHOOK_CONFIGURATION</code></td>
              <td>There is already a webhook registered with same configuration.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>INVALID_JSON</code></td>
              <td>An invalid JSON was specified.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>INVALID_WEBHOOK_CONDITIONAL_PARAMS</code></td>
              <td>The webhook conditional parameters specified are invalid.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>INVALID_WEBHOOK_SUBSCRIPTION_EVENTS</code></td>
              <td>One or more webhook subscription events specified are invalid.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>MISSING_IF_MATCH_HEADER</code></td>
              <td>The If-Match header missing.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>UPDATE_NOT_ALLOWED</code></td>
              <td>The webhook you are trying to update cannot be modified.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>WEBHOOK_LIMIT_EXCEEDED</code></td>
              <td>This webhook can&rsquo;t be activated. The resource has reached the maximum number of active webhooks.</td>
            </tr>
            <tr>
              <td>404</td>
              <td><code>INVALID_WEBHOOK_ID</code></td>
              <td>The <code>webhookId</code> is invalid.</td>
            </tr>
            <tr>
              <td>412</td>
              <td><code>RESOURCE_MODIFIED</code></td>
              <td>The resource is already modified with a newer version.</td>
            </tr>
          </tbody>
        </table></td>
    </tr>
  </tbody>
</table>

Only events and conditional parameters can be  modified. The webhook URL can&rsquo;t be modified once the webhook is created. The modification can also be done in the INACTIVE state.

### PUT /webhooks/{webhookId}/state

<table>
  <thead>
    <tr>
      <th>Entity</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Description</td>
      <td>This endpoint will update the state of a webhook identified by <code>webhookId</code> in the path.</td>
    </tr>
    <tr>
      <td>Endpoint operation</td>
      <td><code>/webhooks/{webhookId}/state</code></td>
    </tr>
    <tr>
      <td>OAuth scopes</td>
      <td><code>webhook_write</code></td>
    </tr>
    <tr>
      <td>Request header</td>
      <td>Standard header. Additionally, If-Match headers, which will be processed as per the Concurrency section of the DC API Guidelines.</td>
    </tr>
    <tr>
      <td>Request body</td>
      <td><code>WebhookInfo</code>
        <pre><code class="javascript language-javascript hljs">{
   "state": ""
}</code></pre></td>
    </tr>
    <tr>
      <td>Response content type</td>
      <td><code>application/json</code></td>
    </tr>
    <tr>
      <td>Response object</td>
      <td>Empty response</td>
    </tr>
    <tr>
      <td>HTTPS status code</td>
      <td>204</td>
    </tr>
    <tr>
      <td>Error code</td>
      <td>Please note that new errors could be returned from APIs or existing error codes can be evolved. Clients are expected to be prepared to do default handling for error scenarios which they do not understand.
        </p>
        <table>
            <thead>
                <tr>
                    <th>HTTPS status code</th>
                    <th>Error code</th>
                    <th>Message</th>
                </tr>
            </thead>
          <tbody>
            <tr>
              <td>400</td>
              <td><code>MISSING_REQUIRED_PARAM</code></td>
              <td>Required parameters are missing. </td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>DUPLICATE_WEBHOOK_CONFIGURATION</code></td>
              <td>There is already a webhook registered with same configuration.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>INVALID_JSON</code></td>
              <td>An invalid JSON was specified.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>MISSING_IF_MATCH_HEADER</code></td>
              <td>The If-Match header is missing.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>UPDATE_NOT_ALLOWED</code></td>
              <td>The webhook you are trying to update cannot be modified.</td>
            </tr>
            <tr>
              <td>400</td>
              <td><code>WEBHOOK_LIMIT_EXCEEDED</code></td>
              <td>This webhook can&rsquo;t be activated. The resource has reached the maximum number of active webhooks.</td>
            </tr>
            <tr>
              <td>404</td>
              <td><code>INVALID_WEBHOOK_ID</code></td>
              <td>The <code>webhookId</code> is invalid.</td>
            </tr>
            <tr>
              <td>412</td>
              <td><code>RESOURCE_MODIFIED</code></td>
              <td>The resource is already modified with a newer version.</td>
            </tr>
          </tbody>
        </table></td>
    </tr>
  </tbody>
</table>

### DELETE /webhooks/{webhookId}

<table>
  <thead>
    <tr>
      <th>Entity</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Description</td>
      <td><strong>This endpoint is used to delete the webhook resource. This is the terminal state of the webhook and the action is irreversible.</strong></td>
    </tr>
    <tr>
      <td>Endpoint operation</td>
      <td><code>/webhooks/{webhookId}</code></td>
    </tr>
    <tr>
      <td>OAuth scopes</td>
      <td><code>webhook_delete</code></td>
    </tr>
    <tr>
      <td>Request header</td>
      <td>Standard header</td>
    </tr>
    <tr>
      <td>HTTPS status code</td>
      <td>204</td>
    </tr>
    <tr>
      <td>Error code</td>
      <td>Please note that new errors could be returned from APIs or existing error codes can be evolved. Clients are expected to be prepared to do default handling for error scenarios which they do not understand.
        <table>
          <thead>
            <tr>
              <th>HTTPS status code </th>
              <th>Error code</th>
              <th>Message</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>403 </td>
              <td><code>FORBIDDEN</code></td>
              <td>Delete is not allowed.</td>
            </tr>
            <tr>
              <td>404</td>
              <td><code>INVALID_WEBHOOK_ID</code></td>
              <td>The webhookId is invalid.</td>
            </tr>
          </tbody>
        </table></td>
    </tr>
  </tbody>
</table>

### Standard error codes in every API request

Any API request may return any of these standard error codes:

| **HTTPS status code** | **Error code** | **Message** |
| --- | --- | --- |
| 400 | `BAD_REQUEST` | The request provided is invalid. |
| 400 | `INVALID_JSON` | An invalid JSON was specified. |
| 400 | `INVALID_ON_BEHALF_OF_USER_HEADER` | The value provided in the `x-on-behalf-of-user` header is in invalid format. |
| 400 | `INVALID_X_API_USER_HEADER` | The value provided in `x-api-user` header is in invalid format. |
| 400 | `MISC_ERROR` | Some miscellaneous error has occurred. |
| 401 | `UNAUTHORIZED` | You cannot work on behalf of this user. |
| 401 | `UNVERIFIED_USER` | The user has registered but has not verified their email address. The user must use the Adobe Sign web site to complete verification. |
| 401 | `NO_AUTHORIZATION_HEADER` | The authorization header was not provided. |
| 401 | `INVALID_ACCESS_TOKEN` | The access token provided is invalid or has expired. |
| 401 | `INVALID_USER` |An invalid user ID or email was provided in the  `x-user` header. |
| 401 | `INVALID_ON_BEHALF_OF_USER` | An invalid user ID or email was provided in the `x-on-behalf-of-user` header. |
| 403 | `API_TERMS_NOT_ACCEPTED` | Your account is locked because an administrator has not agreed to Adobe Sign&rsquo;s Terms of Use. Please contact your account administrator to activate your account. |
| 404 | `PERMISSION_DENIED` | The API caller does not have the permission to execute this operation. |
| 500 | `MISC_SERVER_ERROR` | Some miscellaneous server error has occurred. |

### Standard headers in every API request

Every API request will have the following standard headers. If Any API in the list above does not have one or more of the following headers, the API will explicitly document this fact.

| **Header Name** | **Description** |
| --- | --- |
| `AUTHORIZATION` | An access token with the correct scopes. |
| `x-api-user` | The userId or email of the API caller using the account or group token in the format  `userid:{userId}` **OR** `email:{email}.`  If it is not specified, then the caller is inferred from the token. |
| `x-on-behalf-of-user`  | Account sharing: The user on whose behalf the API caller is working. |