---
parser: v2
author_name: Daniel Wroblewski
author_profile: https://github.com/thecodester
auto_validation: true
time: 20
tags: [ tutorial>beginner, software-product>sap-build, software-product>sap-build-apps, software-product>sap-build-process-automation]
primary_tag: software-product>sap-build
---
  
 

# 7 - Build and Deploy App to BTP
<!-- description --> Build your app and deploy it to SAP BTP, as part of the SAP Build CodeJam.


## Prerequisites
- You have completed the previous tutorial for the SAP Build CodeJam, [Add Approval Flow to Process](codejam-06-spa-approval).





## You will learn
- How to compile/build your app to an MTAR file
- How to deploy your app to SAP BTP





## Intro




### Build your app
1. Open your **ShoppingApp** SAP Build Apps project.

2. Click **Publish**.

    Click **Build and Deploy**.

    ![Open Publish](images/build0.png)

    The build screen is displayed.

    ![Build screen](images/build1.png)
   
2. Click **Create Configuration**.
   
    ![Create configuration](images/build2.png)

3. Click **SAP Build Work Zone**.

    ![SAP Build Work Zone configuration](images/build3.png)

4. For the name of the configuration, enter: `StandardWZ`

    ![Configuration name](images/build4.png)

    Click **Create**.

5. Click the 3 dots on the new configuration, and select **Build**.

    ![Start build](images/build5.png)

    Enter `1.0.0` for the version, and click **Build**.

    ![Click build](images/build6.png)

    The build will start, and you will see **Building** as the status.

    ![Building](images/build7.png)

    It may take about 6 minutes to finish, so this is a time for questions and coffee and cake. ☕🍰

    When complete, you will see the **Delivered** status.

    ![Delivered](images/build8.png)








### Deploy your app
1. Click on the row with your successful build.

    ![Successful build](images/deploy1.png)

2. Click **Deploy**.

    ![Click deploy](images/deploy2.png)

3. Select the Cloud Foundry endpoint for your SAP BTP. 

    You can see your landscape by going to the SAP BTP cockpit, on the **Overview** tab, and see the URL for your Cloud Foundry environment.

    ![SAP BTP landscape](images/deploy-landscape.png)

4. Click **Login with BTP**.

    ![Login](images/deploy4.png)

    Click on your user.

    ![User](images/deploy5.png)

    This will log you into Cloud Foundry. Click **Authorize**.

    ![Authorize](images/deploy6.png)

    Select your Cloud Foundry organization and space – to where you are deploying. If you are using a trial account, you likely will have just one organization and space.

    ![Org and space](images/deploy7.png)

    Click **Continue**.

    You should get a progress bar. It should take about **60 seconds** to complete.

    ![Progress bar](images/deploy8.png)

5. When the deployment finishes, you should get a URL to your deployed app.

    ![App URL](images/deploy9.png)

    If you click the link, your app should be displayed.


If you lose the link to your app, you can see all your deployed apps in your trial account, under **HTML5 Applications**.

![HTML5 Applications](images/deploy10.png)


