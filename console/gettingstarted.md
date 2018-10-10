# Adobe I/O Authentication Overview
Adobe is committed to privacy and security. Nearly all Adobe services require your application to authenticate through the Adobe Identity Management System (IMS) to receive client credentials. The client credentials determine the access and permissions granted to your application.

Any API that accesses a service or content on behalf of an end user authenticates using the OAuth and JSON Web Token standards.

* Your application must be registered through the Adobe I/O Console. The I/O Console is where you can generate an API Key, an important requirement to obtain client credentials.
* If your integration needs to access content or a service on behalf of an end user, that user must be authenticated as well. Your integration will need to pass the OAuth token granted by the Adobe IMS.
* For service-to-service integrations, you will also need a JSON Web Token (JWT) that encapsulates your client credentials and authenticates the identity of your integration. You exchange the JWT for the OAuth token that authorizes access.
 

Use the [Adobe I/O Console](https://console.adobe.io/) to obtain client credentials by creating a new **Integration**. When you create an Integration, you are assigned an **API Key** (client ID) and other access credentials. You can then obtain a secure access token from Adobe for each API session.

An integration can be subscribed to one or more services. In many cases, you will use the same client credentials to access multiple Adobe services. In addition to APIs, you may also subscribe your integration to I/O Events so that your applications can access content and services in real-time.

## Creating an Integration
There are different options for integrations, based on the type of application you are building and the services you need to access. Many services and events are only available through a specific type of integration. Check the service or product documentation to learn more.

* **OAuth Authentication**
If your application makes API requests to a service on behalf of an end user, or if it needs to access end-user content, your users will first need to log in with their Adobe ID. Following a successful user log in, your application will receive an access token unique to that user and your application. Your application must pass this access token which each API request.

User-based services, such as the Creative SDK and Typekit, can only be added to an OAuth integration.The Creative SDK provides a User Auth UI component for mobile and web that makes it easy to include the Adobe ID workflow in your application.

To create an Integration of this type, sign in to the [Adobe I/O Console](https://console.adobe.io/) with your Adobe ID, and choose the service you want to integrate with your app. For more information, see [OAuth Authentication](oauth_workflow.html).

* **Service Account Authentication**
If your application makes API requests to a service on behalf of itself or an enterprise organization, you will need to configure a Service Account integration. Service Accounts are similar to user accounts, but they are unique to your application and have additional security requirements.

To obtain access tokens for your integration, you must first create a JSON Web Token (JWT) that encapsulates your client credentials. For each API session, you will exchange your JWT for an access token from Adobe IMS. This token identifies your integration and grants access to the services you have configured.

Application and organization-based services, such as Adobe Target, Adobe Launch, and Document Cloud PDF Services, can only be added to a Service Account integration.

To create an integration of this type, sign in to the [Adobe I/O Console](https://console.adobe.io/) with your Enterprise ID. Your Enterprise ID must have administrative privileges for your organization to be able to create a new Service Account integration. If you do not have the required permissions, contact an IT Administrator at your company for help. This is typically the person who distributes Creative Cloud, Acrobat or Marketing Cloud licenses within your company.

For more information, see [Service Account Authentication](jwt_workflow.html).

* **API Key Integration**
Some services from Adobe do not require either user-based or application-based authentication. These APIs and events can be accessed by any application that simply specifies an API Key (Client ID). Additional client credentials, such as the Client Secret, are not required.

On its own, this integration type is the least secure and API requests are considered anonymous. In many cases, the service will use the passed API Key to enforce permissions outside of the Adobe IMS.

The Adobe Stock Search API can be accessed through any of the three types of integration. When you access the API with just an API Key, the returned search results are generic, and do not take into account the user or application who made the request.

To create an integration of this type, sign in to the [Adobe I/O Console](https://console.adobe.io/) with your Adobe ID or Enterprise ID. Your Enterprise ID does not require additional permissions to create an API Key integration. For more information, see [API Key Integration](api_key_workflow.html).

## Prerequisites
* In order to create a new Service Account integration, you must provide a public key certificate, also known as a digital certificate. This electronic document proves the ownership of a public key and validates the identity of your integration. You may create your own digital certificate or purchase one through a 3rd-party certificate authority. To learn more, see [Public Key Certificates for JWT](createcert.html).
* For OAuth integrations, you must provide the **default redirect URI** and **redirect URI patterns**. Upon completion of the Adobe ID login flow, Adobe redirects the user to the specified URL.
* For an integration that consumes Adobe I/O Events, you must provide at least one **webhook**, a URL that receives event notifications.
