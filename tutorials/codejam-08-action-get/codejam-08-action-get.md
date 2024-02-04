---
parser: v2
author_name: Rekha DR
author_profile: https://github.com/Rekha-DR
auto_validation: true
time: 40
tags: [ tutorial>beginner, software-product>sap-business-technology-platform,software-product>sap-build, software-product>sap-build-apps--enterprise-edition, software-product>sap-build-process-automation]
primary_tag: software-product>sap-build
---
  

# Create Action Project to Get Data from SAP S/4HANA Cloud
<!-- description --> Create an action project to retrieve SAP S/4HANA Cloud data and call the action from your business process, as part of the SAP Build CodeJam.



## Prerequisites
- You have completed the previous tutorial for the SAP Build CodeJam, [Trigger Process from Your App](codejam-07-connect-app-process).






## You will learn
- How to get access to and create a destination for SAP S/4HANA Cloud APIs on SAP Business Accelerator Hub
- How to create action project
- How to configure API method input and output fields to improve readability for business users
- How to test API using destination option
- How to release and publish action project to be consumed in the SAP Build process Automation


## Intro
Actions are SAP Build Process Automation artifacts that you can create to connect business processes with external systems, be it SAP or non-SAP. This is an important piece of the puzzle especially if you want to extend or automate any of your business processes existing on systems like S/4HANA, Ariba and SuccessFactors. These extensions can be easily built using SAP Build Process Automation. Using actions, GET, POST, PATCH and other calls can be made to external systems.

In this tutorial, you will create an action project based on the Business Partner API of SAP S/4HANA Cloud, using the demo service exposed on the SAP Business Accelerator Hub. Specifically, you will take a business partner ID entered by the user in your shopping app and retrieve information about the business partner.









### Create destination for SAP S/4HANA Cloud 
In order to access the demo SAP S/4HANA Cloud business partner API on the **SAP Business Accelerator Hub**, you need to create a destination.

1.  Go to the [SAP Business Accelerator Hub](https://api.sap.com/) and log-in (or Create a free account)

    On the top-right, click `API Key` to see your key. Copy it and save on the side.

2. Download the destination definition.

3. Open the cockpit on your SAP BTP trial account.

    Go to **Connectivity > Destinations**, and click **Import Destination**.

    Select the destination definition you downloaded.

    In the URL.headers.APIKey, enter the key you earlier retirved from the [SAP Business Accelerator Hub](https://api.sap.com/).

    Click **Save**.

If you click **Check Connection**, you will get a 401 response code. This is OK and you have **successfully** created the destination


>To learn more about the [SAP Business Accelerator Hub](https://api.sap.com/) and how to send up destinations for any API, watch this video from **Daniel Wroblewski**. 

><iframe width="560" height="315" src="https://www.youtube.com/embed/11TUQgQi-9k" frameborder="0" allowfullscreen></iframe>






### Enable destination in processes
SAP Build Process Automation enables administrators to control which destinations – and, therefore, which backend systems – can be connected to from processes. 

In this step, you will enable the destination to be used in your processes.

1. Go to the SAP Build main page.

2. Go to **Control Tower**, and then click **Backend Configuration > Destinations**.

    ![Add destination](add-dest1.png)

3. Click **New Destination**.

    ![New destination](add-dest2.png)

    Select your destination, **SHANA-Hub-Public**.

    Click **Add**.

    ![Add](add-dest3.png)







### Create action project
1. In the SAP Build lobby, click **Connectors > Actions**.

    ![Click Actions](1a_Select_Actions_from_Lobby.png) 

2. Click **Create**.

    ![Click Create](1b_Click_Create.png) 
                
3. Choose **OData Destinations** as the API source.

    ![Choose OData Destinations](1c_Choose_oData_Destinations.png) 

4. Choose **S4HANA-Hub-BP**.

    ![Choose S/4 HANA Hub](1d_Browse_Choose_S4HANA_Hub_Public.png) 

    You will see all the APIs contained in the business partner service, including those for the `A_BusinessPartner` entity.
    
    ![Business Partner Entity](1e_Check_for_Business_Partner_Entity.png) 

    Click **Next**.

5. Enter `Business Partner Service` for the **Project Name** and **Description**. 

    Click **Create**.

    ![Action Project Name](1f_Action_Project_Name.png) 

    You will now see all the APIs in the service, so that you can select the ones you want to include in this action project.

6. Expand the **A_BusinessPartner** entity.   

    ![Expand Business Partner Entity](1g_Expand_A_BusinessPartner.png) 

    Select the **GET** operation that is called **Retrieves business partner data by using business partner number**.  
   
    >Be careful! There are a lot of APIs that look the same.

    ![BP by Number](1h_GET_BP_by_Number.png) 

7. Click **Add**.

    The action project **Business Partner** opens under a new tab.

    ![Action Project Opens ](2a_Action_Project_Opens.png) 













### Configure Action project
You have now created an action project and selected the API to expose. Bu now you must configure – for example, what inputs to require, what outputs to return, and what OData parameters to include.

Your action should be open to the **Retrieves business partner data by using business partner number (GET)** API, and you should see the **Input** tab.

![Configure Business Partner by Number Input ](3a_BP_By_Number_Inputs.png)


1. Click the **Output** tab. 

    Under **Body**, select all the fields by selecting the checkbox next to **d**.
    
    Then expand the node and deselect only the needed fields highlighted shown in the screenshots below.
  
    ![Select all Output Fields ](3b_Configure_Service_Outputs.png) 

    ![Uncheck the output fields set 1 ](3c_Configure_Service_Outputs.png)

    ![Uncheck the output fields set 2 ](3d_Configure_Service_Outputs.png)  

2. Click **Remove** (back at the top of the list).

    ![Remove output Fields](3e_Remove_Output_Fields.png) 

    Confirm **Remove**

    ![Confirm Removal ](3f_Remove_Output_Fields.png) 

    Under **Body**, you will now see only the needed fields.

3. Click **Save**.

    ![Save output Fields](3g_Save_Output_Fields.png) 

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

    ![Release Action Project](4a_Release_Action_Project.png) 

    Click **Release** again, to confirm.

    ![Confirm Release ](4b_Confirm_Release.png) 

    The action project status is changed to **Released**. 

2. Click **Publish to Library**.

    ![Publish to Library](5a_Publish_to_Library.png)

    Click **Publish** to confirm. 

    ![Confirm Publish](5b_Confirm_Publish.png)  

    Now the action project status is changed to **Published**.

    ![Check Action Project Status](5c_Action_Project_Status_Check.png) 

   Back in the SAP Build main page under **Actions**, you can see your action, and its released version.

    ![Action Project under Actions List](6a_Action_Project_in_ActionsList.png) 






### Add action to process
Now you've created an action to retrieve data. Now lets ad it so we can retrieve data as part of our process.



1. Return to the SAP Build lobby, and select your **Purchase Approval**  process.

    ![Return to Lobby](7a_Return_to_Lobby.png) 

2. Under **Overview > Artifacts**, click **Purchase Approval Process**. 

    ![Click on Add](7b_Select_Approval_Process.png) 

    Click the plus sign, **+**, just below the API trigger block.

    ![Click Add Symbol](7d_Click_Add_After_Trigger.png) 

3. Click **Action**. 

    ![ Choose Action](7e_Choose_Action.png) 

    Choose the action created in the previous steps, and click **Add**.

    ![Browse Action Library using filters for Action Project](7f_Set_Filter_Add_Action.png)

    >If you had many actions, you could use the filters at the top.
    
    The action is added to the process flow.

    ![Action Added](7g_Action_Added.png) 

4. With the new action block selected, go to the side panel and configure the action.

    In the **General** tab, click the **Destination variable** field, click **Create Destination Variable**.

    ![Configure Destination Variable](7h_Action_Destination_Variable.png)

    Enter `BPdest` for the identifier, and click **Create**.

5. In the **Inputs** tab, click in the **BusinessPartner** field, and select **Process Inputs > Business Partner** fields to Action **Inputs**.

    ![Configure Action Inputs](7i_Action_Input_Config.png)

    **BusinessPartner** field is now mapped.

    ![Map Business Partner Input Field](7j_Action_Input.png)

6. In the **Outputs** tab, expand **result** to see all the fields that will be returned by the action and be available inside the process.

    ![Examine Action Outputs](7k_Examine_Action_Output.png)

7. Click the condition block, and from the side panel click **Open Condition Editor**.

    ![Configure Condition](8a_Condition_Configuration.png)

    Click **Add**.

    ![Add Condition](8b_Add_Condition.png).

    Set this new condition as follows:
    
    | Field | Value |
    |-------|-------|
    | 1st | **BusinessPartnerGrouping** | 
    | 2nd | **is equal to** |
    | 3rd | `BP01` |
    
    At the top, under **Satisfies**, change to **Any**.
    
    You can expand **Summary** to get a verbal summary of the conditions in plain English.

    ![Save and Release the Project](8d_Condition_Save_Release.png)


    Click **Apply**, and then click **Save** (upper right).

    > ### What's going on?
    >
    >Previously, anything less than 1000 was automatically approved. But now, if the related business partner is in group **BP01**, those requests are also automatically approved.
    
8. Click **Release**, and then confirm by clicking **Release** again

9. Click **Deploy**.

    ![Click Deploy](9a_Deploy.png)

    Click **Next**.

        ![Deploy step 1](9b_Deploy_Step1.png)

    - Select **Destination Variable** value.

        ![Deploy step 2a](9c_Deploy_Step2.png)

    - Click **Next**.

        ![Deploy step 2b](9d_Deploy_Step2.png)

    - Click **Deploy**.

        ![Deploy step 3](9e_Deploy_Step3.png)

    - Status of the Project changes to **Deployed**. Click on SAP Logo at the left upper corner to return to Build Lobby.

        ![Deployment Status](9f_Deployed_Status.png)









### Modify app to send business partner



1. Open **ShoppingApp** from SAP Build lobby.

    ![Open Shopping App](10a_Shopping_App.png)

2. Navigate to the cart page by clicking **Home page**.

    ![Open App Pages](10b_Navigate_to_Pages.png)

    Click the **Cart** page. 

    ![Open Cart Page](10c_Select_Cart.png)

3. Toggle to **Variables**.

    ![Switch from View to Variable](10d_Toggle_to_Variables.png)

    Choose **Page Variables** on the left, then click **Add Page Variable**.

    ![Select Page Variables](11a_Select_Page_Variables.png)

    Change the name of the variable to **BP**.
    
    Click **Save**.

    ![Add Page Variable](11b_Add_Page_Variable_Save.png)

4. Toggle back to **View**.

    ![Switch to App View ](12a_Switch_to_View.png)

    Click the business partner dropdown list, and bind **Selected value** to **Data and Variables > Page Variables > BP**.

   Click **Save** (upper right).

   >The skeleton project already had the business partner dropdown list to have 2 business partner IDs that were valid in the demo API, one with grouping BP01 and one with grouping BP02 – in order to test out the logic of our process. 

6.  Click the **Purchase** button and open its logic canvas.

    ![Click on Purchase Button](13a_Select_purchase_Button.png)

    Click on the **Create record** flow function, and then on the right, click **Custom object** under **Record**.

    >This is the flow function that triggers your process.

    ![Open Add Logic Panel](13b_Open_Add_Logic_Panel.png)

    Click on the binding icon for the **BusinessPartner** field.

    ![Click on BusinessPartner](13c_Select_BP_Field.png)

    Bind the field to **Data and Page Variables > Page Variable > BP**.

    Click **Save**.

    Make sure the Variable is bound.

    ![Page Variable Binding ](13g_BP_Variable_Bound.png)

    **Save** the binding.

    ![ Save Binding ](13h_Save_Binding.png)

    **Save** the changes to the app (upper right).

    ![Save Changes to the App](13i_Save_Changes_to_App.png)








### Test process
1. Launch the app. 

    - Click **Launch** tab.

        ![Launch the App](14a_Launch_App.png)

    - Click **Open preview portal**.
         
        ![Open Preview Portal](14b_Open_Preview_Portal.png)

    - Click **Open web preview**.

        ![Open Web Preview](14c_Open_web_Preview.png)

    - Click **Open** on the **ShoppingApp**.

        ![Open App](14d_Open_App.png)
     


2. Select a product.
      
    ![Select a Product](15a_Select_Product.png)

    Change the quantity and click **Add to the Cart**.

    ![Add to Cart](15b_Add_to_Cart.png)

    
3. Click **Cart** in the left navigation menu.

    ![Cart Page](16a_Cart_Page.png)

    Select **11** for the business partner.

    ![Select Business Partner](16b_Choose_BP.png)
    
    Click **Purchase**.

    ![Click Purchase](16c_Click_Purchase.png)

    You should get a purchase confirmation message.

    ![Purchase Request Created Message](16d_Purchase_Order_Requested.png)


4. In the SAP Build main page, navigate to the **Monitoring** tab.

    ![Click on Monitoring](17a_Navigate_to_Monitoring.png) 

    Under **Monitor**, click **Process and Workflow Instances**.

    ![Choose Process](17b_Monitor_Process.png)

    Select the process you just triggered.

    ![Open the Purchase Approval](17c_Select_Process.png)

    Examine the **Log**, and you can see that the **Retrieves business partner data by using business partner number** action was executed successfully.

    ![Study and Understand the Process Log](17d_Process_Flow_Log.png)

5. Open your **Inbox** by clicking he icon in the header.

    ![Open Inbox](17e_Process_Inbox.png)

    You should see an auto-approve item in the Inbox. Open it, and click **Submit** at the bottom of he page.

    ![Submit the Auto Approval Notification](17f_Approval_Notification.png)
    
    If you go back to the monitoring tab, you will see that the process completed.

    ![Process Completed](17g_Process_Complete.png)

