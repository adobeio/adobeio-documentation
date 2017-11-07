# Set up AEM for Adobe I/O Events

These instructions describe how to set up Adobe Experience Manager (AEM) for Adobe I/O events. You can use Adobe I/O for notification of  AEM events, such as page or asset changes.

1. [Introduction](#Introduction)

1. [Set Up Products](#Set-Up-Products)

1. [Use Adobe I/O](#Use-Adobe-I/O)

1. [Watch the Solution Work](#Watch-It-Work)

## <a name="Introduction">Introduction</a>

Before setting up and using AEM with Adobe I/O, you will need to do the following:

1. [Obtain Authorization](#Obtain-Authorization)
1. [Register an AEM Event Publisher](#Register-an-AEM-Event-Publisher)
1. [Register an AEM Event Consumer App](#Register-an-AEM-Event-Consumer-App)

### <a name="Obtain-Authorization">Obtain Authorization</a>

To complete this solution, you will need authorization to use the following services:

*   Adobe Marketing Cloud Activation Core Services, with administrative permissions
*   AEM instance setup with version 6.2 or 6.3, with administrative permissions
*   An [Adobe ID](https://helpx.adobe.com/x-productkb/global/adobe-id-account-change.html), if you do not already have one
*   Adobe I/O Console setup, with administrative permissions for your enterprise organization. If you do not have administrative permissions, please contact ioevents-beta@adobe.com. After requesting administrative permissions, watch for an email from Adobe Systems Incorporated, as shown:

![admin rights 2](https://user-images.githubusercontent.com/29133525/30258738-46dfb044-9679-11e7-9d0b-f724c32dacac.png)
 
### <a name="Register-an-AEM-Event-Publisher">Register an AEM Event Publisher</a>
To register an AEM Event Publisher:

1. [Install the AEM Event Proxy Package](#Register-an-AEM-Event-Publisher)
1. [Configure Adobe I/O Events Cloud Service]
1. [Perform an AEM Health and Configuration Check]

### <a name="Register-an-AEM-Event-Consumer-App">Register an AEM Event Consumer App</a>

You will need to register an AEM event consumer App to see responses to AEM changes. To do this, you can set up a webhook is able to accept and reply to a challenge http request parameter sent by Adobe CSM.

## <a name="Set-Up-Products">Set Up Products</a>

To set up AEM for Adobe I/O Events:

1. [Install the AEM Event Proxy Package](#Install-the-AEM-Event-Proxy-Package)

1. [Configure OAuth and IMS Authentication](#Configure-OAuth-and-IMS-Authentication)


### <a name="Install-the-AEM-Event-Proxy-Package">Install the AEM Event Proxy Package</a>

To install the AEM Event Proxy Package:

1. Download the latest version of the package from the [pre-release site](https://artifactory.corp.adobe.com/artifactory/maven-cloud-action-local/com/day/cq/dam/aem-event-proxy/).

2. Open AEM Package Manager at http://localhost:4502/crx/packmgr/index.jsp; or click the **Tools** icon and then click **Deployment** and **Packages**.

![package manager navi](https://user-images.githubusercontent.com/29133525/31083270-7f0f6ece-a74e-11e7-87d8-da73464104d7.png)

3. In **Package Manager**, click **Upload Package**. Click the **Browse** button and navigate to the package zip file. Click **OK**.


Note: If you have an older version of the package, delete it to avoid potential conflicts. You can delete it from the following location: **crx/de/index.jsp#/apps/eventproxy/install**.

4. Click **Install**.

![package manager ui](https://user-images.githubusercontent.com/29133525/31083485-0b10be82-a74f-11e7-85e4-4dae028946b9.png)

5. On the **Install Package** dialog box, select **Merge** from the **Access Control Handling** drop-down list and click **Install**.

![install package](https://user-images.githubusercontent.com/29133525/31085363-242d22e8-a754-11e7-9927-a8c7c43d8184.png)

6. Watch the **Activity Log**. If installed, the log reports that the package is imported.

![activity log](https://user-images.githubusercontent.com/29133525/31085648-ff7f2c92-a754-11e7-832a-350e116d6f54.png)

For more information on installing packages in AEM, see [How to Work with Packages](https://docs.adobe.com/content/docs/en/cq/5-6-1/administering/package_manager.html#Access20Package%20Share).

#### Additional Package Installation Notes

1. If you are upgrading the package, delete the previous .jar file from the following location: **/apps/eventproxy/install**

2. Verify that the Access Control Handling is properly applied by checking permissions for the eventproxy-service user group at **/useradmin**. If applied correctly, the eventproxy-service user is added to the following:

*   **/home/users/system/eventproxy/eventproxy-service with jrc:read and rep:write authorizations**
*   **/etc/cloudservices/eventproxy with jrc:read and rep:write authorizations**
*   **/content with jrc:read authorization**

For more information, see AEM [User and Group Administration](https://docs.adobe.com/docs/en/cq/5-6-1/touch-ui/granite-user-group-admin.html).

3. You can also manually update permissions in CRXDE Lite at the following location: **/crx/de/index.jsp#/etc/cloudservices/eventproxy**.

![crxdelite](https://user-images.githubusercontent.com/29133525/31087331-dbd0ac58-a759-11e7-9a0d-199d088763f3.png)

### <a name="Configure-OAuth-and-IMS-Authentication">Configure OAuth and IMS Authentication</a>

To configure OAuth and IMS authentication:

1. [Create a Certificate and Keystore](#Create-a-certificate-and-keystore)
2. [Add the Keystore to the AEM eventproxy-service User Group](#Add-the-keystore-to-the-AEM-eventproxy-service-user-group)
3. [Configure the AEM Link Externalizer](#Configure-the-AEM-Link-Externalizer)

#### <a name="Create-a-Certificate-and-Keystore">Create a Certificate and Keystore</a>

To create a certificate and keystore:

1. Create an RSA private/public certificate in OpenSSL with the following command:

```
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate_pub.crt
```

2. Add the private key and signed crtificate to a PKCS#12 file with the following command:

```
openssl pkcs12 -keypbe PBE-SHA1-3DES -certpbe PBE-SHA1-3DES -export -in certificate_pub.crt -inkey private.key -out author.pfx -name "author"
```
3. When prompted, create an export password and store it for later use.

4. Create a keystore from the generated keys with the following command:

```
cat private.key certificate_pub.crt > private-key-crt
```

Note: On Windows systems, you may need to concatenate the files manually or provide an alternate command. For more information, see the [OpenSSL manpages](https://www.openssl.org/docs/manpages.html).

5. Set the alias as **eventproxy** and a non-empty keystore password (such as admin), with the following command:

```
openssl pkcs12 -export -in private-key-crt -out keystore.p12 -name eventproxy -noiter -nomaciter
```
Note: On Windows systems, this command expression may vary. For more information, see the [OpenSSL manpages](https://www.openssl.org/docs/manpages.html).


#### <a name="Add-the-Keystore-to-the-AEM-eventproxy-service-user-group">Add the Keystore to the AEM eventproxy-service User Group</a> 

To add the key store to the AEM Eventproxy-service user group:

1. In AEM, open the **User Management** group list at http://localhost:4502/libs/granite/security/content/useradmin.html; or click the **Tools** icon and then click **Security** and **Users**.

![user management navigation](https://user-images.githubusercontent.com/29133525/31091124-2264f1d0-a767-11e7-8c57-e2ff5a5723b6.png)

2. On the **User Management** group list, click **eventproxy-service** to open it.

![eventproxy service](https://user-images.githubusercontent.com/29133525/31091250-91d7788a-a767-11e7-9187-c55d8b04c4f4.png)
 
3. On **Account settings**, click **Create KeyStore** and create the key store.

4. Click **Manage KeyStore** and then click to expand the section for **Add Private Key for Key Store file.** 

5. Add the keystore.p12 file by setting the key pair alias to **eventproxy** or the alias specified previously. 

6. Provide the keystore password (the same one provided when generating the key store).

7. Provide the private key password and then provide the private key alias **eventproxy**.

8. Click **Submit**.
 
 ![keystore management](https://user-images.githubusercontent.com/29133525/31192061-576fb936-a8fd-11e7-82b7-cfe36c47f320.png)
 
### <a name="Configure-AEM-Link-Externalizer">Configure the AEM Link Externalizer</a>

The AEM Link Externalizer name can be **author** or any other alias specified in the [Adobe Experience Manager Web Console](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl).

Note: Do not use only the word “localhost” as the default name because others may use it. This will then cause confusion and make it difficult to determine which instance is yours. 

The base url that you specify appears on the AEM Web Console. 

![localhost name](https://user-images.githubusercontent.com/29133525/31195096-62db2bc6-a906-11e7-8271-94908f33ff65.png)
 
 
## <a name="Use-Adobe-I/O">Use Adobe I/O</a>

Use Adobe I/O to do the following:

1. [Configure Adobe I/O Events Cloud Service]
1. [Perform Advanced Configuration for Adobe I/O Events]

## Configure Adobe I/O Events Cloud Service

To configure Adobe I/O Events Cloud Service:

2. Create an Adobe I/O Console Integration.
3. Configure Adobe I/O Events as a Cloud Service in AEM.
4. Configure Advanced Adobe I/O Events.



To create an Adobe I/O Console Integration:

1. On the [Adobe I/O Console](https://adobe.io/console), click **New Integration**.

2. Select **Access an API** and then click **Continue**.

3. On the **Create a new integration** page, select **Adobe I/O Events** and then click **Continue**.

![create new integration](https://user-images.githubusercontent.com/29133525/31202727-035a3578-a921-11e7-8f69-78881c95de46.png)

4. Click **New integration**.

5. On the **Create new integration** dialog box, specify a name for the integration and add a description.

6. For **Public keys**, click **Select a File** and navigate to the **certificate_pub.crt** to upload it.

<img width="1318" alt="new integration dialog box" src="https://user-images.githubusercontent.com/29133525/31202886-c98730de-a921-11e7-8e86-44f9fa6759b9.png">

## Configuring Adobe I/O Events as a Cloud Service in AEM

To configure Adobe I/O events as a cloud service in AEM:

1. Open the [Cloud Services console](http://localhost:4502/libs/cq/core/content/tools/cloudservices.html), or click the **Tools** icon, then click **Deployment** and **Cloud Services**. 

![cloud services ui](https://user-images.githubusercontent.com/29133525/31203370-0c47c47c-a924-11e7-9951-e664f795c196.png)

2. For AEM, click **Configure Now** to create a new configuration.

![cloud service configure now](https://user-images.githubusercontent.com/29133525/31203596-0f7713ea-a925-11e7-8916-6dc492181f60.png)

3. Configure the service as follows:

*   For **Day CQ Link externalizer name**: specify **author** (or any other alias previously onfigured in the AEM Link Externalizer).
*   For **API key**: Provide the key shown on the **Integration Details** page of the Adobe I/O Console. 
*   For **Technical Account ID**: Provide the ID shown on the Adobe I/O Console.
*   For **Organization ID**: Provide the ID shown on the Adobe I/O Console.
*   For **Client Secret**: Click the **Retrieve client secret** button on the **Integration Details** page of the Adobe I/O Console and provide the secret as shown on the console.

![api key](https://user-images.githubusercontent.com/29133525/31204451-17fa305c-a929-11e7-86c2-2a1610a0b264.png)

**API Key and Client Secret on Adobe I/O Console**


### Perform Advanced Configuration for Adobe I/O Events

For all the Adobe I/O event types known to the Adobe I/O event model, you can change:
*   the OSGI event **topic** 
*   the **filter** used in the OSGI event handler
*   the default path for asset events:  **/content/dam**
*   the default path for page events:  **/content**

The OSGI event handler intercepts the events according to these values and then maps the OSGI events to the Adobe I/O event model before forwarding them to Adobe I/O.

You can also perform advanced configuration of events through the OSGI configuration panel.

To use the panel:

1. Click the **Tools** icon in AEM and then click **Operations** and **Web Console**.

![web console navigation ui](https://user-images.githubusercontent.com/29133525/31209778-ac40df88-a94a-11e7-959f-1237b424cdd5.png)

2. In the **OSGI** menu, select **Configuration**.

![osgi configuration](https://user-images.githubusercontent.com/29133525/31209724-46af87e6-a94a-11e7-8ca0-65eaecf5114c.png)

Or, simply search for: **Adobe I/O Events CSM Registration**.

3. For **Adobe I/O Events CSM Registration**, click the **Edit** button.

![edit csm registration](https://user-images.githubusercontent.com/29133525/31209952-da49b480-a94b-11e7-8c9e-70b12ffb7762.png)

The default paths for asset and page events are listed near the top of the configuration page.

![osgi config default path](https://user-images.githubusercontent.com/29133525/31209678-ea371a1a-a949-11e7-8b61-ad409b8119a8.png)


 
## Perform AEM Health and Configuration Check

You can use the [AEM Web Console Sling Health Check](http://localhost:4502/system/console/healthcheck?tags=eventproxy%2C+conf&debug=true&forceInstantExecution=true&overrideGlobalTimeout=40000) to verify your configurations are correct.

To verify your configurations:

1. Check that all your configurations properly load by executing the Health Check tagged with [**eventproxy, conf**](http://localhost:4502/system/console/healthcheck?tags=eventproxy%2C+conf&debug=true&forceInstantExecution=true&overrideGlobalTimeout=40000).

![health check conf](https://user-images.githubusercontent.com/29133525/31209103-04b8a6be-a946-11e7-8c4f-e144a39bee3e.png)

2. Check that the AEM instance is is able to exchange JWT tokens with Adobe I/O Identity Management System (IMS). To do this, execute the Health Check tagged with [**eventproxy,ims**](http://localhost:4502/system/console/healthcheck?tags=eventproxy%2C+ims&debug=true&forceInstantExecution=true&overrideGlobalTimeout=40000).
This verifies that your IMS related configurations are correct and working, including the eventproxy-service user KeyStore configuration, the Adobe I/O console-originated API key, the Technical Account ID, the Organization ID and the client secret.

![health check ims](https://user-images.githubusercontent.com/29133525/31208648-bc00f73e-a943-11e7-9c12-04cbd85797d4.png)


3. Check that the event metadata and the provider associated with the AEM instance are registered in Adobe I/O Channel & Subscription Management (CSM) by executing the Health Check tagged with [**eventproxy,csm**](http://localhost:4502/system/console/healthchecktags=eventproxy%2C+csm&debug=true&forceInstantExecution=true&overrideGlobalTimeout=40000).
This verifies that the AEM instance is successfully registered as an event provider with Adobe IO CSM.

![health check csm](https://user-images.githubusercontent.com/29133525/31209225-cd500554-a946-11e7-9538-29e43c611bc5.png)

 
## Register an AEM Event Consumer App

To register an AEM event consumer App, you can set up a webhook. Your webhook should be able to accept and reply to a challenge http request parameter sent by Adobe CSM.

### Set up Webhook: Example
To create a webhook at webscript.io, you can use the following script to configure it:

```local request  = request
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
```

## <a name="Watch-It-Work">Watch The Solution Work</a> 

You can watch the solution work by testing your integration. To do this:

1. Register your webhook with the Adobe I/O Console

2. Perform a webhook health check

### Register Webhook with the Adobe I/O Console
Once you have your webhook ready, use the adobe.io console to register it:
1. On the [Adobe I/O Console](https://adobe.io/console), click **New Integration**.
2. Select **Subscribe to events** and click **Continue**.
3. Select **AEM-your-day-cq-link-externalizer-base-server-url** and click **Continue**.
4. Select **Create new integration** and fill in the **Configure Integration** detail form.
5. Click the **Add webhook** button and complete the **Add a new Webhook** form.
6. Select the events you want to subscribe to and click **Save**.
7. Click **Create new Integration** and then click **Continue to Integration detail**.

![integration health check](https://user-images.githubusercontent.com/29133525/31210441-8455506c-a94f-11e7-8099-1a9b766af9b8.png)

Note: If you upgraded from version 0.16 of the package to 0.18, your AEM registered event provider ID has changed
as a result. You will need to re-register your webhook and use the latest provider. The label should start with **AEM-** but not with **AEM_DAM**, as it was used in version 0.16.

### Webhook Health Check

To perform a webhook health check:

1. Check that events are sent to and received by Adobe I/O event receiver (the AEM ingress adapater). To do this, execute the Health Check tagged with [**eventproxy, eventreceiver**](http://localhost:4502/system/console/healthcheck?tags=eventproxy%2C+evre&debug=true&forceInstantExecution=true&overrideGlobalTimeout=40000)

![check webhook evre](https://user-images.githubusercontent.com/29133525/31210557-496f319c-a950-11e7-9744-e28aba9a74d1.png)


2. Test the Webhook Subcription by doing the following:
*   by publishing or unpublishing AEM pages
*   by editing, adding, or removing an asset in the AEM DAM
*   or by using the [AEM Assets HTTP API](https://docs.adobe.com/docs/en/aem/6-2/develop/extending/mac-api-assets.html)

The responses appear in your webhook.

<img width="1226" alt="webscript webhook" src="https://user-images.githubusercontent.com/29133525/31210683-35593846-a951-11e7-8681-2557a0bdcdbd.png">

<img width="908" alt="webhook report log" src="https://user-images.githubusercontent.com/29133525/31210699-57b6f6bc-a951-11e7-8199-f1731125ee35.png">

_Note: Please help us make this documentation as useful as possible. If you find an error or have a suggestion to improve it, please click the **Issues** tab on this GiHhub repository and then click the **New issue** button. Provide a title and description for your comment and then click the **Submit new issue** button._

![submit new issue](https://user-images.githubusercontent.com/29133525/32515298-f344bd5a-c3bc-11e7-9978-34516f964f9f.png)
 
 
 
