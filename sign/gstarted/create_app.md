# Create an Application

To use Adobe Sign APIs, you need to first create an application.

Adobe Sign uses the OAuth authentication protocol to authorize requests for any Sign API endpoint. So, you need to get the Application ID and Application Secret from the web UI of Adobe Sign.  

1. [Log in to Adobe Sign](https://secure.echosign.com/public/login).

2.  Select **API** from the top menu.  

    ![Selecting API access](../img/sign_gstarted_1.png) 
    
    If you are already an Enterprise customer, you may not see the API link. In that case, click Account to proceed.

3. Select **API Applications.**  

    ![Selecting API Applications](../img/sign_gstarted_2.png)  

4. Select the **Create** (+) icon and provide details about your app.  

    ![Creating a new app](../img/sign_gstarted_3.png)

When you create a new app, you need to choose the right domain:

- **CUSTOMER:**  Apps for internal use and testing. Use this domain if you need your app to access data only from your account.
- **PARTNER:**  Apps for production and public use. Use this domain if you need your app to access data in any Adobe Sign account.

Next: [Configure OAuth](configure_oauth.md)

