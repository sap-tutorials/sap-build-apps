---
parser: v2
author_name: Daniel Wroblewski
author_profile: https://github.com/thecodester
auto_validation: true
time: 10
tags: [ tutorial>beginner, software-product>sap-business-technology-platform,software-product>sap-build, software-product>sap-build-apps--enterprise-edition, software-product-function>sap-cloud-application-programming-model, software-product>sap-business-application-studio]
primary_tag: software-product>sap-build
---



# Create a CAP Service with BAS Productivity Tools
<!-- description --> Create a SAP Cloud Application Programming model (CAP) service using the productivity tools in SAP Business Application Studio.
 
 
## Prerequisites
- You have created a trial account, as described in [Get a Free Account on SAP BTP Trial](https://developers.sap.com/tutorials/hcp-create-trial-account.html).
- You have installed SAP Business Application Studio on your trial subaccount, as described in [Set Up SAP Business Application Studio for App Development in the Trial Environment](https://developers.sap.com/tutorials/appstudio-lcap-onboarding-trial.html).
- You have installed SAP Build Apps on your trial subaccount.
  

## You will learn
- How to build a CAP service with low-code tools
- How to create roles to enable authorization for your data entities
- How to deploy your CAP service to SAP BTP


## Intro
You will quickly build a Risk Management service with 2 entities: Risks and Mitigations. Each risk can have 0 or 1 mitigations, while mitigations can be assigned to multiple (or no) risks.

Another key feature is the creation of roles, one for viewer of the data and one for manager, meaning a person who can update the data. The role, exposed in SAP BTP as a role collection, can be assigned to a user once the service is deployed.

>You must set up your SAP BTP tenant with SAP HANA Cloud and SAP Build Apps – your deployed CAP service and SAP Build app must be on the same tenant. For this tutorial, we will assume you are using a trial account.

>You can use SAP Business Application Studio on any tenant, and when it comes time to deploy, deploy the service on the tenant with your SAP HANA Cloud and SAP Build Apps. 



### Open SAP Business Application Studio


[OPTION BEGIN [Use SAP Build Lobby]]
These steps open SAP Business Application Studio using SAP Build's lobby. You must have installed one of the SAP Build tools in order to open the SAP Build lobby.

1. Open the your subaccount, making sure **Instances and Subscriptions** on the left-side menu is selected.
    
    ![Open SAP Build lobby](lobby1.jpg)

2. Open the SAP Build lobby by either clicking **SAP Build Apps** or **SAP Build Process Automation**. 

3. In the SAP Build lobby, click **Create**.

    ![Create](lobby2.jpg)

    Click **Build an Application**.

    ![Build an Application](lobby3.jpg)

    Click **Pro Developers**.

    ![Pro Developers](lobby4.jpg)

    Enter `RiskManagement` for the project name, and click **Create**.

    ![Enter name and create](lobby5.jpg)

The new project will be created in the lobby.

![New project](lobby6.jpg)

Click the project to open the project in SAP Business Application Studio.

![SAP Business Application Studio](lobby7.jpg)



[OPTION END]

[OPTION BEGIN [Use BAS Directly]]
These steps open SAP Business Application Studio in the traditional way, without using SAP Build's lobby.

1. Open the your subaccount, making sure **Instances and Subscriptions** on the left-side menu is selected.

    Click **SAP Business Application Studio**.

    ![Open BAS](1-openBAS.jpg)

2. After opening SAP Business Application Studio, click **Create Dev Space**.

    ![Create dev space](2-dev-space-create.jpg)

    Enter a name for your dev space, select **Full-Stack Application Using Productivity Tools**, and then click **Create Dev Space**.

    ![Create LCNC dev space](2-dev-space-create2.jpg)

    The dev space will be created, and you will see that is being started.

    ![Starting dev space](2-dev-space-create3.jpg)

    Soon it will say **Running**.

    ![dev space runnings](2-dev-space-create4.jpg)

3. Click the dev space name to open it, and you will the main canvas of SAP Business Application Studio.

    ![dev space runnings](2-dev-space-create5.jpg)

4. Click **Create Project**.

    ![Create project](3-create-project.jpg)

    Select the **Full-Stack Project** generator, and click **Start**.

    ![Create project](3-create-project2.jpg)

    For the name of the project, enter `RiskManagement`, and then click **Finish**.

    ![Create project](3-create-project3.jpg)

    BAS will generate the project and all the files, and you will see the following, where you can create data models, the services that will expose the data, and more:

    ![Create project](3-create-project4.jpg)

[OPTION END]


### Create data model
1. In the storyboard, click the plus sign ( **+** ) above **Data Models**.

    ![New data model](4-create-data-model.jpg)

    This will display the data model canvas, with a new data entity called `Entity1`. You will now change the name and add the fields.

    Click the **Show Details** icon to open the editor.

    ![Alt text](4-create-data-model2.jpg)

2. In the editor, change the name of the entity to `Risks` (save by clicking the checkmark ☑️) and add the following fields:

    | Property name    | Property Type |  Max Length |
    | -------- | ------- | -------- | 
    | title  | string    | 100 | 
    | prio | string     | 5 | 
    | descr    | string    | 100 | 
    | impact    | integer    | | 
    | criticality    | integer    |  | 

    Click the checkmark ☑️ to save the changes and close the editor by clicking the top-right **X**.

    ![Risks entity fields](4-create-data-model3.jpg)

3. Create another entity called **Mitigations** by clicking **Add Entity** and dropping the new entity on the canvas.

    ![New entity](4-create-data-model4.jpg)

    Again, click the **Show Details** icon (select the entity to see its side menu).

    ![Alt text](4-create-data-model5.jpg)

    Change the name of the entity to `Mitigations` (and click ☑️) and add the following fields:

    | Property name    | Property Type |  Max Length |
    | -------- | ------- | -------- | 
    | description  | string    | 100 | 
    | owner | string     | 100 | 
    | timeline    | string    | 100 | 

    ![Alt text](4-create-data-model6.jpg)

    Click the checkmark ☑️ to save the changes and close the editor by clicking the top-right **X**.

4. Add relationship between the entities by selecting the **Mitigations** entity, and clicking – and holding down – the **Relationship** icon.
   
    ![Mitigations relationsip](4-create-relationship.jpg)

    Drag the icon and drop it on the **Risks** entity.

    ![Risks relationsip](4-create-relationship2.jpg)

    This will open the relationship editor.

    ![Alt text](4-create-relationship3.jpg)
    
    Set the following properties:

    | Field    | Value | 
    | -------- | ------- |
    | Type  | Association    |
    | Multiplicity  | To-many    |
    | Name  | risks    |
    | Target Entity  | Risks    |
    | Backlink Property  | miti    |

    >All the fields should already be set, except the backlink property.

    Click the checkmark ☑️ to save the changes and close the editor by clicking the top-right **X**.

5. For both entities, open the **Show Details** pane again.

    Select the **Aspects** tab, and enable the **managed** aspect. Click the checkmark ☑️ to save the changes.

    ![Aspects](4-create-relationship4.jpg)

    Repeat for **Mitigations** entity.





### Create service

1. Back in the storyboard, click the plus ( **+** ) next to **Services** to add a service.

    ![Add service](5-service-add.jpg)

    Choose the **Risks** entity, and click **OK**.

    ![Risks entity](5-service-add2.jpg)

2. Click **Add Entity** and place the new entity in the canvas.

    ![Add entity](5-service-add3.jpg)

    Choose the **Mitigations** entity, and click **OK**.




### Add sample data

1. Go back to the storyboard.

2. Right-click the **Mitigations** entity, and select **Add Sample Data**.

    ![Add sample data](6-sample-data-mitigations.jpg)

3. Choose **Import**, and click **Add**.

    ![Import data](6-sample-data-mitigations2.jpg)

4. Select the downloaded `RiskManagement-Mitigations.csv` file.

    The data is automatically added and saved.

    ![Data added](6-sample-data-mitigations3.jpg)

5. Go back to the storyboard, and do the same for the **Risks** entity with the `RiskManagement-Risks.csv` file.





### Add user roles

1. On the storyboard, select the little person icon next to **RiskManagementService**.

    ![Open roles editor](7-roles-man.jpg)

    >You can also open the **User Roles** editor by selecting the **Open Editor** dropdown and choosing **User Roles**.

    >![Open editor alternative](7-roles-man1a.jpg)

2. Add a role by clicking the plus sign ( **+** ) next to **User Roles**.

    ![Plus for roles](7-plus-roles.jpg)
    
    Call the role `RiskViewer` and keep the privilege at **Read*.

    ![Add role](7-roles-man3.jpg)

    Click **Save**.

1. Select the new role, and then select your service, **RiskManagementService** from the dropdown. 

    ![Add service to role](7-roles-man4.jpg)

    Click **Add Service Entities**, and toggle on the two entities. The privileges should already be set to **Read**.

    Click **Save**.

     ![Add entities](7-roles-man5.jpg)

2. Add another role by clicking the plus sign ( **+** ).

    Call the role `RiskManager` and change the privilege to **Full*.

    ![Add role](7-roles-man6.jpg)

    Click **Save**.

3. Select the new role, and then select your service, **RiskManagementService** from the dropdown. 

    Click **Add Service Entities**, and toggle on the two entities. The privileges should already be set to **Full**.

    Click **Save**.

     ![Add entities](7-roles-man7.jpg)





### Deploy project to SAP BTP
[OPTION BEGIN [Task Explorer]]
1. Open the Task Explorer by clicking on the side panel.

    ![Task Explorer](8-icon-task-explorer.jpg) 

    Expand the **Deploy** node to see your project.

    ![Expand node](8-deploy-project.jpg)

2. Hover over **Deploy RiskManagement** and click the **Run** icon next to your project.

    The deployment process will run briefly, a **Terminal** window will open at the bottom of the screen, and then the Cloud Foundry login screen will appear. (You may need to make the **Terminal** area smaller to see the login screen.)

    ![Cloud Foundry login](8-CF-sign-in.jpg)

    >You are deploying to Cloud Foundry, so you must login and tell BAS to which Cloud Foundry to deploy.

3. Enter your Cloud Foundry endpoint, and your username and password for SAP BTP trial, and click **Sign-in**.

    Once you login to Cloud Foundry in general, BAS will want to know to which subaccount and space you want to deploy to.

    ![Subaccount and space](8-deploy-login4.jpg)

    Select the subaccount (Cloud Foundry organization), and then the space.

    >If you are working on trial, you will likely have only one choice for each.

    Click **Apply**.

The deployment will take about 5 minutes to finish. When complete, you will get the URL to your service, though you will not have permissions to view it.

[OPTION END]

[OPTION BEGIN [Command Palette]]
You should use the Task Explorer for deployment. The following instructions describe another, more manual way to do deployment.

1. Log into Cloud Foundry on your trial account by pressing **Control-Shift-P**.

    ![Command palette](8-deploy-login.jpg)

    Type in `CF: Log`, and then select **CF: Login to Cloud Foundry**.

    ![Login command](8-deploy-login2.jpg)

    Enter your Cloud Foundry endpoint, and your username and password for SAP BTP trial, and click **Sign-in**.

    ![Credentials](8-deploy-login3.jpg)

2. Once you login to Cloud Foundry in general, BAS will want to know to which subaccount and space you want to deploy to.

    ![Subaccount and space](8-deploy-login4.jpg)

    Select the subaccount (Cloud Foundry organization), and then the space.

    >If you are working on trial, you will likely have only one choice for each.

    Click **Apply**.

3. Go back to the **Home** page, and click **Deploy > Deploy**.

    The deployment will take about 5 minutes to finish. When complete, you will get the URL to your service, though you will not have permissions to view it.

    ![Deployment](8-deploy-login6.jpg)
   
[OPTION END]

### Check service

1. In your trial account, on the left-side menu, click **Cloud Foundry > Spaces**. You likely will have only one space, called **dev**.

    Click the space, and you will now see your application.

    ![Check app](9-check.jpg)

    Click on the link `RickManagement-srv`, and you will see a link. Click on it.

    ![Service start](9-check2.jpg)

    You will get a page with information about your service, with a link to the service (`service/RiskManagement`).

    >You will not see anything yet since you are not authorized.

2. You will need some information about your service to create a destination so that it can be accessed by other applications, like SAP Build Apps.

    Go back to the previous page, and on left click **Service Bindings**, and then click `RiskManagement-uaa`.

    ![Service bindings](9-check4.jpg)

    On the right you will see a service key, and by clicking **View** you will get the client ID, and secret and other information for this service so that you can create a destination.

    ![Service key](9-check5.jpg)




