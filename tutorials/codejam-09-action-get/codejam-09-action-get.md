---
parser: v2
author_name: Rekha DR
author_profile: https://github.com/Rekha-DR
auto_validation: true
time: 30
tags: [ tutorial>beginner, software-product>sap-build, software-product>sap-build-apps, software-product>sap-build-process-automation]
primary_tag: software-product>sap-build
---
   

# 9 - Create Action to Get Data from SAP S/4HANA Cloud
<!-- description --> Create an action project to retrieve SAP S/4HANA Cloud data and call the action from your business process, as part of the SAP Build CodeJam.



## Prerequisites
- You have completed the previous tutorial for the SAP Build CodeJam, [Trigger Process from Your App](codejam-08-work-zone-app).






## You will learn
- How to create destination to SAP S/4HANA Cloud demo APIs on SAP Business Accelerator Hub
- How to create and publish an action project
- How to consume an action from a process


## Intro
Actions are SAP Build artifacts that you can create to connect processes and apps to external systems, be it SAP or non-SAP. This is an important piece of the puzzle especially if you want to extend or automate any of your business processes existing on systems like SAP S/4HANA, SAP Ariba and SAP SuccessFactors.

In this tutorial, you will create an action project based on the Business Partner API of SAP S/4HANA Cloud, using the demo service exposed on the SAP Business Accelerator Hub. Specifically, you will:

- Set up a destination to the API.
- Send a business partner ID from your app to your process.
- Retrieve data about that business partner from inside the process.
- Make decisions in the process based on the retrieved data.







 

### Create destination for SAP S/4HANA Cloud 
In order to access the demo SAP S/4HANA Cloud business partner API on the **SAP Business Accelerator Hub**, you need to create a destination.

1.  Go to the [SAP Business Accelerator Hub](https://api.sap.com/) and log in (or create a free account)

    ![SAP Business Accelerator Hub](businesshub.png)

    On the top-right, click your avatar, and then click `Settings`.
    
    ![Settings](hub-key1.png)
    
    Click `Show API Key`.

    ![Show API Key](hub-key2.png)

    Save the key for later.

    ![Save API Key](hub-key3.png)

    >The last time we tested, the **Copy Key and Close** button did not work, so you may have to select the key and copy it manually.

2. Download the destination definition.
   
    Click [S4HANA-Hub-Public](https://github.com/sap-tutorials/sap-build-apps/blob/main/tutorials/codejam-09-action-get/S4HANA-Hub-Public), and then click the download button.

    ![Download](Download.png)

2. In the SAP BTP cockpit, click **Connectivity >  Destinations**.

    <!-- border -->
    ![Open destinations](3-open-destinations.png)

3. Click **Import Destination**, and then select the `S4HANA-Hub-Public` file you downloaded.

    <!-- border -->
    ![New destination](4-create-destination.png)

    The draft destination will be filled in except for your API key.

    ![Add destination](add-destination.png)

    Enter the key (you saved earlier from the SAP Business Accelerator Hub).

    Click **Save**.

If you click **Check Connection**, you will get a 401 response code. This is OK, and you have **successfully** created the destination.

>To learn more about the [SAP Business Accelerator Hub](https://api.sap.com/) and how to set up destinations for any API, watch this video from **Daniel Wroblewski**. 

><iframe width="560" height="315" src="https://www.youtube.com/embed/11TUQgQi-9k" frameborder="0" allowfullscreen></iframe>









### Enable destination in processes
SAP Build Process Automation enables administrators to control which destinations – and, therefore, which backend systems – can be connected to processes during runtime. 

In this step, you will enable the destination to be used in your processes.

1. Go to the SAP Build main page.

2. Go to **Control Tower**, and then click **Backend Configuration > Destinations**.

    ![Add destination](add-dest1.png)

3. Click **Add**.

    ![New destination](add-dest2.png)

    Select your destination, **SHANA-Hub-Public**.

    Click **Next**.

    ![Add](add-dest3.png)

    On the second screen, keep the settings to allow the destination in **All Environments**.

    Click **Add Destination**.

    ![Add](add-dest4.png)
    
    The destination will be added to the list of destinations that can be used within your processes.

    ![Added](add-dest5.png)







### Create action project
1. In the SAP Build lobby, click **Connectors > Actions**.

    ![Click Actions](1a_Select_Actions_from_Lobby.png) 

    Click **Create**.

    ![Click Create](1b_Click_Create.png) 
                
2. Choose **OData Destinations** as the API source.

    ![Choose Other BTP Destinations](1c_Choose_Other_BTP_Dest.png) 

3. Choose **S4HANA-Hub-Public**.
    
    ![Choose S4HANA Destination](1d_Browse_Choose_S4HANA_Dest.png) 

    >**Why do I see the CAP service?**
    >
    >You might be wondering why you see the CAP service here, when you only added the SAP S/4HANA service in the Control Tower.
    >
    >In the Control Tower, you added the destinations that could be used during runtime – and the `sap.processautomation.enabled` additional property in the destination enables the administrator to select it for runtime.
    
    >But destinations have a second property that indicates whether they should be shown in the actions setup screens, `sap.applicationdevelopment.actions.enabled`. When you set up your destinations, you added this to both the CAP and SAP S/4HANA destinations.

    You will now see all the APIs contained in the Business Partner service, including those for the `A_BusinessPartner` entity. Expand **A_BusinessPartner** to see the specific APIs available.

    ![Choose S/4 HANA Hub](1d_Browse_Choose_S4HANA_Hub_Public.png) 

    >This screen is just to show you the APIs available – you do not have to select anything.

    Click **Next**.

4. Enter:

    - `Business Partner ` for the **Name**

    - `API to read Business Partner from SAP S/4HANA Cloud` for the **Description**

    Click **Create**.

    ![Action Project Name](1f_Action_Project_Name.png) 

    After about a minute, the action project will be created and open.
    
    You will now see all the APIs in the service, so that you can select the ones you want to include in this action project.

5. Expand the **A_BusinessPartner** entity.   

    >It might help to select the **GET** filter and search by `retrieves business partner data`.

    >![Search entity](search.png)

    Select the **GET** operation that is called **/A_BusinessPartner('{BusinessPartner}')** with the description **Retrieves business partner data by using business partner number** (see screenshot above).  
   
    >Be careful! There are a lot of APIs that look the same. Use the search to help make finding the right one easier.

6. Click **Add**.

    The action project **Business Partner** opens under a new tab.

    ![Action Project Opens ](2a_Action_Project_Opens.png) 

    >The action will open with the selected APIs. You can always go back and add additional APIs to expose.












### Configure action project
You have now created an action project and selected the API to expose. But now you must configure – for example, what inputs to require, what outputs to return, and what OData parameters to include.

Your action should be open to the **Retrieves business partner data by using business partner number (GET)** API, and you should see the **Input** tab.

![Configure Business Partner by Number Input ](3a_BP_By_Number_Inputs.png)


1. Click the **Output** tab. 

    Under **Body**, select all the fields by selecting the checkbox next to **d**.
    
    Then expand the node and deselect only the needed fields highlighted in the screenshots below.
  
    ![Select all Output Fields ](3b_Configure_Service_Outputs.png) 

    ![Uncheck the output fields set 1 ](3c_Configure_Service_Outputs.png)

    ![Uncheck the output fields set 2 ](3d_Configure_Service_Outputs.png)  

2. Scroll back up, and click **Remove**.

    ![Remove](3e_Click_Remove.png)

    Confirm **Remove**.

    ![Confirm Removal ](3f_Remove_Output_Fields.png) 

    Under **Body**, you will now see only the needed fields.

    ![Save output fields](3g_Save_Output_Fields.png) 

3. Click **Save**.

4. Click the **Test** tab.

    Select your **Destination** from the dropdown value, **S4HANA-Hub-Public**.
    
    Under **Test Input Values**, set **BusinessPartner** to `11`.
    
    Click **Test**.

    ![Test GET Business Partner by Number Service](3h_Test_BP_Service_With_Number.png) 

    You should get a **200:&nbsp;OK** response, and see the data from the demo API.

    ![Examine the Service Response](3i_Examine_Service_Response.png) 









### Release and publish action
To make the action available to your processes, you must release and publish the action.


1. Click **Release**.

    >If **Release** is disabled, make sure you've saved the project.

    ![Release Action Project](4a_Release_Action_Project.png) 

    Click **Release** again, to confirm.

    ![Confirm Release ](4b_Confirm_Release.png) 

    The action project status is changed to **Released**. 

2. Click **Publish**.

    ![Publish to Library](5a_Publish_to_Library.png)

    Click **Publish** to confirm. 

    ![Confirm Publish](5b_Confirm_Publish.png)  

    Now the action project status is changed to **Published**.

    ![Check Action Project Status](5c_Action_Project_Status_Check.png) 

    Go back to the SAP Build main page, refresh the page, and under **Actions** you can see your action and its released version.

    ![Action Project under Actions List](6a_Action_Project_in_ActionsList.png) 

    >If it's not updated, you can refresh the page or refresh the list with the refresh icon on the top right.







### Add action to process
You've created an action to retrieve data. Now add it to the process so you can retrieve data inside the process.


1. Return to the SAP Build lobby, and open your **Purchase Approval** project.

    ![Return to Lobby](7a_Return_to_Lobby.png) 

2. Select the **Editable** version at the top, and then under **Overview > Artifacts**, click **Purchase Approval Process**. 

    ![Click on Add](7b_Select_Approval_Process.png) 

    Click the plus sign, **+**, just below the API trigger block.

    ![Click Add Symbol](7d_Click_Add_After_Trigger.png) 

3. Click **Action**. 

    ![Choose Action](7e_Choose_Action.png) 

    Choose **Browse All Actions**.
    
    ![Browse Library](7e2_Browse_Library.png) 
    
    Choose your action created in the previous steps. You may want to filter by your action project -- if necessary, click **Show Filter**.
    
    ![Browse Action Library using filters for Action Project](7f_Set_Filter_Add_Action.png)

    Click **Add**.
    
    The action is added to the process flow.

    ![Action Added](7fa-action-added.png)

4. With the new action block selected, go to the side panel and configure the action.

    In the **General** tab, click the **Destination Variable** field, click **Create Destination Variable**.

    ![Configure action](7h_Action_added.png)

    Enter `Business_Partner_Destination` for the **Identifier**, and click **Create**.

    ![Configure Destination Variable](7h_Action_Destination_Variable.png)

    >The destination variable creates an environmental variable for the destination. That is, when the process is deployed, you can then select which destination – meaning which backend system – to use in that environment.
    >
    >You can change the destination at deployment without having to change the process itself.

5. In the **Inputs** tab, click in the **BusinessPartner** field, and select **Process Inputs > Business Partner** field.

    ![Configure Action Inputs](7i_Action_Input_Config.png)

    **BusinessPartner** field is now mapped.

    ![Map Business Partner Input Field](7j_Action_Input.png)

6. In the **Outputs** tab, expand **result** to see all the fields that will be returned by the action and be available inside the process.

    Notice it only includes the fields you selected when defining the action.

    ![Examine Action Outputs](7k_Examine_Action_Output.png)

7. Click the condition block, and from the side panel click the 3 dots and then **Open Condition Editor**.

    ![Configure Condition](8a_Condition_Configuration.png)

    Click **Add**.

    ![Add Condition](8b_Add_Condition.png)

    Set this new condition as follows:
    
    | Field | Value | How to Enter |
    |-------|-------|-----|
    | 1st | **BusinessPartnerGrouping** | Select from action fields | 
    | 2nd | **is equal to** | Select from comparison options |
    | 3rd | `BP01` | Type in |
    
    At the top, under **Satisfies**, change to **Any**.
    
    You can expand **Summary** to get a verbal summary of the conditions in plain English.

    ![Save and Release the Project](8d_Condition_Save.png)


    Click **Apply**, and then click **Save** (upper right).

    > **What's going on?**
    >
    >Previously, anything less than 1000 was automatically approved. But now, if the related business partner is in group **BP01**, those requests are also automatically approved.

8. Click anywhere on **Approval Form** block.

    In the side panel under **Inputs**, you will now bind the business partner fields because you now have data from the backend.

    Bind the following fields:

    | Field | Action Field |
    |-------|-------|
    | BP Grouping | **BusinessPartnerGrouping** | 
    | Business Partner  | **BusinessPartner** | 
    | Business Partner Full Name | **BusinessPartnerFullName** | 

    ![Configure approval inputs](9a_Approval_Input_Config.png)
    
8. You may see an error next to your script task.

    If you do:
   
    * Open the editor for the **Script Task**.

    * Click **Apply**.

    ![Script error](scripterror.png)

1.  Click **Save** (upper right).

2.  Click **Release**.

    ![New version](release-confirm.png)
   
    Confirm by clicking **Release** again.

3.  Click **Show Project Version**.

    ![Show version](9a_show_version.png)

    Click **Deploy**.

    Select the **Public** environment, and click **Upgrade**.

    ![Click Deploy](9a_Click_Deploy.png)

    The triggers that will be deployed will be displayed. There is nothing to do here but confirm.
    
    Click **Deploy**.
    
    ![alt text](deploy-triggers.png)

    Now you will see the environment variables for which you need to provide values. 

    ![Destinations](deploy-destinations.png)
    
    >Previously, you did not get this screen because you did not have any variables to configure. But now you defined a variable for managing the destination, so you must select the destination for this variable for this deployment.

    Select the destination you created for accessing SAP S/4HANA.

    ![Choose destination](deploy-destinations2.png)
    
    Click **Deploy**.

    The status of the project changes to **Deployed**. 

    ![Deployment Status](9f_Deployed_Status.png)

4.  Go back to the SAP Build lobby, refresh the page.

    Next to your process project, click the row of the project, or click the chevron.
    
    ![Republish](republish0.png)

    Click the **Versions** tab, and hen click the 3 dots next to the latest deployed version.
    
    ![Republish versions](republish0a.png)

    Click **Publish to Library**.

    ![Republish to library](republish0b.png)

    In the dialog, click **Publish**.













### Modify app to send business partner
Now that the process expects a business partner, we must change the app to provide one.

1. Open the **ShoppingApp** project from SAP Build lobby.

    ![Open Shopping App](10a_Shopping_App.png)

2. Navigate to the cart page.

    ![Open Cart Page](10c_Select_Cart.png)

3. Click **Variables**.

    Choose **Page Variables** on the left, then click **Add Page Variable > From Scratch**.

    ![Select Page Variables](11a_Select_Page_Variables.png)

    Change the name of the variable to **BP**.
    
    Click **Save**.

    ![Add Page Variable](11b_Add_Page_Variable_Save.png)

    >Since you need to get a business partner from the user, and then pass it to process, you will need a place to store it.

4. Click **User Interface**.

    Click the business partner dropdown list, and bind **Selected value** to **Data and Variables > Page Variables > BP**.

    ![Dropdown binding](dropdown-binding.png)

    Click **Save** (upper right).

    >The skeleton project already had the business partner dropdown list to have 2 business partner IDs that were valid in the demo API, one with grouping BP01 and one with grouping BP02 – in order to test out the logic of our process. 

6.  Click the **Purchase** button and open its logic canvas.

    ![Click on Purchase Button](13a_Select_Purchase_Button.png)

    Click the **Trigger process** flow function.
    
    ![Trigger process flow function](13a_trigger_process.png)
   
    On the right, under **Input Parameters**, click **Custom object**. 
    
    In the dialog, bind **Business Partner** to **Data and Page Variables > Page Variable > BP**.

    Click **Save**.

    ![Bind to BP](13b-bind-BP.png)

    Click **Save** (upper right).




### Test app
1. Relaunch the app. 

2. Select a product.
      
    ![Choose product](15a_Choose_Product.png)

    Change the quantity to `5` and click **Add to Cart**.

    ![Add to Cart](15b_Add_to_Cart.png)

3. Click **Cart** in the left navigation menu.

    Select **1000000** for the business partner.

    ![Select Business Partner](16b_Choose_BP.png)
    
    Click **Purchase**.

    ![Click Purchase](16c_Click_Purchase.png)

    You should get a purchase confirmation message – it may take a minute.
    
    ![Purchase Request Created Message](16d_Purchase_Requested.png)

4. In the SAP Build Lobby, navigate to the **Monitoring** tab.

    ![Click on Monitoring](17a-redo.png)

    Under **Monitor**, click **Process and Workflow Instances**.

    ![Choose Process](17b_Monitor_Process.png)

    Select the process you just triggered.

    ![Open the Purchase Approval](17c_Select_Purchase_Process.png)

    Examine the **Log**, and you can see that the **Retrieves business partner data by using business partner number** action was executed successfully.

    ![Study and Understand the Process Log](17d_Process_Log.png)

    In the process **Context** you can see that data from **S4HANA** is fetched successfully.

    ![Choose Process](17e_Process_Context.png)

    You can also see that the last step is an approval form, waiting to be filled out -- the request is over 1000 and is not from someone in grouping BP01.

5. Open your **Inbox** by clicking he icon in the header.

    You should see an **Approve Form** task in the Inbox
     
    Open it, and you should now see that the business partner fields in the form are filled in with the backend data.

    ![Submit the Auto Approval Notification](17f_Approval_Notification.png)

    Click **Approve** at the bottom of the page.
    
    Refresh your Inbox, and now you will see the approval notification form. 

    Click **Submit**.

    ![Approved Notification in the Inbox](17h_Approved_Notification.png)

6. Go back to the monitoring tab and click **Refresh**. 

    The **Purchase Approval Process** flow is now **Completed**.

    ![Process Completed](17i_Process_Completed.png)

You can test again with the other business partner to see if your purchase gets automatically approved.