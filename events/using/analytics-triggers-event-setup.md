<!--:navorder: 2-->

# Integrate Analytics Triggers with Adobe I/O Events

These instructions describe how to use Adobe Analytics triggers to notify you of Adobe I/O events, including the behavior of your site&rsquo;s users. Follow the instructions below to try the solution yourself.

- [Introduction](#introduction)
- [Set up products](#setupproducts)
- [Use Adobe I/O](#useadobeio)
- [Watch the solution work](#watchthesolutionwork)

**Resources**
- [Debugging](help/debug.md#analyticstriggersevents)
- [FAQ](help/faq.md#analyticstriggersevents)

## Introduction

### What are Triggers?
Triggers is a Marketing Cloud Activation core service that enables marketers to identify, define, and monitor key consumer behaviors, and then generate cross-solution communication to re-engage visitors.
For more information on triggers, see the [Triggers Help Page](https://marketing.adobe.com/resources/help/en_US/mcloud/triggers.html).


Before setting up and using Adobe I/O, you will need to do the following:

1. [Obtain product authorization](#obtainproductauthorization)
2. [Obtain administrative permissions](#obtainadministrativepermissions)

### Obtain product authorization

To complete this solution, you will need authorization to use the following services:
*   Adobe Analytics, including Triggers
*	Adobe Marketing Cloud Activation Core Services
*   Adobe Experience Manager (AEM), or an external website that connects to Analytics
*   An [Adobe ID](https://helpx.adobe.com/x-productkb/global/adobe-id-account-change.html), if you do not have one already

### Obtain administrative permissions

You will also need administrative permissions for the following:
* Adobe Analytics
* Your enterprise organization
* AEM, if using that service to connect to Analytics

If you do not have administrative permissions, please contact ioevents-beta@adobe.com. After requesting administrative permissions, watch for an email from Adobe Systems Incorporated, as shown:

   ![Admin rights email](../../img/events_atrig_01.png)

## Set up products

To set up Adobe products for this solution:

1. [Set up AEM](#setupaem)
1. [Set up Analytics Triggers](#setupanalyticstriggers)

### Set up AEM

To set up AEM with Analytics Triggers and Dynamic Tag Management (DTM), follow the [step-by-step documentation](https://github.com/johnwight/Adobe-AEM-with-DTM-and-Analytics) for that configuration. You can also follow the video shown below:


   [![Video: Integrate AEM](../../img/events_atrig_02.jpg)](https://youtu.be/vtcZCS-LFeg "Video: Integrate AEM")

### Set up Analytics Triggers

To set up Analytics Triggers:

1. [Get product access through Adobe Admin Console](#getproductaccessthroughadobeadminconsole)
2. [Configure reporting for Triggers](#configurereportingfortriggers)
3. [Configure DTM](#configuredtm)
4. [Specify a new trigger](#specifyanewtrigger)

#### Get product access through Adobe Admin Console	 

To get access through the Adobe Admin Console:

1.	Sign into the console by clicking the **Sign in** button on the administrator rights email you received from Adobe and then providing your credentials.

2.	On the main screen of the Admin Console, select **Products**.

3.	On the Products page of the console, verify that your requested products have been added to the site and then select the **Adobe Analytics** icon.

      ![Admin Console products page](../../img/events_atrig_03.png "Admin Console products page")

4.	Select the **Configuration Details** tab and do the following:

    1.	Verify the **Name** of your configuration.

    2.	Provide a **Description**, if applicable.

    3.	Verify the display name that will show on the **Products** page.

    4.	Under **Enabled Services**, select the option for **Triggers**.

        ![Admin Console: configure product](../../img/events_atrig_04.png "Admin Console: configure product")

  5. To give permissions to users who want access to Adobe services in the cloud:

     1. Select **User management** and then select **Users**.
     2. Select the user&rsquo;s name.
     
        ![Selecting the user name](../../img/events_atrig_05.png "Selecting the user name")

     3. For the user&rsquo;s **Access and rights**, provide **Product Access** and **Admin Rights** from the drop-down for the available products and services.
     
        ![Setting the user access rights](../../img/events_atrig_06.png "Setting the user access rights")

#### Configure reporting for Triggers

To configure reporting for Triggers:

1. On the Marketing Cloud home page, select the **App** button at the top right corner.

    ![New apps icon](../../img/events_atrig_07.png "New apps icon")

2. Select the Analytics Launcher.

    ![Analytics Launcher icon](../../img/events_atrig_08.png "Analytics Launcher icon")

3. On the **Admin** tab of the Analytics home screen, select the **Report Suites** option. (**Note:** You must have administrative privileges for the **Admin** tab to appear on your screen.)

    ![Admin tab, selecting Report Suites](../../img/events_atrig_09.png "Admin tab, selecting Report Suites")

4. On the Reports Suites Manager page, select **Create New** and then select **Report Suite**. Configure the new report suite so that it is accessible in Adobe Analytics.

    ![Creating a new report suite](../../img/events_atrig_10.png "Creating a new report suite")

#### Configure DTM

To configure DTM:

1.	On the Marketing Cloud home page, select the **Apps** icon and then select **Activation**.

    ![Marketing Cloud Admin, selecting Activation](../../img/events_atrig_11.png "Marketing Cloud Admin, selecting Activation")

2.	On the Activation page, select **Dynamic Tag Management**.

    ![Launching Dynamic Tag Management](../../img/events_atrig_12.png "Launching Dynamic Tag Management")

3.	On the **Overview** tab, select the **Settings** icon.

    ![Opening settings](../../img/events_atrig_13.png "Opening settings")

4. On the Settings page, set the variable as follows:

    1. Expand the **Global Variables** section.
    2. Choose the **Evar name** drop-down and then select **eVar3**.
    3. Select the **Set as** option.
    4. In the **Set as** field, type the following URL element:
        ```
        %window.location.host%%window.location.pathname%
        ```
    5. Click **Save eVar**.

        ![Global Variables, saving evar setttings](../../img/events_atrig_14.png "Global Variables, saving evar setttings")

5. On the **Approvals** tab, select the **Approve** button.    

    ![Selecting the Approve button](../../img/events_atrig_15.png "Selecting the Approve button")

6. On the **Overview** tab, select the **Publish Queue** button.

    ![Selecting the Publish Queue button](../../img/events_atrig_16.png "Selecting the Publish Queue button")

#### Specify a new trigger

You can specify triggers for many events on your site. This example will set notifications to be sent when carts are abandoned: set a trigger for sessions when the user visits either a **cart.html**, **checkout.html** or **order.html** page, but never reaches the **thank-you.html** page within a ten minute session. The trigger indicates that the user added products to the cart, and was about to make a purchase, but later decided otherwise, or forgot to complete the purchase.

To specify a new trigger:

1. On the Marketing Cloud home page, select the **Apps** icon, and then select **Activation**.

    ![Marketing Cloud Admin, selecting Activation](../../img/events_atrig_17.png "Marketing Cloud Admin, selecting Activation")

2. On the Triggers card, select the **Launch** button.
    
    ![Launching Triggers](../../img/events_atrig_18.png "Launching Triggers")

3. On the **Triggers** page, select the **New Triggers** button, and then choose **Abandonment**.

    ![Selecting the Abandonment trigger](../../img/events_atrig_19.png "Selecting the Abandonment trigger")

4. On the **New Trigger** box, specify a **Name** and provide a **Description** for your trigger. Select the **Report Suite** that you previously setup from the drop-down field.

    ![Entering trigger details](../../img/events_atrig_20.png "Entering trigger details")

5. On the **Triggers Settings** page, define the business rules for your trigger. You can drag a dimension/metric box from the left panel to the right side of the screen and then specify the business rules for what must happen and what must not happen in a session. In this case, set the trigger to fire after 10 minutes of inactivity after the rules are met.

    ![Defining trigger business rules](../../img/events_atrig_21.png "Defining trigger business rules")

6. Save your changes.

Once you save the trigger, any event in your report suite that meets the defined business rules criteria will cause a trigger to fire. You can view the status of triggers on the **Triggers** page.

![Viewing triggers listing](../../img/events_atrig_22.png "Viewing triggers listing")

## Use Adobe I/O

Use Adobe I/O to create a new integration with the Console. To do this:

1. After signing in to the [Adobe I/O Console](https://adobe.io/console), select **New Integration**.

    ![The New Integration button](../../img/events_atrig_23.png "The New Integration button")

2. Choose **Receive near real-time events** and continue.

    ![Choosing to receive near real-time events](../../img/events_atrig_24.png "Choosing to receive near real-time events")

3. Choose **Analytics Triggers** as an event provider and  continue.

    ![Selecting the Analytics Triggers provider](../../img/events_atrig_25.png "Selecting the Analytics Triggers provider")

4. Choose **Continue** again to move on to the next page without making any changes.

5. Provide the **Name** and **Description** for your integration.

    ![Name and describe your integration](../../img/events_atrig_26.png "Name and describe your integration")

6. Generate a public certificate. To do this:

    1. Open a terminal and execute the following command:

        `openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate_pub.crt`

    2. Upload the public certificate by selecting the **Select a File** link and then by uploading the certificate from your computer:

        ![Upload the public certificate file](../../img/events_atrig_27.png "Upload the public certificate file")


7. Add your event registration (webhook) details and save. For information on creating and registering webhooks, see [Introduction to Webhooks]../intro/webhook_docs_intro.md).

    ![Providing webhook details](../../img/events_atrig_28.png "Providing webhook details")

## Watch the solution work

Your enterprise may have its own tool that you can use to subscribe and listen to webhook events. Alternatively, you can use the following procedure to set up notifications with Slack.
To watch your trigger work on Slack:

1. Clone the repository and follow the setup described on https://github.com/hirenshah111/webhook_server.

2. Modify the Slack details in webhook_server/public/javascripts/app.js according to how you want to see the notifications.

3. Run the application and create a **triggers2** listener, then select **Connect**.

    ![Listening to webhook events](../../img/events_atrig_29.png "Listening to webhook events")


Trigger messages are received as `POST` requests on this thread.

## Authors
- Hiren Shah [@hirenshah111](https://github.com/hirenshah111).
- John Wight [@johnwight](https://github.com/johnwight).

## Feedback?

Please help make this solution as useful as possible. If you find a problem in the documentation or have a suggestion, select the **Issues** tab on this GiHhub repository, and then the **New issue** button. Provide a title and description for your comment and then select the **Submit new issue** button.

![Submit a new issue](../../img/events_atrig_30.png "Submit a new issue")
