# Set up AEM for Adobe I/O Events

These instructions describe how to set up Adobe Experience Manager (AEM) for Adobe I/O events. 

## Introduction

Use Adobe I/O for notification of AEM events such as page or asset changes.

These instruction include the following sections:

1. What's Needed
1. Register an AEM Event Publisher
1. Register an AEM Consumer App

## What's Needed

To complete this solution, you will need authorization to use the following services:

*   Adobe Marketing Cloud Activation Core Services, with administrative permissions
*   AEM instance setup with version 6.2 or 6.3, with administrative permissions
*   An [Adobe ID](https://helpx.adobe.com/x-productkb/global/adobe-id-account-change.html), if you do not already have one
*   Adobe I/O Console setup, with administrative permissions for your enterprise organization

### Administrative Permissions

If you do not have administrative permissions, please contact ioevents-beta@adobe.com. After requesting administrative permissions, watch for an email from Adobe Systems Incorporated, as shown:

![admin rights 2](https://user-images.githubusercontent.com/29133525/30258738-46dfb044-9679-11e7-9d0b-f724c32dacac.png)


System Requirement
Register an AEM Event Publisher
Install aem-event-proxy Package
Configure Adobe I/O Events Cloud Service
Configure oAuth/IMS 
Creating Adobe I/O Console Integration
Configuring Adobe I/O Events as one of the Cloud Services in AEM
Advanced Adobe I/O Events configurations
AEM Health Check and Configuration Check
AEM Health Check
Register an AEM Event Consumer App
Set up Webhook
Test Integration 
Register Webhook in Integration
Webhook Health Check
Test the WebhookSubcription
 

 
## Register an AEM Event Publisher
To register an AEM Event Publisher:

1. Install the AEM Event Proxy Package
1. Configure Adobe I/O Events Cloud Service
1. Perform an AEM Health and Configuration Check



### Install the AEM Event Proxy Package

To install the AEM Event Proxy Package:

1. Download the latest version of the package from the [pre-release site](https://artifactory.corp.adobe.com/artifactory/maven-cloud-action-local/com/day/cq/dam/aem-event-proxy/).

1. In AEM, click the **Tools** icon, then click **Deployment** and then **Packages**.

![package manager navi](https://user-images.githubusercontent.com/29133525/31083270-7f0f6ece-a74e-11e7-87d8-da73464104d7.png)

1. In **Package Manager**, click **Upload Package**. Click the **Browse** button and navigate to the package zip file. Click **OK**.


Note: If you have an older version of the package, delete it to avoid potential conflicts. You can delete it from the following location: 

```
crx/de/index.jsp#/apps/eventproxy/install
```
1. Click **Install**.

![package manager ui](https://user-images.githubusercontent.com/29133525/31083485-0b10be82-a74f-11e7-85e4-4dae028946b9.png)

1. In the **Install Package** dialog box, select **Merge** from the **Access Control Handling** drop-down list and click **Install**.

![install package](https://user-images.githubusercontent.com/29133525/31085363-242d22e8-a754-11e7-9927-a8c7c43d8184.png)

Watch the **Activity Log** to verify that the package is installed. If installed, log reports that it is imported.













Install aem-event-proxy Package

Install Package
Please download the latest version of the package from the prerelease site. 
Upload and install the package
Navigate to the package manager of your local AEM instance (/crx/packmgr/index.jsp)
Upload
Click “Upload Package”
Note: if you have an older version, to avoid conflict, please delete it from crx/de/index.jsp#/apps/eventproxy/install
Install
Once the package is uploaded, click “Install” (right hand upper corner)
In the Install Dialog Menu, click on Advanced Settings
Select Merge in the Access Control Handling dropdown select
Click on Install
Watch the activity log, it should end with Package installed in some_ms.
AEM Package Installation Documentations
 
Note:
If you are doing a package upgrade :
please delete the old version (the old jars) from `/apps/eventproxy/install`
please verify the ACL (see below) 
If the Access Control Handling are properly applied, the eventproxy-service user should be added to
/home/users/system/eventproxy/eventproxy-service with jrc:read and rep:write authorizations
/etc/cloudservices/eventproxy with jrc:read and rep:write authorizations
/content with jrc:read 
You can validate this by checking the permissions for eventproxy-service user group at /useradmin
AEM User Administration Documentations
 
If you don't see the correct permission, you can also manually updated the permission by going to CRXDE Lite at /crx/de/index.jsp#/etc/cloudservices/eventproxy

 
Configure Adobe I/O Events Cloud Service
Configure oAuth/IMS 
Create Certificate
You can create this certificate wherever you'd like
Use openssl to create an RSA private/public certificate
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate_pub.crt
To add the private key and the signed certificate to a pkcs#12 file
openssl pkcs12 -keypbe PBE-SHA1-3DES -certpbe PBE-SHA1-3DES -export -in certificate_pub.crt -inkey private.key -out author.pfx -name "author"
Note: you will be asked to create an export password, remember it for later use
To create a keystore from the generated keys, run the following command:
cat private.key certificate_pub.crt > private-key-crt
Use the following command to set the alias as eventproxy and a non-empty (I choose admin) keystore password.
openssl pkcs12 -export -in private-key-crt -out keystore.p12 -name eventproxy -noiter -nomaciter
Add the keystore to eventproxy-service user group
Open User Admin
go to /libs/granite/security/content/useradmin.html
open eventproxy-service user group
Create Key Store
Click Create Key Store (under Account Settings), and create a key store.
Add the Keystore
Click Manage Key Store, and then click Add Private Key from Key Store file to add the keystore.p12 file.
Set the key pair alias to eventproxy(or the other on you used in the aboveopensslpkcs12 command.
Provide the keystore password(the password you provided when generating the keystore), private key password, and the private key alias eventproxy.
Note: don't be creative with the alias name, just use eventproxy

 
Configure AEM Link Externalizer
the AEM Link Externalizer name can be author or any other config alias that configured before at /system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl
please don’t use plain “localhost”, as this causes confusion when because it is the default localhost name and when other people also use it, you won't be able to tell which instance is yours
the base url that you configured there will appear as such on the adobe I/O consoled

Creating Adobe I/O Console Integration
Go to https://adobe.io/console
Click New Integration.
Select the following options:
"Access an API"
select "Adobe I/O events"
New integration
Specify a name for the integration gateway and add a description.
Upload the file certificate_pub.crt to the integration service.

Configuring Adobe I/O Events as one of the Cloud Services in AEM
Open the Cloud Services console (/libs/cq/core/content/tools/cloudservices.html)
Click "Configure Now" under Adobe I/O Events to Create a new configuration.


 
With the following content in the configuration
Day CQ Link externalizer name: author (or any other alias you configured in the AEM Link Externalizer see above)
API key: the_one_from_adobe_io_console
Technical Account ID: the_one_from_adobe_io_console
Organization ID: the_one_from_adobe_io_console
Client Secret: the_one_from_adobe_io_console
 
Advanced Adobe I/O Events configurations
Note that you can also do advanced configuration through the osgi configuration panel, search for: Adobe I/O Events' CSM Registration :
For all the Adobe I/O event types known to the Adobe I/O Event Model

You can change/edit 
* the OSGI event `topic` 
* the `filter` used in the OSGI event handler
* as well as the observed path
  * defaulted to `/content/dam` for `asset` events
  * and `/content` for `page` events
  
The various OSGI event handler will intercept the events according to these value 
and then map these OSGI events to the Adobe I/O Event Model before forwarding them to Adobe I/O.
 
 
AEM Health Check and Configuration Check
AEM Health Check
If you have the configuration done properly, then you are all set, you can trigger AEM events and register webhooks to listen to them (see the next section). If you have issues figuring the right configuration out, or if you want to double check or reload/reset the configuration you may use the various Sling Health Check available in this bundle.
First, check that all your configurations are proper and properly loaded: execute the Health Check tagged with eventproxy, conf
http://localhost:4502/system/console/healthcheck?tags=eventproxy%2C+conf&debug=true&forceInstantExecution=true&overrideGlobalTimeout=40000
Second, check that this AEMinstanceisisableto exchange JWT exchange tokens with Adobe I/O IMS (Identity Management System) execute the Health Check tagged with eventproxy,ims
http://localhost:4502/system/console/healthcheck?tags=eventproxy%2C+ims&debug=true&forceInstantExecution=true&overrideGlobalTimeout=40000
this will validate that all your IMS related configurations are correct and working as expected (Meaning you got the eventproxy-service user KeyStore properly configured, as well as all the adobe.io console originated API key, Technical Account ID, Organization ID and Client Secret)
Third, check that the event metadata and the provider associated with this AEM instance are registered in Adobe I/O CSM (Channel & Subscription Management), execute the Health Check tagged with eventproxy,csm
http://localhost:4502/system/console/healthcheck?tags=eventproxy%2C+csm&debug=true&forceInstantExecution=true&overrideGlobalTimeout=40000
this will validate that your AEM instance is successfully registered as an event provider against Adobe IO CSM (Channel & Subscription Management),

 
Register an AEM Event Consumer App
Set up Webhook
Your webhook should be able to accept and reply a challenge http request parameter sent by CSM (Channel & Subscription Management).
For instance: to create a webhook at webscript.io, use this script to configure it
local request  = request
local method = request.method
if method == "GET" then
    if (request.query["challenge"]) then
        log("got challenge: " .. request.query["challenge"])
    else
        log("no challenge")
    end
    return request.query["challenge"]
end
if method == "POST" then
    log("webhook invoked with: " .. request.body)
    return request.body
end
 
Test Integration 
Register Webhook in Integration
Once you have your webhook ready, use the adobe.io console to register it:
Go to https://adobe.io/console
Click New Integration
Select Subscribe to events
Click Continue
Select AEM-<your-day-cq-link-externalizer-base-server-url>
Click Continue
Select Create new integration
Fill in the Configure Integration detail form
Click the Add webhook button
Fill-intheAdd a new Webhook form
Select the events you want to subscribe to
Click Save
Click Create newInrtegration
Click Continue to Integration detail

Note that if you went through an upgrade fro version 0.16 of the package to 0.18, your AEM registered event provider id has changed
as a result, you will need to re-register your webhook and use the latest provider (his label should start with AEM- and not with AEM_DAM, used in version 0.16)
Webhook Health Check
Check that events are sent to and received by Adobe I/O event receiver (the AEM ingress adapater): execute the Health Check tagged with eventproxy, eventreceiver
http://localhost:4502/system/console/healthcheck?tags=eventproxy%2C+evre&debug=true&forceInstantExecution=true&overrideGlobalTimeout=40000
Test the WebhookSubcription
by publishing/unpublishing AEM pages
by editing/adding/removing an asset in the AEM DAM
or by using the aem asset API [1] https://docs.adobe.com/docs/en/aem/6-2/develop/extending/mac-api-assets.html
You can see responses in your webhook
 
 
 
