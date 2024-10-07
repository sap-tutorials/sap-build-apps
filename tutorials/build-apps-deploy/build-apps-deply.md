---
parser: v2
author_name: Daniel Wroblewski
author_profile: https://github.com/thecodester
auto_validation: true
time: 10
tags: [ tutorial>beginner, software-product>sap-business-technology-platform,software-product>sap-build, software-product>sap-build-apps--enterprise-edition]
primary_tag: software-product>sap-build
---
 

# Deploy SAP Build App to SAP BTP
<!-- description --> Deploy an app created with SAP Build Apps to HTML5 applications on SAP BTP.

 
## Prerequisites
- To deploy, you need to be a member of the Cloud Foundry org and space to which you will be deploying to.


## You will learn
- How to deploy an SAP Build app to SAP BTP



## Intro
After creating an app in SAP Build Apps, and after viewing the preview on the web and on your mobile device, you can then deploy it directly to SAP BTP, like any other HTML5 application.

As of the writing of this tutorial, you could only deploy to an Cloud Foundry org's default space.

>**IMPORTANT:** You need to be a member of the Cloud Foundry org and space to which you will be deploying to.

>If you are an admin of the SAP BTP subaccount, you can see who is an org member by going to the cockpit and navigating to **Cloud Foundry > Org Members**.

>![Org members](org-members.png)

> If you navigate to the space, there is a similar members for space members.

>![Space members](org-space-members.png)

---


### Create build configuration
1. Open the **Launch** tab.
   
2. Select **Open Build Service**.
   
    ![Build service](build1.png)

    The following is displayed, where you can create a build configuration (a set of properties to describe how you want your app to be built) and to deploy it.

    ![Build home page](configure1.png)

3. Click **Create Configuration**.

    Select **SAP Build Work Zone**.
    
    ![Create configuration](build1a.png)

2. Enter **StandardWZ** for the configuration name, and click **Create**.

    ![Start to create configuration](build2.png)

    This creates a build configuration.

    ![Configuration created](build2a.png)
    


### Build the app
1. Click the 3 dots on the configuration, and select **Build**.

    ![Click build button](build3.png)

2. Enter `1.0.0` for the version number, and click **Build**.
   
    ![Start build](build3a.png)

    The build will start.

    ![Build started](build3b.png)

    After about 5 minutes, the status will change from **Building** to **Delivered**.

    ![Build completed](build3c.png)




### Deploy app
Before deploying, you must have a build that completed successfully, showing a status of **Delivered**.

>**IMPORTANT:** In order to deploy, you must be a member of Cloud Foundry in your SAP BTP tenant, and a member of the space to which you want to deploy.
>
>An admin can add you to the org and space, starting from the SAP BTP cockpit.
>
>![Cloud Foundry](deploy-prereqs.png)

1. Click the **>** symbol for the build you want to deploy.

    ![Open build](deploy1.png)

2. Click **Deploy**.

    ![Deploy button](deploy2.png)

3. In the dialog, you have to specify to which Cloud Foundry environment you want to deploy it, as well as your credentials.

    ![Deploy dialog](deploy3.png)

    For **API Endpoint**, go to the **Overview** page of your SAP BTP subaccount, and check which API endpoint is used by your subaccount.

    ![Check API endpoint](deploy4.png)

    Then select it in the dropdown.

    ![Select API endpoint](deploy5.png)

3. Once you select your API endpoint, you need to log into Cloud Foundry.

    Click **Login with BTP**.

    ![Click Login](deploy6.png)

    A dialog opens, and lets you by default sign in with your default SAP BTP user (using the default IDP, not the custom one for SAP Build Apps). Select the default user.
       
    ![Select user](deploy7.png)

    This will log you in and populate the **Organization** dropdown list with the ones you have permission to. 

4. Here, I have rights to several SAP BTP subaccounts. I'll choose the one for my SAP BTP trial account.

    ![Select org](deploy8.png)

    Once you select an organization, you will get a list of spaceds to which you belong. Select the one you want to deploy to. Most users doing this tutorial will only have one, and it will be named dev.

    ![Select space](deploy9.png)

    Click **Continue**.

    The app will start to deploy.

    ![Deployment starts](deploy9a.png)

5. Once the deployment ends successfully, you will get a linkto the deployed app. Click the link to open the app.

    ![Deployment successful](deploy9b.png)

    Once deployed, you can go back to the cockpit and see the app under **HTML5 Applications**, with the same number as in the URL you got above.

    ![Deployment successful](deploy9c.png)

### Running the app
You can click the link returned when you deployed the app. But you may get the following unless you have set up SAP Build Apps to work with SAP Build Work Zone, which is the component that runs your deployed apps.

![Run with error](run1.png)

You must configure your SAP BTP according to the instructions here: [Integration with the App Editor - Deployed Applications](https://help.sap.com/docs/build-apps/service-guide/integration-to-app-builder#deployed-applications?).  

Here is a summary of what needs to be done as of the time of publishing this tutorial.

>The screenshots and instructions below are based on using a SAP BTP trial account. If you have a productive SAP BTP account, the steps may be different but the basics would be the same.

1. Go to the SAP BTP cockpit, navigate to **Instances and Subscriptions**.

    Open the **SAP Build Work Zone, standard edition** application.

    ![SAP Build Work Zone](run2.png)

    On the left navigation, click **Settings**, then select the **Identity Authentication** tab.

    Select the checkbox, and click **Enable**.

    After a few seconds, you should see a success message.

2. Go to the SAP BTP cockpit, navigate to **Instances and Subscriptions**.

    Open **Cloud Identity Services**.

    ![Cloud Identity Services](run3.png)

    This opens an admin dashboard. Select the **Applications & Resources > Applications**.
    
    Then on the left, select the **SAP Build Work Zone** application.
    
    ![Work Zone app](run4.png)

    Scroll down and select **Dependencies**.

    ![Dependencies](run5.png)

    Add a dependency with the following properties:

    | Field | Value |
    |------|--------|
    | Dependency Name | Enter `sap-build-apps-api` |
    | Application | Select the application for **SAP Build Apps** on your subaccount |
    | API | Select **SAP Build Apps backend projects** |

    Click **Save**.
    
    ![New dependency](run6.png)
