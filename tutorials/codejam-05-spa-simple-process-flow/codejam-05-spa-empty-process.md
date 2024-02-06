---
parser: v2
author_name: Rekha DR
author_profile: https://github.com/Rekha-DR
auto_validation: true
time: 50
tags: [ tutorial>beginner, software-product>sap-business-technology-platform,software-product>sap-build, software-product>sap-build-apps--enterprise-edition, software-product>sap-build-process-automation]
primary_tag: software-product>sap-build-process-automation
---
 


# Create a Business Process Project

<!-- description --> Create an empty business process to better understand how to release, deploy, and monitor processes, as part of the SAP Build CodeJam.


## Prerequisites

- You have completed the previous tutorial for the SAP Build CodeJam, [Write Logic to Maintain the Cart](codejam-04-add-to-cart).
  

## You will learn
- How to create a SAP Build Process Automation project
- How to create a Build Process that automates the Purchase Order Approval
- How to Create Process Instance and Monitor it



## Intro

SAP Build Process Automation is a low code/no code tool under SAP Build Portfolio. With this tool, you will get access to a new range of opportunities for running your day-to-day workflows. In this tutorial, you will learn how to create a business process  using visual drag and drop simplicity.

There are many use cases/scenarios where you can bring about innovation using SAP Build Process Automation. In this tutorial, you will see how a Purchase Order Approval process can be can be created which gets triggered by the Purchase order generated in the Shopping cart App. These Purchase Order requests have to be reviewed and approved/Rejected by the Approver. Once approved or rejected, the requester will be notified. So we prepare for this.





### Create a process project

1. In the Build **Lobby**, choose **Create**.

    ![Build Lobby ](1_Create_Business_Project_Lobby.png) 

2. Choose **Build an Automated Process**.

    ![Build Automated Process](2_Build_an_Automated_Process.png) 

    Choose **Business Process**.

    ![Choose Automation Type](3_Business_Automation_Type.png)

    In the Create a Business Process project dialog box, enter the following:

    | Field | Value |
    | ------ | ------ |
    | **Project Name** | `Purchase Approval` |
    | **Description** | `My purchase approval project`

    Click **Create**.

    ![Give Project Name](4_Business_Project_Name.png)

    >You may get a disclaimer screen. If so, click **Accept**.

    >![Disclaimer](Disclaimer.png)

3. Once the project is created, it will automatically try to create a process for the project.

    In the dialog, for **Name**, enter `Purchase Approval Process`.

    Click **Create**.

    ![Give Process Name](5_Business_Process_Create.png) 

    >You may get a message about the new layout editor or other news. Click **Close**.
    >
    >![Docs](docs.png)

    In your project, you will see an empty process with start and end nodes.

    ![Empty Business Process](6_Empty_Business_Process.png)






### Configure the process inputs
A process makes decisions based on data sent to it when it is triggered (the process inputs), as well as data from API calls made as part of the process (actions).

In this step, you will specify the inputs required for the process.

>In later tutorials we will create an action to get data from APIs.

1. Click the **Purchase Approval Process** tab.

    Expand the side panel. 

    ![Business Process Side Panel](7_Process_Side_Panel.png)

2. In **Process Details** (side panel to the right), click **Variables**.

    Next to **Process Inputs**, click **Configure**.

    ![Configure Process Input Parameters](8_Configure_Process_Inputs.png)

3. In the **Configure Process Inputs** window, click **Add Input** to add each input.

    ![Add Process Inputs ](9_Add_Process_Inputs.png)

    Add the following inputs, flagging them all as **Required**. 

    |  Field Name                | Type | Required |
    |  :-------------            | :------------- | :-------------
    |  `Order ID`                  | **String** | Yes |
    |  `Total`                  | **Number** | Yes |
    |  `Business Partner`          | **String** | Yes |
    |  `New Status`                | **String** | Yes |
  
    >The editor will remove leading and trailing spaces.

    Click **Apply**.

    ![Complete Adding Process Inputs](10_Complete_Adding_Process_Inputs.png)

4. Click **Save** (upper right) and close the side panel by clicking the **X**.

    ![Save Process Inputs](11_Save_Process_Inputs.png)



### Create API trigger
A business process is started by defining a trigger, an event that indicates to your SAP Build Process Automation to start an instance of this process.


>A process can be triggered in one of 3 ways:

>- **API:** SAP Build Process Automation defines a common API so that other apps can trigger processes.

>- **Event:** An event from SAP S/4HANA Cloud – via SAP Event Mesh – can be set up to trigger a specific process.

>- **Form:** You can build a form to give to your users that can be filled in and submitted to trigger the process. That form is created within the same project. Forms can also be built with SAPUI5.

For this process, we will define an API trigger, so our app can call an API to trigger it.

1. Click **Add a Trigger**.

    ![Create a Trigger](12a_Add_a_Trigger.png)

2. Choose **Call an API**.

    ![Select Call an API](12b_Select_Call_an_API.png)

3. In the Create API Trigger dialog, enter `Purchase Approval Trigger` for the **Name**. 

    Click **Create**.

    ![Create API Trigger](12c_Create_API_Trigger.png)

The process inputs are automatically added to the trigger. Click the **Trigger** block, and expand the side panel.

![Examine API Trigger Outputs](12d_Examine_API_outputs.png)

Note that API trigger's **Outputs** are automatically synchronized from the process inputs, meaning that the API will send these inputs and the rest of the process can use them.







### Release Project
To run the process, you must release and deploy it.

Releasing a project creates a version or snapshot of the changes and deploying the project makes it available in runtime to be consumed (or triggered). You can only deploy a released version of the project, and at a given time there can be multiple deployed versions of the same project.

1. Click **Release** (upper right).

    ![Press Release Project](13a_Press_Release_Project.png)

2. In the **Release Project** dialog, you can decide what type of change you are making, what version number to create, and a description of the changes in this version.
    
    However, the first time you release the project, it will automatically be set to 1.0.0, and you can only add a description.

    Click **Release**.

    ![Confirm Release Project](13b_Confirm_Release_Project.png)

Examine the release status under the **Overview** tab. Make sure it is set to **Released**.

![Examine Release Status Overview](13c_Check_Release_Status.png)

>From this dropdown you can view all the released and deployed versions of your project, as well as the latest version marked **Editable**. Whenever you need to make changes, make sure you have the **Editable** version.

>![Versions](versions.png)





### Deploy Project 

1. Once the project is released successfully, you will find a **Deploy** option on the top-right corner of the screen. 
   
    Click **Deploy**.

    ![Examine Release Status Overview](13d_Click_Deploy.png)

2. You will be asked to select an environment. Select **Public** and click **Deploy**.

    ![Deploy public](13e_Click_Deploy.png)

    >SAP Build Process Automation enables you to create multiple environments with different security so you can control who can run and update processes. For this CodeJam, you will work with the default **Public** environment, which allows everyone to deploy processes to the environment and execute those processes.

    >For more information, go to the SAP Build lobby and then **Control Tower > Environments**, or see [Environments](https://help.sap.com/docs/build-process-automation/sap-build-process-automation/environments).

3. A dialog indicates the triggers to be created and deployed.
    
    Click **Deploy**.

    ![Deploy trigger](13f_Click_Deploy.png)

    >Deploying will take a couple of seconds/minutes depending upon how big your project is and how many artifacts it has. Any errors during deployment are displayed in the **Deployment Console** at the bottom of the screen.

    Once the deployment is successful, the status will change at the top of the page to **Deployed**. 
    
    ![Deployment Status](14d_Deployment_Status.png)

4. Return to the SAP Build lobby by clicking SAP Logo in the top-left corner.

    ![Return to Lobby](14e_Return_to_Lobby.png)







### Start a process instance
SAP Build Process Automation lets you trigger a process manually, generally so you can quickly test it.


1. In the SAP Build lobby, choose **Monitoring**. 

    ![Return to Lobby to Choose Monitor](14f_Choose_Monitor_on_Lobby.png)

2. Under **Manage**, click **Processes and Workflows**.

    ![Monitor Overview](15_Choose_Processes_Workflows.png)

    This area shows you all the processes that have been deployed. If you are using SAP Build Process Automation for the first time in your trial account, there is only one process and it is already selected.

    If it isn't selected, select it.

    ![First project](15a_project.png)

    >For when you have many more processes, you can perform a free text search, or you can select the project and then see all the processes in that project.

    With the process selected, you will see information about the process, most notably:

    - Its **ID**. You will need this ID when you make an API call to trigger this process.

    - Buttons for showing all the instances of this process that were triggered, and for starting a new instance of the process.

    >**IMPORTANT:** Make note of the process ID.

4. Click **Start New Instance**.

    This opens a dialog that lets you trigger a process – for testing  – without using a form, API call, or external event.

    ![Start New Process Instance](15e_Start_New_Instance.png)

    A dialog opens that lets you provide the required inputs for the process, in JSON format.
    
    >**IMPORTANT:** You must provide the values for the inputs that you defined as "process inputs," in JSON format. BUT ... the example JSON you will see in the dialog is not related AT ALL to what you need to provide. Therefore, you will delete this JSON.  

    >![Inputs for Starting Instance](15f_Starting_Process_Instance.png)

    Here's a reminder of the inputs you defined (in the JSON, you must use the identifier name):
   
    ![Identifiers of Process Inputs as JSON keys](15g_Identifier_as_JSON_Input.png)

5. Delete the JSON in the dialog, and replace it with the following:

    ```JavaScript
    {
    "businessPartner" : "11",
    "newStatus" : "APPROVED",
    "orderId" : "100000",
    "total" : 1000 
    }
    ```

    ![JSON Key Value Pair for your Process](15h_JSON_Context.png)

6. Click **Start New Instance and Close**.

    If all goes well, you will get the message **Instance started**.

    ![Success](success.png)







### Monitor the Process  
Once triggered, we can monitor the process instance from the **Monitor** section of the **Monitoring** tab.

1. In the same screen you triggered the process, click **Show instances**.
    
    [Show instances](show-instances.png)

    >You can also get to the same place with the menu by clicking **Monitoring**, and under **Monitor** clicking **Process and Workflow Instances** (likely first tile on the page). 

    >You would then have to filter or search for your instance. The **Show Instances** button takes you there directly.

    >![Monitor Process Flow](16a_Monitor_Process.png)

2. By default, completed processes are not shown in the list. And because we have no actions inside the process, it will start and immediately complete.

    To see your instance, in the filters, click the dropdown for the **Status** filter, and select **Completed**.

    Now you should see your process instance.

    ![Process instance](16a2_Monitor_Process.png)
    
4. Click on your process instance.

    In the details section at the top, you can see:

    - When it started
    - When it completed
    - Who started it
    - ID of the instance
    - Definition ID of the process (this is the ID we need for later)

    ![Examine the Process Flow Context](16b_Examine_Process_Log_Context.png)

    Under **Logs** you can see all the events that occurred, such as its triggering, approval forms completed, API calls performed, and any other action that was taken as part of the process.

    Under **Context**, you can see the data that was passed to th eprocess, or that was retrieved during steps that involved data. 


**Congratulations!** You have created your first simple SAP Build Process Automation process.


