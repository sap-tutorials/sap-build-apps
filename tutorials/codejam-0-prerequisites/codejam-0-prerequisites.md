---
parser: v2
author_name: Ian Thain
author_profile: https://github.com/ithain
auto_validation: true
time: 30
tags: [ tutorial>beginner, software-product>sap-build, software-product>sap-build-apps]
primary_tag: software-product>sap-build
---
   

# Set Up Prerequisites for SAP Build CodeJam
<!-- description --> Get ready for your SAP Build CodeJam now!

## Prerequisites
- Do this before you come to CodeJam ⏱️
- Bring your laptop 💻
- Bring your device 📱
- Be ready to have FUN! 🤗






## You will learn
- How to set up the SAP BTP trial account
- How to set up SAP Build Apps
- How to set up SAP Process Automation
- How to create a destination for the CAP service



 



## Intro






### Create SAP BTP trial account
>**IMPORTANT:** For SAP Build and this CodeJam, you must have an SAP BTP trial account on the **US EAST (VA) - AWS** region.

>![alt text](USEast.png)

If you already have an SAP BTP trial account in the  **US EAST (VA) - AWS** region, you can skip this step.

If you already have an SAP ID but not an SAP BTP trial account, you can go directly to [https://account.hanatrial.ondemand.com/trial](https://account.hanatrial.ondemand.com/trial) and create an SAP BTP trial account. 

If you do not already have an SAP ID, follow this tutorial: [Get a Free Account on SAP BTP Trial](https://developers.sap.com/tutorials/hcp-create-trial-account.html)

<!-- border -->
![Get trial account tutorial](BTPTut.png)

When you are done, you can get to your trial account by going to [https://account.hanatrial.ondemand.com/](https://account.hanatrial.ondemand.com/).






### Install SAP Build Apps
Here is video from **Daniel Wroblewski** showing how to install SAP Build Apps.

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZpQM2B1v2GY" frameborder="0" allowfullscreen></iframe> 

If you prefer, here are the step-by-step instructions.

1. Go to the trial account (not the subaccount). Your screen will look something like this:

    ![Enter subaccount](installapps1.png)

2. From the left-side menu, click **Boosters**.

    Search for the `SAP Build Apps`, and then click the tile for the **Get Started with SAP Build Apps - Detailed Account Setup**.

    ![Booster](installapps2.png)

    Click **Start**.

    ![Start booster](installapps3.png)

3. After the system checks prerequisites, click **Next**.

    ![Check prerequisites](installapps4.png)

    Choose **Select Subaccount**, and click **Next**.

    ![Select subaccount](installapps5.png)

    >Your trial account can have only one subaccount, so you must use the existing one.

    On the **Configure Subaccount** page, just click **Next**.

    ![Next](installapps6.png)
    
    On the **Add Users** page, just click **Next**.

    ![alt text](installapps7.png)

    On the **Review** page, click **Finish**.

    ![Finish](installapps8.png)

    The wizard will start to install all the needed components.

    ![Installing](installapps9.png)

4. When the installation is complete, you will see the following. 
    
    ![Finished](installapps10.png)
    
    Click **Navigate to Subaccount**, and then click **Instances and Subscriptions** to see that SAP Build Apps was installed.

    ![Installed](after1.png)

5. But SAP Build Apps must run on a custom identity provider – the part of SAP BTP that defines all the users. And though the custom identity provider and your user in it are automatically created for you, you must create a password for your user.

    An email has already been sent to the email you used to create your SAP BTP trial account.

    ![Email](after2.png)

    Click the blue button **Click here to activate your account**.

5. Enter a password (twice), and click **Continue**.

    ![Continue](after3.png)

    Your account in the new custom identity provider is activated. 

    The admin screen for the custom identity provider opens. You will not need this and you can close the window.

If you want to test, go back to the **Instances and Subscriptions** screen (it should be open on your previous tab).

Click the icon next to SAP Build Apps. The SAP Build lobby will open up, empty. This indicates you are logged in with your custom identity provider user and all is working OK.

![Working](after4.png)












### Install SAP Build Process Automation
Here is video from **Daniel Wroblewski** showing how to install SAP Build Process Automation.

>STOP the video at **1:10 mins**. You do **NOT** have to install the Desktop Automation agent.

<iframe width="560" height="315" src="https://www.youtube.com/embed/2gB7ipo8TNY" frameborder="0" allowfullscreen></iframe> 

If you prefer, here are the step-by-step instructions.

1. In the **Instances and Subscriptions**, click **Create**. 

    ![Create](spa1.png)

2. Set the following fields:

    | Field | Value|
    |-------|------|
    |  Service      | **SAP Build Process Automation**     |
    | Plan        |  **Standard**     |
    | Instance Name       | `spa-service`     |

    Click **Create**.

    ![Service](spa2.png)

    The wizard will run and you will see **Creation in Progress** (the service appears under **Instances**).

    ![Creating](spa3.png)

    After a few minutes, it will turn green to **Created**.
    
    ![Created](spa4.png)

3. In the same **Instances and Subscriptions**, click **Create** again. 

4. Enter the following:

    | Field | Value|
    |-------|------|
    |  Service      | **SAP Build Process Automation**     |
    |  Plan        |  **Free**     |

    Click **Create**.

    ![Free service](spa5.png)

    The wizard will run and you will see **Processing**. 

    ![Processing](spa6.png)

    >**IMPORTANT:** It may take as long as a half-hour to complete the subscription to SAP Build Process Automation. If you are doing these prerequisites before the CodeJam, just continue.
    
    >But if you are doing these at the event (which we do NOT recommend), you do not have to wait and can skip the rest of step 3 for now (and move to Step 4) as you won't be using SAP Build Process Automation until much later in [tutorial 4](codejam-04-spa-empty-process) in this tutorial mission.
    
    >**But when the subscription is complete,** and you reach the point that you must use SAP Build Process Automation, remember to return to this step and finish it (we will remind you in Tutorial 4). 

5. When the subscription for SAP Build Process Automation is complete, the status will turn green to **Subscribed**.

    ![Subscribed](spa7.png)
   
    On the left-side menu, go to **Security > Users**.
   
    Select your user for the custom identity provider.

    >That's the user with your email and whose **Identity Provider** field says **Custom IAS tenant**.

    ![User](spa8.png)

6. Once your user is selected, click the 3 dots under **Role Collections**.

    Select **Assign Role Collections**.

    ![Role collection](spa9.png)

    In the dialog, enter `process` in the search box, select all the roles, and click **Assign Role Collection**.

    ![Assign](spa10.png)

If you want to test, go back to the **Instances and Subscriptions** screen. Click the icon next to SAP Build Process Automation, which will open the SAP Build lobby.

Click on **Create** and try to create **Build an Automated Process > Business Process** project – if you can save the new project, this indicates you are logged in with your custom identity provider user and all is working OK. You can delete the test project.

![Working](after4.png)





### Create a destination for our CAP service
We will be using a CAP service created for you to maintain your shopping cart ... we will talk more about that later. 

In order to work with the CAP service, you need to create a destination in the SAP BTP cockpit, which you can then access from your SAP Build Apps project.

1. Download the destination definition.
   
    Click [CodeJamOrdersService](https://github.com/sap-tutorials/sap-build-apps/blob/main/tutorials/codejam-03-cart-page/CodeJamOrdersService), and then click the download button.

    ![Download](downloadCAP.png)

2. In the SAP BTP cockpit, click **Connectivity >  Destinations**.

    <!-- border -->
    ![Open destinations](images/3-open-destinations.png)

3. Click **Import Destination**, and then select the `CodeJamOrdersService` file you downloaded.

    The draft destination will be filled in.

    <!-- border -->
    ![Skeleton destination](images/destination.png)

    Click **Save**.

4. Test the connectivity to the new destination by clicking **Check Connection**.
   
    ![Test destination](images/test-dest.png)

    You should receive **200: OK**. 











### Further study
Congratulations, you are now ready for our **SAP Build CodeJam**. Have a safe journey and the SAP Advocates Team looks forward to seeing you on the day of your event.

In the meantime, here are some additional resources that provides more information about SAP Build low-code tooling.

- [Fun Overview Video of SAP Build](https://www.sap.com/assetdetail/2022/11/18c567fd-4d7e-0010-bca6-c68f7e60039b.html)
  
- [SAP Build YouTube Playlist from Developer Advocates](https://www.youtube.com/playlist?list=PL6RpkC85SLQA-fRzv03OPgWLFAj0v6AVn)

- [Overview Video of SAP Build Apps](https://youtu.be/alwEHrzNRRU)

- [Overview Video of SAP Build Process Automation](https://www.youtube.com/watch?v=S-cEA8EVCfU)

- [Overview Video of SAP Build Work Zone](https://www.youtube.com/watch?v=UJpaF4DcHWc)


>**Things to Ponder**
>
>Who do you think the audience is for SAP Build low-code tools?
>
>What type of scenarios do you think SAP Build low-code tools are good for?
>
>Do you know the concept of Fusion Development? 
>
>Why do we have to create a new user for SAP Build Apps, when we have an existing user?

