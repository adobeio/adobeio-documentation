<!--:navorder: 2-->

# Integrate Analytics Triggers with Adobe I/O Events

These instructions describe how to use Adobe Analytics triggers to notify you of Adobe I/O events, including the behavior of your site's users. Follow the instructions below to try the solution yourself.

- [Introduction](#Introduction)
- [Set Up Products](#Set-Up-Products)
- [Use Adobe I/O](#Use-Adobe-I/O)
- [Watch the Solution Work](#Watch-It-Work)

**Resources**
- [Debugging](help/debug#analytics)
- [FAQ](help/faq#analytics)

<a id="Introduction">&nbsp;</a>

## Introduction

### What are Triggers?
Triggers is a Marketing Cloud Activation core service that enables marketers to identify, define, and monitor key consumer behaviors, and then generate cross-solution communication to re-engage visitors.
For more information on triggers, see the [Triggers Help Page](https://marketing.adobe.com/resources/help/en_US/mcloud/triggers.html).


Before setting up and using Adobe I/O, you will need to do the following:

1. [Obtain Product Authorization](#Obtain-Product-Authorization)
2. [Obtain Administrative Permissions](#Obtain-Administrative-Permissions)

<a id="Obtain-Product-Authorization">&nbsp;</a>

### Obtain Product Authorization

To complete this solution, you will need authorization to use the following services:
*   Adobe Analytics, including Triggers
*	Adobe Marketing Cloud Activation Core Services
*   Adobe Experience Manager (AEM), or an external website that connects to Analytics
*   An [Adobe ID](https://helpx.adobe.com/x-productkb/global/adobe-id-account-change.html), if you do not have one already

<a id="Obtain-Administrative-Permissions">&nbsp;</a>

### Obtain Administrative Permissions

You will also need administrative permissions for the following:
* Adobe Analytics
* Your enterprise organization
* AEM, if using that service to connect to Analytics

If you do not have administrative permissions, please contact ioevents-beta@adobe.com. After requesting administrative permissions, watch for an email from Adobe Systems Incorporated, as shown:

   ![Admin rights email](../../img/events_atrig_01.png)

<a id="Set-Up-Products">&nbsp;</a>

## Set Up Products

To set up Adobe products for this solution:

1. [Set Up AEM](#Integrate-AEM)
1. [Set Up Analytics](#Set-up-Triggers)

<a id="Integrate-AEM">&nbsp;</a>

### Set Up AEM

To set up AEM with Analytics Triggers and Dynamic Tag Management (DTM), follow the [step-by-step documentation](https://github.com/johnwight/Adobe-AEM-with-DTM-and-Analytics) for that configuration. You can also follow the video shown below:


   [![Video: Integrate AEM](../../img/events_atrig_02.jpg)](https://youtu.be/vtcZCS-LFeg "Video: Integrate AEM")

<a id="Set-up-Triggers">&nbsp;</a>

### Set up Analytics

To set up Analytics:

1. [Get Product Access through Admin Console](#Admin-Console)
2. [Configure Reporting for Triggers](#Reporting-for-Triggers)
3. [Configure DTM](#Configure-DTM)
4. [Specify a New Trigger](#Specify-a-New-Trigger)

<a id="Admin-Console">&nbsp;</a>

#### Get Product Access through Adobe Admin Console	 

To get access through the Adobe Admin Console:

1.	Sign into the console by clicking the **Sign in** button on the administrator rights email you received from Adobe and then providing your credentials.

2.	On the main screen of the Admin Console, click **Products**.

3.	On the Products page of the console, verify that your requested products have been added to the site and then click the Adobe Analytics icon.

      ![admin products page](../../img/events_atrig_03.png)

4.	Click the **Configuration Details** tab and do the following:

    1.	Verify the **Name** of your configuration.

    2.	Provide a **Description**, if applicable.

    3.	Verify the display name that will show on the **Products** page.

    4.	Under **Enabled Services**, select the option for **Triggers**.

     ![admin console configure product](../../img/events_atrig_04.png)

  5. To give permissions to users who want access to Adobe services in the cloud:

     1. Click **User management** and then click **Users**.
     2. Click on user's name.
     
      ![user name](../../img/events_atrig_05.png)

     3. For the user's **Access and rights**, provide **Product Access** and **Admin Rights** from the drop-down for the available products and services.

      ![user permissions](../../img/events_atrig_06.png)

<a id="Reporting-for-Triggers">&nbsp;</a>

#### Configure Reporting for Triggers

To configure reporting for Triggers:

1. On the Marketing Cloud home page, click the **App** button on top right corner.

    ![new apps icon](../../img/events_atrig_07.png)

2. Click on Analytics Launcher.

    ![analytics launcher](../../img/events_atrig_08.png)

3. On the **Admin** tab of the Analytics home screen, click the **Report Suites** option. You must have administrative privileges for the **Admin** tab to appear on your screen.

    ![admin tab for report suites](../../img/events_atrig_09.png)


4. On the Reports Suites Manager page, click **Create New** and select **Report Suite**. Configure the new report suite so that it is accessible in Adobe Analytics.

    ![report suite manager](../../img/events_atrig_10.png)


<a id="Configure-DTM">&nbsp;</a>

#### Configure DTM

To configure DTM:

1.	On the Marketing Cloud home page, click the **Apps** icon and then click **Activation**.

    ![dtm 1 icon](../../img/events_atrig_11.png)

2.	On the Activation page, click **Dynamic Tag Management**.

    ![dtm card](../../img/events_atrig_12.png)

3.	On the **Overview** tab, click the **Settings** icon.

    ![settings icon](../../img/events_atrig_13.png)

4. On the Settings page, set the variable as follows:

    1. Expand the **Global Variables** section.
    2. Click the **Evar name** drop-down and select **eVar3**.
    3. Select the **Set as** option.
    4. In the **Set as** field, type the following URL element:
        ```
        %window.location.host%%window.location.pathname%
        ```
    5. Click **Save eVar**.

        ![set evar](../../img/events_atrig_14.png)

5. On the **Approvals** tab, click the **Approve** button.    

    ![approve button](../../img/events_atrig_15.png)

6. On the **Overview** tab, click the **Publish Queue** button.

    ![publish button](../../img/events_atrig_16.png)

<a id="Specify-a-New-Trigger">&nbsp;</a>

#### Specify a New Trigger

You can specify triggers for many events on your site. For example, in this case, we will set notifications to be sent when  carts are abandoned. We will set a trigger for sessions when the user visits either a **cart.html**, **checkout.html** or **order.html** page, but never reaches the **thank-you.html** page within a ten minute session. The trigger indicates that the user added products to the cart, and was about to make a purchase, but later decided otherwise, or forgot to complete the purchase.

To specify a new trigger:

1. On the Marketing Cloud home page, click the **Apps** icon and then click **Activation**.

    ![dtm 1 icon](../../img/events_atrig_17.png)

2. On the Triggers card, click the **Launch** button.
    
    ![resize 2 dtm card](../../img/events_atrig_18.png)

3. On the **Triggers** page, click the **New Triggers** button and select **Abandonment**.

    ![abandonment trigger](../../img/events_atrig_19.png)

4. On the **New Trigger** box, specify a **Name** and provide a **Description** for your trigger. Select the **Report Suite** that you previously setup from the drop-down field.

    ![trigger dialog](../../img/events_atrig_20.png)

5. On the **Triggers Settings** page, define the business rules for your trigger. You can drag a dimension/metric box from the left panel to the right side of the screen and then specify the business rules for what must happen and what must not happen in a session. In this case, we set the trigger to fire after 10 minutes of inactivity after the rules are met.

    ![triggers settings page](../../img/events_atrig_21.png)

6. Click the **Save** button.

Once you save the trigger, any event in your report suite that meets the defined business rules criteria will cause a trigger to fire. You can view the status of triggers on the **Triggers** page.

![triggers listing](../../img/events_atrig_22.png)

<a id="Use-Adobe-I/O">&nbsp;</a>

## Use Adobe I/O

Use Adobe I/O by creating a new integration with the Console. To do this:

1. After signing in to the [Adobe I/O Console](https://adobe.io/console), click **New Integration**.

    ![new integration button](../../img/events_atrig_23.png)

2. Select **Receive real-time events** and click **Continue**.

    ![real time events](../../img/events_atrig_24.png)

3. Select **Analytics Triggers** as an event provider and click **Continue**.

    ![create io trigger](../../img/events_atrig_25.png)

4. Click **Continue** to move on to the next page without making any changes.

5. Provide the **Name** and **Description** for your integration.

    ![name integration box](../../img/events_atrig_26.png)

6. Generate a public certificate. To do this:

    1. Open a terminal and execute the following command:

        `openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate_pub.crt`

    2. Upload the public certificate by clicking the **Select a File** link and then by selecting the certificate from your computer:

        ![public certificate](../../img/events_atrig_27.png)


7. Add Webhook details and Click **Save**. For information on creating and registering webhooks, see [Introduction to Webhooks](https://github.com/adobeio/adobeio-events-documentation/blob/master/Webhook_docs_intro.md).

    ![webhook details](../../img/events_atrig_28.png)


<a id="Watch-It-Work">&nbsp;</a>

## Watch the solution work

Your enterprise may have its own tool that you can use to subscribe and listen to webhook events. Alternatively, you can use the following procedure to set up notifications with Slack.
To watch your trigger work on Slack:

1. Clone the repository and follow the setup described on https://github.com/hirenshah111/webhook_server.

2. Modify the Slack details in webhook_server/public/javascripts/app.js according to how you want to see the notifications.

3. Run the application and create a **triggers2** listener, then click **Connect**.

    ![listen to webhook](../../img/events_atrig_29.png)


Trigger messages are received as `POST` requests on this thread.

## Authors
- Hiren Shah [@hirenshah111](https://github.com/hirenshah111).
- John Wight [@johnwight](https://github.com/johnwight).

## Feedback?

Please help make this solution as useful as possible. If you find a problem in the documentation or have a suggestion, click the **Issues** tab on this GiHhub repository and then click the **New issue** button. Provide a title and description for your comment and then click the **Submit new issue** button.

![submit new issue](../../img/events_atrig_30.png)
