# Integrate Analytics Triggers with Adobe I/O Events

These instructions describe how to use Adobe Analytics triggers to notify you of Adobe I/O events, including the behavior of your site's users. Follow the instructions below to try the solution yourself.

1. [Introduction](#Introduction)

1. [Set Up Products](#Set-Up-Products)

1. [Use Adobe I/O](#Use-Adobe-I/O)

1. [Watch the Solution Work](#Watch-It-Work)

## <a name="Introduction">Introduction</a>

### What are Triggers?
Triggers is a Marketing Cloud Activation core service that enables marketers to identify, define, and monitor key consumer behaviors, and then generate cross-solution communication to re-engage visitors.
For more information on triggers, see the [Triggers Help Page](https://marketing.adobe.com/resources/help/en_US/mcloud/triggers.html).


Before setting up and using Adobe I/O, you will need to do the following:

1. [Obtain Product Authorization](#Obtain-Product-Authorization)

2. [Obtain Administrative Permissions](#Obtain-Administrative-Permissions)



### <a name="Obtain-Product-Authorization">Obtain Product Authorization</a>

To complete this solution, you will need authorization to use the following services:
*   Adobe Analytics, including Triggers
*	Adobe Marketing Cloud Activation Core Services
*   Adobe Experience Manager (AEM), or an external website that connects to Analytics
*   An [Adobe ID](https://helpx.adobe.com/x-productkb/global/adobe-id-account-change.html), if you do not have one already


### <a name="Obtain-Administrative-Permissions">Obtain Administrative Permissions</a>

You will also need administrative permissions for the following:
* Adobe Analytics
* Your enterprise organization
* AEM, if using that service to connect to Analytics

If you do not have administrative permissions, please contact ioevents-beta@adobe.com. After requesting administrative permissions, watch for an email from Adobe Systems Incorporated, as shown:

   ![admin rights 2](https://user-images.githubusercontent.com/29133525/32473847-d5ba50ba-c326-11e7-8443-fef38b12bcff.png)

## <a name="Set-Up-Products">Set Up Products</a>

To set up Adobe products for this solution:

1. [Set Up AEM](#Integrate-AEM)

1. [Set Up Analytics](#Set-up-Triggers)

### <a name="Integrate-AEM">Set Up AEM</a>

To set up AEM with Analytics Triggers and Dynamic Tag Management (DTM), follow the [step-by-step documentation](https://github.com/johnwight/Adobe-AEM-with-DTM-and-Analytics) for that configuration. You can also follow the video shown below:


   [![Integrate AEM](https://img.youtube.com/vi/vtcZCS-LFeg/0.jpg)](https://youtu.be/vtcZCS-LFeg)


### <a name="Set-up-Triggers">Set up Analytics</a>

To set up Analytics:

1. [Get Product Access from Admin Console](#Admin-Console)

2. [Configure Reporting for Triggers](#Reporting-for-Triggers)

3. [Configure DTM](#Configure-DTM)

4. [Specify a New Trigger](#Specify-a-New-Trigger)



#### <a name="Admin-Console">Get Product Access through Adobe Admin Console</a>	 

To get access through the Adobe Admin Console:

1.	Sign into the console by clicking the **Sign in** button on the administrator rights email you received from Adobe and then providing your credentials.

2.	On the main screen of the Admin Console, click **Products**.

3.	On the Products page of the console, verify that your requested products have been added to the site and then click the Adobe Analytics icon.

      ![admin products page](https://user-images.githubusercontent.com/29133525/32474273-0b6c2272-c329-11e7-81be-aeb232549191.png)

4.	Click the **Configuration Details** tab and do the following:

    1.	Verify the **Name** of your configuration.

    2.	Provide a **Description**, if applicable.

    3.	Verify the display name that will show on the **Products** page.

    4.	Under **Enabled Services**, select the option for **Triggers**.

     ![admin console configure product](https://user-images.githubusercontent.com/29133525/32474470-df2f94c2-c329-11e7-8db1-7b06a211fca8.png)

  5. To give permissions to users who want access to Adobe services in the cloud:

     1. Click **User management** and then click **Users**.
     2. Click on user's name.
     
      ![user name](https://user-images.githubusercontent.com/29133525/32474586-7a7be066-c32a-11e7-80f1-5419d548d194.png)

     3. For the user's **Access and rights**, provide **Product Access** and **Admin Rights** from the drop-down for the available products and services.

      ![user permissions](https://user-images.githubusercontent.com/29133525/32474735-409e7d8a-c32b-11e7-879c-5f2bdf0825b2.png)



#### <a name="Reporting-for-Triggers">Configure Reporting for Triggers</a>

To configure reporting for Triggers:

1. On the Marketing Cloud home page, click the **App** button on top right corner.

    ![new apps icon](https://user-images.githubusercontent.com/29133525/30260555-554e9dbe-9685-11e7-9ecd-86b239f007f9.png)

2. Click on Analytics Launcher.

    ![analytics launcher](https://user-images.githubusercontent.com/29133525/30260673-01c26c88-9686-11e7-8b31-5203c6386591.png)

3. On the **Admin** tab of the Analytics home screen, click the **Report Suites** option. You must have administrative privileges for the **Admin** tab to appear on your screen.

    ![admin tab for report suites](https://user-images.githubusercontent.com/29133525/32473097-075ec802-c323-11e7-9f4e-371acf9f4914.png)


4. On the Reports Suites Manager page, click **Create New** and select **Report Suite**. Configure the new report suite so that it is accessible in Adobe Analytics.

    ![report suite manager](https://user-images.githubusercontent.com/29133525/32474884-36e55ee8-c32c-11e7-8a50-928b73efb2f8.png)


#### <a name="Configure-DTM">Configure DTM</a>

To configure DTM:


1.	On the Marketing Cloud home page, click the **Apps** icon and then click **Activation**.

    ![dtm 1 icon](https://user-images.githubusercontent.com/29133525/30288083-b9ca747a-96e4-11e7-9c23-5f97daf4233c.png)

2.	On the Activation page, click **Dynamic Tag Management**.

    ![dtm card](https://user-images.githubusercontent.com/29133525/32474786-9157a9fe-c32b-11e7-9126-3423d132d243.png)

3.	On the **Overview** tab, click the **Settings** icon.

    ![settings icon](https://user-images.githubusercontent.com/29133525/32475040-fdcf1454-c32c-11e7-8e13-c5b1073b5325.png)

4. On the Settings page, set the variable as follows:

    1. Expand the **Global Variables** section.
    2. Click the **Evar name** drop-down and select **eVar3**.
    3. Select the **Set as** option.
    4. In the **Set as** field, type the following URL element:
        ```
        %window.location.host%%window.location.pathname%
        ```
    5. Click **Save eVar**.

        ![set evar](https://user-images.githubusercontent.com/29133525/32475180-b6967356-c32d-11e7-8485-e761c5a4d323.png)

5. On the **Approvals** tab, click the **Approve** button.    

    ![approve button](https://user-images.githubusercontent.com/29133525/32475284-36a8221a-c32e-11e7-8cba-62823e6e7038.png)

6. On the **Overview** tab, click the **Publish Queue** button.

    ![publish button](https://user-images.githubusercontent.com/29133525/32475340-7ee8d178-c32e-11e7-9ba3-cab950bf2776.png)


#### <a name="Specify-a-New-Trigger">Specify a New Trigger</a>

You can specify triggers for many events on your site. For example, in this case, we will set notifications to be sent when  carts are abandoned. We will set a trigger for sessions when the user visits either a **cart.html**, **checkout.html** or **order.html** page, but never reaches the **thank-you.html** page within a ten minute session. The trigger indicates that the user added products to the cart, and was about to make a purchase, but later decided otherwise, or forgot to complete the purchase.

To specify a new trigger:

1. On the Marketing Cloud home page, click the **Apps** icon and then click **Activation**.

    ![dtm 1 icon](https://user-images.githubusercontent.com/29133525/30290835-1765ebf6-96ee-11e7-880c-46177a716ac5.png)

2. On the Triggers card, click the **Launch** button.
    
    ![resize 2 dtm card](https://user-images.githubusercontent.com/29133525/32473778-715a6588-c326-11e7-9114-f4a2b7da2fbf.png)

3. On the **Triggers** page, click the **New Triggers** button and select **Abandonment**.

    ![abandonment trigger](https://user-images.githubusercontent.com/29133525/32475431-ee36a582-c32e-11e7-9803-f0216855b290.png)

4. On the **New Trigger** box, specify a **Name** and provide a **Description** for your trigger. Select the **Report Suite** that you previously setup from the drop-down field.

    ![trigger dialog](https://user-images.githubusercontent.com/29133525/32475528-7287302c-c32f-11e7-909f-b2c86313bf08.png)


6. On the **Triggers Settings** page, define the business rules for your trigger. You can drag a dimension/metric box from the left panel to the right side of the screen and then specify the business rules for what must happen and what must not happen in a session. In this case, we set the trigger to fire after 10 minutes of inactivity after the rules are met.

    ![triggers settings page](https://user-images.githubusercontent.com/29133525/30291990-c37f6ff4-96f1-11e7-84ff-51559bd886c2.png)


7. Click the **Save** button.


Once you save the trigger, any event in your report suite that meets the defined business rules criteria will cause a trigger to fire. You can view the status of triggers on the **Triggers** page.

![triggers listing](https://user-images.githubusercontent.com/29133525/30292257-af31b0ce-96f2-11e7-8e3c-32531e0d0d9e.png)

## <a name="Use-Adobe-I/O">Use Adobe I/O</a>

Use Adobe I/O by creating a new integration with the Console. To do this:

1. After signing in to the [Adobe I/O Console](https://adobe.io/console), click **New Integration**.

    ![new integration button](https://user-images.githubusercontent.com/29133525/30292388-2ccdd986-96f3-11e7-93bd-93f74bb4e3a4.png)

2. Select **Receive real-time events** and click **Continue**.

    ![real time events](https://user-images.githubusercontent.com/29133525/30292486-946dc812-96f3-11e7-9bc5-2aa0196f704b.png)


3. Select **Analytics Triggers** as an event provider and click **Continue**.

    ![create io trigger](https://user-images.githubusercontent.com/29133525/30292595-064305ec-96f4-11e7-9e60-3ee811c7949a.png)

4. Click **Continue** to move on to the next page without making any changes.

5. Provide the **Name** and **Description** for your integration.

    ![name integration box](https://user-images.githubusercontent.com/29133525/30292714-6d00a8ac-96f4-11e7-8449-f93bb2fccfcb.png)

6. Generate a public certificate. To do this:

    1. Open a terminal and execute the following command:

        ```
        openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate_pub.crt
        ```
    2. Upload the public certificate by clicking the **Select a File** link and then by selecting the certificate from your computer:

        ![public certificate](https://user-images.githubusercontent.com/29133525/32476036-2e563e40-c332-11e7-9fbd-889f1ba8054f.png)


7. Add Webhook details and Click **Save**. For information on creating and registering webhooks, see [Introduction to Webhooks](https://github.com/adobeio/adobeio-events-documentation/blob/master/Webhook_docs_intro.md).

    ![webhook details](https://user-images.githubusercontent.com/29133525/32476125-b55c816a-c332-11e7-8edd-a5ca7d1d5611.png)


## <a name="Watch-It-Work">Watch the Solution Work</a>

Your enterprise may have its own tool that you can use to subscribe and listen to webhook events. Alternatively, you can use the following procedure to set up notifications with Slack.
To watch your trigger work on Slack:

1. Clone the repository and follow the setup described on https://github.com/hirenshah111/webhook_server.

2. Modify the Slack details in webhook_server/public/javascripts/app.js according to how you want to see the notifications.

3. Run the application and create a **triggers2** listener, then click **Connect**.

    ![listen to webhook](https://user-images.githubusercontent.com/29133525/30293183-e0657718-96f5-11e7-8696-86153cc4d5ff.png)


Trigger messages are received as `POST` requests on this thread.

## Authors
- Hiren Shah [@hirenshah111](https://github.com/hirenshah111).
- John Wight [@johnwight](https://github.com/johnwight).

## Feedback?

Please help make this solution as useful as possible. If you find a problem in the documentation or have a suggestion, click the **Issues** tab on this GiHhub repository and then click the **New issue** button. Provide a title and description for your comment and then click the **Submit new issue** button.

![submit new issue](https://user-images.githubusercontent.com/29133525/32515298-f344bd5a-c3bc-11e7-9978-34516f964f9f.png)
