# Integrate Analytics Triggers with Adobe IO Events

These instructions describe how to use Adobe Analytics triggers to notify you of Adobe IO events, including the behavior of your site's users. Follow the instructions to try the solution yourself. You will need an Adobe Experience Manager license to complete this solution.

## Introduction

Use Adobe IO to integrate with Analytics triggers so that you can be notified of events on your website.

These instruction include the following tasks:

1. Setup the Analytics triggers environment
1. Configure triggers and rules
1. Integrate with Adobe I/O
1. View event notifications through webhooks

### What is a trigger?
A trigger is a Marketing Cloud Activation core service that enables marketers to identify, define, and monitor key consumer behaviors, and then generate cross-solution communication to re-engage visitors.
For more information on triggers, see the [Triggers Architecture Wiki](https://wiki.corp.adobe.com/display/omtrplatform/Triggers+Architecture).

## What's Needed

To complete this demo, you will need authorization to the following services:
*	Adobe Marketing Cloud
*	Adobe Analytics
*	Adobe Analytics Administrator
*	Default Report Suite
*	Activation Core Services
*	Triggers


### Authorization

To receive authorization, follow these steps:  

1.	Send an email to Maria Jeronimo (mjeronim@adobe.com) or Ron Quan (rquan@adobe.com) requesting administrative rights for your organization and access to the tools listed above.
2.	Watch for an email from Adobe Systems Incorporated providing authorization for administrator rights, as shown:


    ![admin rights 2](https://user-images.githubusercontent.com/29133525/30258738-46dfb044-9679-11e7-9d0b-f724c32dacac.png)


### Obtain an Adobe ID

If you have not already done so, obtain an [Adobe ID](https://helpx.adobe.com/x-productkb/global/adobe-id-account-change.html) account.

## Setup Triggers

To set up Adobe Analytics triggers:

1. [Configure the Adobe Admin Console](#Admin Console).
2. [Configure reporting for triggers](#Reporting for Triggers).
3. [Activate the triggers](#Activate Triggers).
4. [Specify a new trigger](#Specify new trigger).




### <a name="Admin Console">Configure the Adobe Admin Console</a>	 

To configure the Adobe Admin Console:

1.	Sign into the console by clicking the **Sign in** button on the administrator rights email you received from Adobe and then providing your credentials.

2.	On the main screen of the Admin Console, click **Products**.


3.	On the Products page of the console, verify that your requested products have been added to the site and then click the Adobe Analytics icon.

    ![admin products page](https://user-images.githubusercontent.com/29133525/30259844-d5bfffce-9680-11e7-89fc-fee114664b1e.png)
    
4.	Click the **Configuration Details** tab and do the following:

    1.	Verify the **Name** of your configuration.

    2.	Provide a **Description**, if applicable.

    3.	Verify the display name that will show on the **Products** page.

    4.	Under **Enabled Services**, select the option for **Triggers**.
    
    
        ![admin console configure product](https://user-images.githubusercontent.com/29133525/30259707-086fdda0-9680-11e7-8ad9-e00e5a733cd1.png)

  5. To give permissions to users who want access to Adobe services in the cloud:
     
     1. Click **User management** and then click **Users**.
     2. Click on user's name.
     
         ![user name](https://user-images.githubusercontent.com/29133525/30260182-0e9e21ca-9683-11e7-8ce8-c6def3dcc2c6.png)
       
     3. For the user's **Access and rights**, provide **Product Access** and **Admin Rights** from the drop-down for the available products and services.
     
         ![user permissions](https://user-images.githubusercontent.com/29133525/30260361-258f91e2-9684-11e7-8d1b-4c58d1d22b8b.png)
  

  

### <a name="Admin Console">Configure Reporting for Triggers</a>

To configure reporting for Triggers:

1. On the Marketing Cloud home page, click the **App** button on top right corner.

    ![new apps icon](https://user-images.githubusercontent.com/29133525/30260555-554e9dbe-9685-11e7-9ecd-86b239f007f9.png)

2. Click on Analytics Launcher.

    ![analytics launcher](https://user-images.githubusercontent.com/29133525/30260673-01c26c88-9686-11e7-8b31-5203c6386591.png)

3. On the **Admin** tab of the Analytics home screen, click the **Report Suites** option. You must have administrative privileges for the **Admin** tab to appear on your screen.

    ![admin tab for report suites](https://user-images.githubusercontent.com/29133525/30260836-e3d8b370-9686-11e7-99e6-8832f74b8578.png)


4. On the Reports Suites Manager page, click **Create New** and select **Report Suite**. Configure the new report suite so that it is accessible in Adobe Analytics.

    ![report suite manager](https://user-images.githubusercontent.com/29133525/30260977-9a224b50-9687-11e7-9bb9-c847a03e5735.png)


### <a name="Activate Triggers">Activate the Triggers</a>

1. To activate the triggers, go to the Marketing Cloud home page again and click the **Activation** option.

    ![triggers activation icon](https://user-images.githubusercontent.com/29133525/30261169-8369e322-9688-11e7-8f22-9a8e14b7f804.png)


2.  On the Activation page, click **Launch** on the Triggers card as shown below:

    ![trigges launcher](https://user-images.githubusercontent.com/29133525/30261304-3e41845c-9689-11e7-9fc1-e61b0530eab4.png)



## Integrate AEM

To integrate Analytics triggers and Dynamic Tag Management (DTM) with Adobe Experience Manager, follow the step-by-step video shown below:




## Configure DTM

To configure Dynamic Tag Management (DTM):


1.	On the Marketing Cloud home page, click the **Apps** icon and then click **Activation**.

    ![dtm 1 icon](https://user-images.githubusercontent.com/29133525/30288083-b9ca747a-96e4-11e7-9c23-5f97daf4233c.png)

2.	On the Activation page, click **Dynamic Tag Management**.

    ![dtm card](https://user-images.githubusercontent.com/29133525/30288581-87f3e362-96e6-11e7-9cad-df4229cf8645.png)

3.	On the **Overview** tab, click the **Settings** icon.

    ![analytics settings icon](https://user-images.githubusercontent.com/29133525/30289100-3ab1926e-96e8-11e7-9e7b-88d2c0e3b559.png)

4. On the Settings page, set the variable as follows:
    
    1. Expand the **Global Variables** section.
    2. Click the **Evar name** drop-down and select **eVar3**.
    3. Select the **Set as** option.
    4. In the **Set as** field, type the following URL element:
        ```
        %window.location.host%%window.location.pathname%
        ```
    5. Click **Save eVar**.
    
        ![set evar](https://user-images.githubusercontent.com/29133525/30289707-43110e1a-96ea-11e7-91a8-0eccac12ec1d.png)
       
5. On the **Approvals** tab, click the **Approve** button.    

    ![approve button](https://user-images.githubusercontent.com/29133525/30290129-8f44bd8a-96eb-11e7-9b31-87604beb8a53.png)

6. On the **Overview** tab, click the **Publish Queue** button.

    ![publish button](https://user-images.githubusercontent.com/29133525/30290297-0f3a7cb4-96ec-11e7-8e68-2b45200e76ca.png)
    
    
### <a name="Specify new trigger">Specify a New Trigger</a>

You can specify triggers for many events on your site. For example, in this case, we will set notifications to be sent when  carts are abandoned. We will set a trigger for sessions when the user visits either a **cart.html**, **checkout.html** or **order.html** page, but never reaches the **thank-you.html** page within a ten minute session. The trigger indicates that the user added products to the cart, and was about to make a purchase, but later decided otherwise, or forgot to complete the purchase.

To specify a new trigger:

1. On the Marketing Cloud home page, click the **Apps** icon and then click **Activation**.

    ![dtm 1 icon](https://user-images.githubusercontent.com/29133525/30290835-1765ebf6-96ee-11e7-880c-46177a716ac5.png)

2. On the Triggers card, click the **Launch** button.

    ![trigges launcher](https://user-images.githubusercontent.com/29133525/30290916-5a4e58a4-96ee-11e7-8751-ce859cd75433.png)


3. On the **Triggers** page, click the **New Triggers** button and select **Abandonment**.

    ![abandonment trigger](https://user-images.githubusercontent.com/29133525/30291142-16358d44-96ef-11e7-85f7-e3b30d809087.png)

4. On the **New Trigger** box, specify a **Name** and provide a **Description** for your trigger. Select the **Report Suite** that you previously setup from the drop-down field.

    ![trigger dialog](https://user-images.githubusercontent.com/29133525/30291268-92d7eaa4-96ef-11e7-83db-d6c6da15dcf3.png)


6. On the **Triggers Settings** page, define the business rules for your trigger. You can drag a dimension/metric box from the left panel to the right side of the screen and then specify the business rules for what must happen and what must not happen in a session. In this case, we set the trigger to fire after 10 minutes of inactivity after the rules are met. 
   
    ![triggers settings page](https://user-images.githubusercontent.com/29133525/30291990-c37f6ff4-96f1-11e7-84ff-51559bd886c2.png)


7. Click the **Save** button.


Once you save the trigger, any event in your report suite that meets the defined business rules criteria will cause a trigger to fire. You can view the status of triggers on the **Trigger** page.

![triggers listing](https://user-images.githubusercontent.com/29133525/30292257-af31b0ce-96f2-11e7-8e3c-32531e0d0d9e.png)

## IO Integration

To integrate with Adobe IO:

1. On the Adobe IO console, click **New Integration**.

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
    2. Upload the public certificate as shown:
    
        ![public certificate](https://user-images.githubusercontent.com/29133525/30292851-d78ee8e6-96f4-11e7-82ea-61bd85eb4212.png)


7. Add Webhook details and Click **Save**.

    ![webhook details](https://user-images.githubusercontent.com/29133525/30292950-2c3854a4-96f5-11e7-9acb-21df39c44a02.png)



## Watch It Work

To watch your trigger work:

1. Clone the repository and follow the setup described on https://github.com/hirenshah111/webhook_server.

2. Modify the Slack details in webhook_server/public/javascripts/app.js according to how you want to see the notifications.

3. Run the application and create a **triggers2** listener, then click **Connect**.

    ![listen to webhook](https://user-images.githubusercontent.com/29133525/30293183-e0657718-96f5-11e7-8696-86153cc4d5ff.png)


Trigger messages are received as `POST` requests on this thread.
