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

    After about 5 minutes, the status will change from **Building** to **Done**.





### Deploy app
Before deploying, you must have a build that completed successfully, which then shows a **Deploy** button.

1. Click **Deploy**.

2. Select the endpoint for your SAP BTP tenant.

    Select the organization and space, and click **Continue**.



