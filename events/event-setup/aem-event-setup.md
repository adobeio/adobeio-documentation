# Set up AEM Events with Adobe I/O Events

These instructions describe how to set up Adobe Experience Manager (AEM) for Adobe I/O Events. You can use Adobe I/O for notification of AEM events, such as page or asset changes.

1. [Introduction](#Introduction)

1. [Set Up Products](#Set-Up-Products)

1. [Use Adobe I/O](#Use-Adobe-I/O)

1. [Watch the Solution Work](#Watch-It-Work)

## <a name="Introduction">Introduction</a>

Before setting up and using AEM with Adobe I/O, you will need to do the following:

1. [Obtain Authorization](#Obtain-Authorization)

1. [Register an AEM Event Consumer App](#Register-an-AEM-Event-Consumer-App)

### <a name="Obtain-Authorization">Obtain Authorization</a>

To complete this solution, you will need authorization to use the following services:

*   AEM instance with version 6.2 or 6.3, with administrative permissions
*   [Adobe I/O Console](https://adobe.io/console) access, with administrative permissions for your enterprise organization. If you do not have administrative permissions, please contact ioevents-beta@adobe.com. After requesting administrative permissions, watch for an email from Adobe Systems Incorporated, as shown:

      ![admin rights 2](https://user-images.githubusercontent.com/29133525/32855060-ddbbe8cc-c9fd-11e7-88b9-1aa1fb1ab380.png)
 

### <a name="Register-an-AEM-Event-Consumer-App">Register an AEM Event Consumer App</a>

You will need to register an AEM event consumer app, such as a webhook, to see responses to AEM changes. These instructions include steps for setting up a webhook that is able to accept and reply to a [challenge http request](../Webhook_docs_intro.md#orgec22b7a) parameter sent by Adobe I/O Channel & Subscription Management (CSM). For more information on understanding and working with webhooks, see the [Introduction to Adobe I/O Events Webhooks](../Webhook_docs_intro.md).

## <a name="Set-Up-Products">Set Up Products</a>

To set up AEM for Adobe I/O Events:

1. [Install the AEM Event Proxy Package](#Install-the-AEM-Event-Proxy-Package)

1. [Configure OAuth and IMS Authentication](#Configure-OAuth-and-IMS-Authentication)


### <a name="Install-the-AEM-Event-Proxy-Package">Install the AEM Event Proxy Package</a>

To install the AEM Event Proxy Package:

1. Download the latest version of the package [here](https://github.com/adobeio/adobeio-events-documentation/files/1564085/aem-event-proxy-0.32.221.zip).

2. Open AEM Package Manager by clicking the **Tools** icon and then clicking **Deployment** and **Packages**.

   ![package manager navi](https://user-images.githubusercontent.com/29133525/32855537-36c7cd72-c9ff-11e7-9098-a5aba8f4493f.png)

3. In **Package Manager**, click **Upload Package**. Click the **Browse** button and navigate to the package zip file. Click **OK**.


Note: If you have an older version of the package, delete it to avoid potential conflicts. You can delete it from the following location: **crx/de/index.jsp#/apps/eventproxy/install**.

4. Click **Install**.

      ![package manager ui](https://user-images.githubusercontent.com/29133525/32855716-cad2c24c-c9ff-11e7-8220-3366caaa528d.png)

5. On the **Install Package** dialog box, select **Merge** from the **Access Control Handling** drop-down list and click **Install**.

      ![install package](https://user-images.githubusercontent.com/29133525/32855829-2b1392d0-ca00-11e7-855f-e4f8ae7d7fc1.png)

6. Watch the **Activity Log**. If installed, the log reports that the package is imported.

      ![activity log](https://user-images.githubusercontent.com/29133525/31085648-ff7f2c92-a754-11e7-832a-350e116d6f54.png)

For more information on installing packages in AEM, see [How to Work with Packages](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/package-manager.html).

#### Additional Package Installation Notes

1. If you are upgrading the package, delete the previous .jar file from the following location: **`/apps/eventproxy/install`**

2. Verify that the Access Control Handling is properly applied by checking permissions for the eventproxy-service user group at **/useradmin**. If applied correctly, the eventproxy-service user is added to the following:

*   **`/home/users/system/eventproxy/eventproxy-service` with `jcr:read` and `rep:write` authorizations**
*   **`/etc/cloudservices/eventproxy` with `jcr:read` and `rep:write` authorizations**
*   **`/content` with `jcr:read` authorization**

For more information, see AEM [User, Group and Access Rights Administration](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/user-group-ac-admin.html).

3. You can also manually update permissions in CRXDE Lite at the following location: **`/crx/de/index.jsp#/etc/cloudservices/eventproxy`**.

      ![crxdelite](https://user-images.githubusercontent.com/29133525/32855967-9de04aa6-ca00-11e7-8db2-1fcf608fc249.png)

### <a name="Configure-OAuth-and-IMS-Authentication">Configure OAuth and IMS Authentication</a>

To configure OAuth and IMS authentication:

1. [Create a Certificate and Keystore](#Create-a-Certificate-and-Keystore)
2. [Add the Keystore to the AEM eventproxy-service User Group](#Add-the-keystore-to-the-AEM-eventproxyservice-User-Group)
3. [Configure the AEM Link Externalizer](#Configure-the-AEM-Link-Externalizer)

#### <a name="Create-a-Certificate-and-Keystore">Create a Certificate and Keystore</a>

To create a certificate and keystore:

1. Create an RSA private/public certificate in OpenSSL with the following command:

```
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate_pub.crt
```

2. Add the private key and signed certificate to a PKCS#12 file with the following command:

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


#### <a name="Add-the-Keystore-to-the-AEM-eventproxyservice-User-Group">Add the Keystore to the AEM eventproxy-service User Group</a> 

To add the key store to the AEM Eventproxy-service user group:

1. In AEM, open the **User Management** group by clicking the **Tools** icon and then clicking **Security** and **Users**.

      ![user management navigation](https://user-images.githubusercontent.com/29133525/32856124-216ac180-ca01-11e7-9a57-ec815ab80fd8.png)

2. On the **User Management** group list, click **eventproxy-service** to open it.

      ![eventproxy service](https://user-images.githubusercontent.com/29133525/32856308-a90add0a-ca01-11e7-8112-1491ce585e0e.png)
 
3. On **Account settings**, click **Create KeyStore** and create the key store.

4. Click **Manage KeyStore** and then click to expand the section for **Add Private Key for Key Store file.** 

5. Add the keystore.p12 file by setting the key pair alias to **eventproxy** or the alias specified previously. 

6. Provide the keystore password (the same one provided when generating the key store).

7. Provide the private key password and then provide the private key alias **eventproxy**.

8. Click **Submit**.
 
      ![keystore management](https://user-images.githubusercontent.com/29133525/32856408-f5a370be-ca01-11e7-9e78-7ee03317d1ce.png)
 
### <a name="Configure-the-AEM-Link-Externalizer">Configure the AEM Link Externalizer</a>

To configure AEM Link Externalizer:

1. Open the Web Console, or click the **Tools** icon, then click **Operations** and **Web Console**. 

    The AEM Link Externalizer name can be **author** or any other alias specified in the Adobe Experience Manager Web Console.

    ![aem-console](https://user-images.githubusercontent.com/7494850/34497015-8ff19640-efb0-11e7-8145-b257dfc8fa4b.png)

2. Scroll down the list to find **Day CQ Link Externalizer**, update the domain name, and click **Save** when done.
 
    Note: The base url that you specify appears on the AEM Web Console. Do not use only the word “localhost” as the default name because others may use it. This will then cause confusion and make it difficult to determine which instance is yours. 

    ![aem web console base url](https://user-images.githubusercontent.com/29133525/32803801-f3dcce30-c941-11e7-8bf4-405c43d03f74.png)

## <a name="Use-Adobe-I/O">Use Adobe I/O</a>

Use Adobe I/O to do the following:

1. [Create an Adobe I/O Console Integration](#Create-an-Adobe-I/O-Console-Integration)

1. [Configure Adobe I/O Events as a Cloud Service in AEM](#Configure-Adobe-I/O-Events-as-a-Cloud-Service-in-AEM)

1. [Configure Advanced Adobe I/O Events](#Configure-Advanced-Adobe-I/O-Events)

### <a name="Create-an-Adobe-I/O-Console-Integration">Create an Adobe I/O Console Integration</a>

To create an [Adobe I/O Console](https://adobe.io/console) Integration:

1. After signing in to the Adobe I/O Console, click **New Integration**.

2. Select **Access an API** and then click **Continue**.

      ![access an api](https://user-images.githubusercontent.com/29133525/32519316-9580406a-c3c9-11e7-98bf-aabba0187250.png)


3. On the **Create a new integration** page, select **Adobe I/O Events** and then click **Continue**.

      ![io events integration](https://user-images.githubusercontent.com/29133525/32520095-0967bb3c-c3cc-11e7-9c9e-8a730c07b3c9.png)

4. Click **New integration**.

      ![create new integration option](https://user-images.githubusercontent.com/29133525/32520604-d00d9eea-c3cd-11e7-88d7-a35175ae5b91.png)

5. On the <a name="Create-new-integration-box">**Create a new integration** dialog box</a>, specify a name for the integration and add a description. To add **Public keys certificates**, click **Select a File** and navigate to your **certificate_pub.crt** to upload it.

      ![updated create new integration](https://user-images.githubusercontent.com/29133525/32857108-1221197e-ca04-11e7-9a86-42293b6b729a.png)

6. Click **Create Integration.**

### <a name="Configure-Adobe-I/O-Events-as-a-Cloud-Service-in-AEM">Configure Adobe I/O Events as a Cloud Service in AEM</a>

To configure Adobe I/O Events as a cloud service in AEM:

1. Open the Cloud Services console, or click the **Tools** icon, then click **Deployment** and **Cloud Services**. 

      ![cloud services ui](https://user-images.githubusercontent.com/29133525/32857472-2e76d8c4-ca05-11e7-8edf-0b878d1b7d81.png)

2. Under **Adobe Marketing Cloud** on the **Cloud Services** page, click **Show Configurations** for **Adobe I/O Events**. 

      ![show configurations of adobe events](https://user-images.githubusercontent.com/29133525/32805318-499a990c-c946-11e7-938a-2ea8e803175b.png)

3. Configure the service by specifying each field in the **Edit Component** dialog box. You can copy your credentials from the [Adobe I/O Console](https://adobe.io/console) and paste them into each required field.

*   For **AEM Link externalizer**: specify **author** (or any other alias previously configured in the AEM Link Externalizer).
*   For **API key**: Provide the key shown on the **Integration Details** page of the Adobe I/O Console. 
*   For **Technical Account ID**: Provide the ID shown on the Adobe I/O Console.
*   For **Organization ID**: Provide the ID shown on the Adobe I/O Console.
*   For **Client Secret**: Click the **Retrieve client secret** button on the **Integration Details** page of the Adobe I/O Console and provide the secret as shown on the console.

    ![copy secrets](https://user-images.githubusercontent.com/29133525/32806941-7fde170a-c94b-11e7-9bf4-a237c1afb2c1.png) 
![edit configuration adobe io](https://user-images.githubusercontent.com/29133525/32806239-13fe90c0-c949-11e7-932d-4ba3d9d643f4.png)

## Perform an AEM Health and Configuration Check

You can use the AEM Web Console Sling Health Check to verify that your configurations are correct.

To verify your configurations:

1. Check that all your configurations properly load by executing the Health Check tagged with **eventproxy, conf**.

      ![health check conf](https://user-images.githubusercontent.com/29133525/32858983-6f86c61c-ca0a-11e7-8203-b5edf34d05af.png)

2. Check that the AEM instance is able to exchange JWT tokens with Adobe I/O Identity Management System (IMS). To do this, execute the Health Check tagged with **eventproxy,ims**.
This verifies that your IMS related configurations are correct and working, including the eventproxy-service user Keystore configuration, the Adobe I/O console-originated API key, the Technical Account ID, the Organization ID and the client secret.

      ![health check ims](https://user-images.githubusercontent.com/29133525/32858687-8e94e7e2-ca09-11e7-945b-3ce845b6717a.png)

3. Check that the event metadata and the provider associated with the AEM instance are registered in Adobe I/O Channel & Subscription Management (CSM) by executing the Health Check tagged with **eventproxy,csm**.
This verifies that the AEM instance is successfully registered as an event provider with Adobe I/O CSM.

      ![health check csm](https://user-images.githubusercontent.com/29133525/32858496-ea6ab2a0-ca08-11e7-9272-db04454611c2.png)

 
## Register an AEM Event Consumer App

To register an AEM event consumer App, you can set up a webhook. Your webhook should be able to accept and reply to a challenge http request parameter sent by Adobe I/O CSM.

### Set up Webhook: Example
To create a webhook at webtask.io, add the following code to make sure the Challenge is echoed back. This is needed for the verification by Adobe I/O CSM when we register the webhook URL later using CSM API:

```
var express = require('express');
var Webtask = require('webtask-tools');
var bodyParser = require('body-parser');
var app = express();

app.use(bodyParser.json());
app.get('/webhook', function (req, res) {
   var result = "No challenge";
   if (req.query["challenge"]){
      result = req.query["challenge"]
      console.log("got challenge: " + req.query["challenge"]);
   } else {
      console.log("no challenge")
   }
   res.status(200).send(result)
});

app.post('/webhook', function (req, res) { 
   console.log(req.body)
   res.writeHead(200, { 'Content-Type': 'application/text' });
   res.end("pong");
});

module.exports = Webtask.fromExpress(app);
```

## <a name="Watch-It-Work">Watch the Solution Work</a> 

You can watch the solution work by testing your integration. To do this:

1. [Register Your Webhook with the Adobe I/O Console](#Register-it)
2. [Perform a Webhook Health Check](#Webhook-health-check)


### <a name="Register-it">Register Webhook with the Adobe I/O Console</a> 

Once you have your webhook ready, use the [Adobe I/O Console](https://adobe.io/console) to register it:

1. On the Adobe I/O Console, click **New Integration**.

2. Select **Receive near-real time events** and click **Continue**.

      ![receive near real time events](https://user-images.githubusercontent.com/29133525/32859993-63f51472-ca0d-11e7-9ce8-c5570a324c86.png)

3. Select the AEM Link Externalizer base URL that you [previously specified](#Configure-the-AEM-Link-Externalizer) and then click **Continue**.

      ![aem externalizer base url on marketing cloud](https://user-images.githubusercontent.com/29133525/32860382-aec87b50-ca0e-11e7-8eb6-febd13497b17.png)

4. Select **Create new integration** and fill in the **Integration Details** form [similar to your previous integration](#Create-new-integration-box).

5. Click the **Add webhook** button and complete the **Add a new webhook** form.

6. Select the events you want to subscribe to and click **Save**.

      ![integration health check](https://user-images.githubusercontent.com/29133525/32861939-a93861b4-ca13-11e7-99b9-3ffe136d1272.png)

Note: Once you have registered your webhook, responses will include a [status](https://github.com/adobeio/adobeio-events-documentation/blob/master/Webhook_docs_intro.md#org85f36da) field to show if it is ```VERIFIED```.

### <a name="Webhook-health-check">Perform a Webhook Health Check</a> 

To perform a webhook health check:

1. Check that events are sent to and received by Adobe I/O Event receiver (the AEM ingress adapater). To do this, execute the Health Check tagged with **`eventproxy`, `eventreceiver`**.

    ![event receiver health check](https://user-images.githubusercontent.com/7494850/34497828-08e94c5c-efb4-11e7-868d-50a3b3d3aa65.png)

    You can also emit a custom osgi event sample by triggering the Health Check tagged with **`eventproxy`, `custom`**.
    
    ![custom event health check](https://user-images.githubusercontent.com/7494850/34497879-3648edc4-efb4-11e7-96b9-9cc27f04ef96.png)

    You should see events posted through your webhook when you perform these health checks.
    
2. Test the Webhook Subscription by doing the following:
      *   by publishing or unpublishing AEM pages
      *   by editing, adding, or removing an asset in the AEM DAM or by using the [AEM Assets HTTP API](https://helpx.adobe.com/experience-manager/6-3/assets/using/mac-api-assets.html)

      The responses appear in your webhook.
      
      ![web hook response 1](https://user-images.githubusercontent.com/29133525/32862896-d6495e26-ca16-11e7-8309-e422617654a4.png)
      
      ![web hook response 2](https://user-images.githubusercontent.com/29133525/32863030-5d9f1a14-ca17-11e7-8ceb-66e2c5d92561.png)
      

### Optional :<a name="Configure-Advanced-Adobe-I/O-Events">Adobe I/O Events' OSGI to XDM Event Mapping Configurations</a>

For all Adobe I/O event types defined by the Adobe I/O Event Model, there is an **Adobe I/O Events OSGI to XDM Event Mapping Configuration**.

For each of these you can change/edit:

* The OSGI Topic you want to observe: `osgiTopic`
* The OSGI Filter you want to apply in your osgi event observation. If left empty no osgi filtering is done: `osgiFilter`
* The JCR osgiJcrPathFilter to filter the OSGI events further. If left empty, no resource path filtering is done: `osgiJcrPathFilter`
* The OSGI Event Handler Type (use the default `com.day.cq.dam.eventproxy.service.impl.listener.AdobeIoEventHandler`  to map any custom osgi event): `osgiEventHandlerClassName`
* The Adobe I/O XDM Event Type to map to the OSGI event, again, use the default (`com.adobe.xdm.event.OsgiEmittedEvent`) to map your custom osgi events : `adobeIoXdmEventClassName`
* The Adobe I/O Event Code (unique to your Event provider, i.e. unique to your AEM instance/cluster): `adobeIoEventCode`
* The Adobe I/O Event Label as it will appear on the Adobe I/O Console: `adobeIoEventLabel`
  
The various OSGI event handler will intercept the events according to these values and then map these OSGI events to the Adobe I/O Event Model before forwarding them to Adobe I/O.

The solution leverages the OSGI configuration factory pattern, hence not only can you edit these configurations, but you can also remove and add such configurations.

To configure using the panel:

1. Click the **Tools** icon in AEM and then click **Operations** and **Web Console**.

      ![web console navigation ui](https://user-images.githubusercontent.com/29133525/32857842-82113b22-ca06-11e7-9c90-52a1882c6298.png)

2. In the **OSGI** menu, select **Configuration**.

      ![osgi configuration](https://user-images.githubusercontent.com/29133525/32857941-e41ac81a-ca06-11e7-9592-d4289e9ce35f.png) and search for: **Adobe I/O Events CSM Registration**.

3. For **Adobe I/O Events OSGI to XDM Event Mapping Configurationn**, click the **+**, **Edit**, or **Delete** buttons.

## Authors
- Sarah Xu [@sarahxxu](https://github.com/sarahxxu).
- John Wight [@johnwight](https://github.com/johnwight).

      
## Feedback?

Please help make this solution as useful as possible. If you find a problem in the documentation or have a suggestion, click the **Issues** tab on this GitHub repository and then click the **New issue** button. Provide a title and description for your comment and then click the **Submit new issue** button.

   ![submit new issue](https://user-images.githubusercontent.com/29133525/32515298-f344bd5a-c3bc-11e7-9978-34516f964f9f.png)
 
 
 
