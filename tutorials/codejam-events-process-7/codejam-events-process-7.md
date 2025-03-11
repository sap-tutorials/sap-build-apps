---
parser: v2
author_name: Daniel Wroblewski
author_profile: https://github.com/thecodester
auto_validation: true
time: 20
tags: [ tutorial>intermediate, software-product>sap-build, software-product:technology-platform/sap-business-technology-platform/sap-integration-suite, software-product>sap-build-process-automation]
primary_tag: software-product>sap-build
---

# 7 - Add Approval Step and Use Inbox

<!-- description --> Add a step to allow an approver to see the details sent with the event and approve the request for a badge. 

## Prerequisites
- You have completed the previous tutorial for the event-driven processes CodeJam, [Capture Event in SAP Build Process Automation](codejam-events-process-6).

## You will learn
- How to add an approval step
- How to bind values to step inputs
- How to work with the Inbox

## Intro
One of the most basic steps you can create is an approval step, which sends an approval form to the recipients Inbox (in SAP BTP) and lets them approve or reject a request.

In this tutorial you will create an approval form and assign it to a recipient (you). You will also be introduced to the Inbox, where such forms are sent for execution by the assigned parties. The Inbox has many featured to enhance the approval flow, such as:

- Warnings when the due date approaches or passes

- Hides completed tasks

- Ability to send to multiple parties and have one claim the task

- Ability to approve / reject tasks via API





### Add approval form
1. Go back to your process project, and make sure you are working in the **Editable** version by clicking the dropdown at the top of the page and selecting **Editable**.
   
    ![Editable version](assets/approval1.png)

2. With the **Badge Process** process open, click the plus sign, **+**, between the trigger and the end.

    ![New step](assets/approval2.png)

    Select **Approval**.

    ![Approval step](assets/approval3.png)

    Select **Blank Approval**.

    ![Blank approval form](assets/approval4.png)

3. Enter the following (the identifier is automatically generated):

    | Field | Value |
    |-------|--------|
    | **Name** | Badge Approval | 
    | **Description** | This is the form in which the main approver will indicate approval or rejection of the badge. |  

    Click **Create**.

    ![Approval form name](assets/approval5.png)

    You will see the form add to the process, with different paths for approval and rejection.

There will be an red error, but this is expected and you did nothing wrong. The error is just an indicator that you need to add some settings, like to whom to send the form. 

![Approval form added](assets/approval6.png)




### Design approval form
1. Click the 3 dots on the side of the approval form, and select **Open Editor**.

    ![Open editor](assets/configure1.png)

2. Add the following fields, just by clicking the corresponding square on the left:

    - Headline 1
    - Text
    - Text
    - Text
    - Text

    ![Add fields](assets/configure2.png)

3. On the form, click each field and set its label to the following:

    | Field | Value |
    |-------|--------|
    | **Headline 1** | Badge Request | 
    | **Text** | First Name | 
    | **Text** | Last Name | 
    | **Text** | cc | 
    | **Text** | Community ID |     

    ![Name fields](assets/configure3.png)

4. On the form, select each **Text** field and set it to **Read Only**.

    ![Make fields read only](assets/configure4.png)

5. Click **Save** (upper right).

>You can configure the buttons for forms. You can have up to 10 buttons, and they can be named whatever you want.

>In addition, for each button, your process will have a different output. So an approval can have up to 10 different paths, not just two.


### Bind approval form fields
For each step in a process, you must bind its properties and inputs to the appropriate values. For example, for a form you must say to whom to send the form.

The binding step enables you to reuse steps, for example, to send the same approval form at different points in your process to different stakeholders.

1. Back in the process, select the **Badge Approval** form and make sure the process panel is open.

    >The side panel is where you set the bindings for the selected step, or for the entire process if no step is selected. There is an icon to open and close the side panel.

    ![Open side panel](assets/bind1.png)

2. For the **Subject** field, enter the following (including trailing space):

    ```Text
    Badge Request - 
    ```

    ![Add text to subject](assets/bind2.png)

    Click at the end of the field twice. Then, with the cursor still at the end of the field, select **Process Inputs > data > BusinessPartner**.

    ![Add field to subject](assets/bind3.png)

    The field should now look like this:

    ![Subject field bound](assets/bind4.png)

3. Under **Recipients**, click inside the **Users** field, and enter the email address you used to create your SAP BTP trial.

    ![Recipients](assets/bind5.png)

4. Under the **Inputs** tab, bind the following fields:

    | Field | Value |
    |-------|--------|
    | **BP ID** | Process Inputs > data > BusinessPartner | 
    | **Community ID** | Process Inputs > data > YY1_SAPCommunityUsername | 
    | **First Name** | Process Inputs > data > FirstName | 
    | **Last Name** | Process Inputs > data > LastName | 

    ![Bind inputs](assets/bind6.png)

5. Click **Save** (upper right).






### Add notification form
1. Click the plus sign, **+**, under **Approve**.

    ![Add notification form](assets/notification1.png)

2. Select **Form**.

    ![Select form](assets/notification2.png)

    Select **Blank Form**.

3. Enter the following (the identifier is automatically generated):

    | Field | Value |
    |-------|--------|
    | **Name** | Notification | 
    | **Description** | This form notifies the approver that the badge is ready and to indicate if it has been picked up. |  

    Click **Create**.

    ![Select form](assets/notification3.png)

4. Open up the editor.

    ![Open editor](assets/notification4.png)

    Just click **H1** to add a **Headline 1** field, and type into the field **Badge Ready for Pickup**.

    ![Design form](assets/notification5.png)

5. Click **Save**.



### Bind notification form fields
1. Back in the process, select the **Notification** form and make sure the process panel is open.

    ![Open side panel](assets/bindform1.png)

2. For the **Subject** field, enter the following (including trailing space):

    ```Text
    Badge Request Approved - 
    ```

    ![Add text to subject](assets/bindform2.png)

    Click at the end of the field twice. Then, with the cursor still at the end of the field, select **Process Inputs > data > BusinessPartner**.

    ![Add field to subject](assets/bindform3.png)

    The field should now look like this:

    ![Subject field bound](assets/bindform4.png)

3. Under **Recipients**, click inside the **Users** field, and enter the email address you used to create your SAP BTP trial.

    ![Recipients](assets/bind5.png)

4. Click **Save**.






### Release and deploy process
1. Click **Release**.

    ![Release](assets/release1.png)

    This time, since it's not the first release, you can specify what type of change you made, as well as provide release notes.
    
    ![Click release](assets/release2.png)

    Keep all the settings the same.

    Click **Release**.

    >Note that after releasing, you are still inside the editable project, not the released version.

2. Click **Show project version** (upper left).

    ![Released version](assets/release3.png)

    This will take you to the released version of your project, so you can deploy it.

    >You can navigate between the versions of your project at the very top using the dropdown (assuming there is more than one version).

3. Click **Deploy**.
   
    ![Deploy](assets/release4.png)

    Select the **Public** environment.

    Select **Upgrade** – as you already have a version deployed.

    ![Deploy](assets/release5.png)

   You get a message that your deployment will update the existing trigger. 

   Click **Deploy**.  




### 🥳 Trigger process
This step is an exciting moment. Here you will trigger your process from your real event (not manually like you did before). This is the crux of the entire CodeJam – to be able to trigger processes from S/4HANA or other SAP events.

1. Go to the **Create Business Partner** app we provided to you.

    ![Business partner app](assets/BP1.png)

2. Enter the following:

    | Field | Value |
    |-------|--------|
    | **First Name** | Anything you want | 
    | **Last Name** | Anything you want | 
    | **Country** | Select one of the countries |      
    | **SAP Community Username** | Your user name in the SAP Community |      

    >**IMPORTANT:** You set up your REST Delivery Point to publish only events that contain your community user. So you need to provide your user in order for the event to be delivered to your SAP BTP tenant and to trigger your process.

    Click **Create**.

    ![Add business partner](assets/BP2.png)

    Your business partner is created.

    ![Business partner created](assets/BP3.png)

3. Check that the event was received into SAP Build by going to **Monitoring**.

    Click **Acquired Events**.

    ![Acquired events](assets/BP4.png)

    Click **Business Events**. You should see an event created for your new business partner, including the business partner ID that you saw when you created it.

    ![Acquired events](assets/BP5.png)

    😺🙃





### Check your Inbox
Now that we know the event arrived into SAP Build Process Automation, let's check if our process was triggered and the approval form was created.

1. From the SAP Build header, click the Inbox icon.

    ![Open Inbox](assets/inbox1.png)

    This opens the Inbox, and you will see the approval form, with all the data from our simulated event that we pasted in. Notice that the form has the ID of the business partner you created. 

    ![Approval form](assets/inbox2.png)

2. Before acting on the form, go back to the browser tab with SAP Build's main page.

    Go to **Monitoring**.
    
    Go to **Process and Workflow Instances**.

    ![Monitoring](assets/inbox3.png)

    At the same time, from the SAP Build main page, go to **Monitoring > Processes and Workflow Instances**.

    This time, you do not have to change the filter because the process instance has not completed and instead stopped for the response to the approval form.

    ![Triggered process](assets/inbox4.png)

3. Click on **Badge Process**, and you will see the header info, logs, and context for the process instance you just started.

    In the logs, notice **Task "Badge Request"** is available – this is the approval form waiting for your action.

    ![Process instance info](assets/inbox5.png)

    Expand the **Task "Badge Request"**. You'll see the recipients (you) as well as an ID for that specific task instance (there are APIs for manipulating that specific task, which use this ID).

    ![Task info](assets/inbox6.png)

4. Go back to the Inbox and approve the badge by clicking **Approve** on the bottom right.

    ![Approve badge](assets/inbox7.png)

    The task in the Inbox disappears. But refresh the Inbox and the notification will be shown.

    ![Notification received](assets/inbox8.png)

5. Now go back to the **Monitoring** area and refresh the view of your process instance.
   
    You now see that you completed the approval step and now a second task is awaiting you, the notification.

    ![Approval completed](assets/inbox9.png)

You can go back to the Inbox and click **Submit** on the notification form, then return to the **Monitoring** area and see that the process instance completed.

![Process instance completed](assets/inbox10.png)
