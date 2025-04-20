---
parser: v2
author_name: Daniel Wroblewski
author_profile: https://github.com/thecodester
auto_validation: true
time: 20
tags: [ tutorial>intermediate, software-product>sap-build, software-product>sap-integration-suite, software-product>sap-build-process-automation]
primary_tag: software-product>sap-build
---

# 8 - Create Action to Get Data from SAP S/4HANA Cloud

<!-- description --> Create an action project for defining the API call for retrieving additional data about the business partner, who ID was sent in the event. 

## Prerequisites
- You have completed the previous tutorial for the event-based processes CodeJam, [Add Approval Step and Use Inbox](codejam-events-process-7).

## You will learn
- How to create an action (for calling APIs)
- How to add an action into your process
- How to configure an action
- How to work with the data returned by the action

## Intro
IT administrators can create "actions" to expose backend APIs to developers of processes or apps (using SAP Build Apps). 

From within a package of APIs, actions let the admin expose only the APIs that are meaningful to the developers, and to select a subset of the input and output parameters to make complex APIs easier to use.

Actions specify the details of the API call, but do not specify a backend system. The developer of the process or app will later – during design time – assign the appropriate destination to the action.


### Create action to get a business partner
1. Open SAP Build, and click **Actions**.

    ![Open Actions](assets/action1.png)

2. Click **Create**.

    Scroll down and select **SAP Business Accelerator Hub**.

    ![SAP Business Accelerator Hub](assets/action2.png)

    Enter **BusinessPartner** in the search.

    ![Business partner search](assets/action3.png)

    Select **Business Partner (A2X)** from **SAP S/4HANA Cloud Public Edition**.

3. The next screen just shows all the API calls inside this API package, for your information.

    ![Review APIs](assets/action4.png)

    Click **Next**.

4. Now you can name the action.

    ![Name action](assets/action5.png)

    Keep the defaults, and click **Create**.

    The newly created action opens, again showing all the OData calls. But now you can select which ones to include in your action.

5.  In the search, enter **Retrieves business partner data**.

    Open the **Business Partner** node, and select the **/A_BusinessPartner('{BusinessPartner}')** API call.

    ![Select API](assets/action6.png)

    Click **Add**.

    You now get the action where you can modify the inputs and outputs.

    ![Action opened](assets/action7.png)


6. Click **Output**.

    Check the **d** node, and then expand it.

    ![Select outputs](assets/action8.png)

    **Uncheck** the following fields (it might help to sort by **Key**):

    ![Sort](assets/action9.png)

    - BusinessPartner
    - BusinessPartnerGrouping
    - CreationDate
    - FirstName
    - LastName
    - NameCountry

    Scroll back to the top and click **Remove** and then confirm **Remove**.

    ![Remove outputs](assets/action10.png)

    You should be left with just a few fields in the output.

    ![Outputs](assets/action10a.png)

7. Click **Test**.

    Select **Manual** under **Host**.

    For the URL, enter: 
    
    ```URL
    https://s4-mock-server-with-bp-created-events.cfapps.eu10.hana.ondemand.com/sap/opu/odata/sap/API_BUSINESS_PARTNER
    ```

    ![Set up test](assets/action11.png)

    Set the authentication to **Basic Authentication** and then enter the user and password distributed in the CodeJam by your instructor.

    ![Add credentials](assets/action12.png)

8. For the **BusinessPartner**, input the ID for the business partner you created (or enter this ID: **1003769**).

    Click **Test**.

    You should get data from the backend.

    ![Test successful](assets/action13.png)

9. Click **Save**.

    ![Save action](assets/action13a.png)

    Click **Release**, then click **Release** to confirm.

    ![Release action](assets/action14.png)

    Click **Publish**, and then click **Publish** to confirm.

    ![Publish action](assets/action15.png)








### Create destination to SAP S/4HANA Cloud
In the action test, we supplied the URL and credentials, but this was just for the test. In order to use in production, we want to use a destination – a connection defined in the SAP BTP cockpit to a backend system.

Destinations can then be used seamlessly in applications and processes to connect to systems, without needing to know the connection details and using all types of sophisticated authentication schemes. 

1. Download the destination template. 

    Click [Destination template](https://github.com/sap-tutorials/sap-build-apps/blob/main/tutorials/codejam-events-process-8/S4HANA_Badges), and then click the download button.

    ![Download destination](assets/destination1.png)

2. Go to the SAP BTP cockpit for your subaccount.

    >You can always return to your trial account cockpit by going to [https://account.hanatrial.ondemand.com/](https://account.hanatrial.ondemand.com/) and then opening your subaccount.

    Click **Connectivity > Destinations**.

    ![Import destination](assets/destination2.png)

    Click **Import Destination**, and select the file you downloaded.

3. Enter the password distributed in the CodeJam by your instructor.

    Click **Save**.

    ![Save destination](assets/destination3.png)

4. Click **Check Connection**, and you should get a **200** status code.

    ![Test destination](assets/destination4.png)






### Add destination to Control Tower
You created a destination, but you must let SAP Build Process Automation know that you want to allow it to be used in deployed processes.

1. In the main SAP Build page, click **Control Tower**.

2. Click **Destinations**.

    ![Add destination to Control Tower](assets/destination-add1.png)

3. Click **Add**.

    ![Click Add](assets/destination-add2.png)

    Select the **S4HANA_Badges** destination.

    ![Select destination](assets/destination-add3.png)

    Click **Next**.

    Keep **All Environments**, and click **Add Destination**.

    ![Destination added](assets/destination-add4.png)

The destination should now appear in the list of destinations that can be used with your processes.




### Add action to process
1. Go back to your process project.

    Make sure you are in the Editable version.

    ![Open project](assets/addaction1.png)

2. Under the trigger, click the plus sign, **+**.

    ![Add step](assets/addaction2.png)

    Select **Action**.

    ![Select Action](assets/addaction3.png)

    Select **Browse All Actions**.

    ![Browse All Actions](assets/addaction4.png)

    Click **Add** next to your action. To help you find the right action, you can see the name of the acttion project.

    ![Add action](assets/addaction5.png)

    The action is added to the process.

3. With the action selected and the process panel on the right open, find the **Destination Variable** field.

    Click **Select a Destination Variable**, then click **Create Destination Variable**.

    ![Add destination variable](assets/addaction6.png)

    For the identifier, enter **BPdest**.

    Click **Create**.

    ![Destination variable added](assets/addaction7.png)

4. In the **Inputs** tab, click inside the **BusinessPartner** field.

    Select **Process Inputs > data > BusinessPartner**.

    ![Bind business partner](assets/addaction8.png)

5. Save the process by clicking **Save** (upper right). 



### Update approval form
Now that we've retrieved the data, we want to display it in the approval form. You will add a field to display the business partner's country and grouping.

1. On the **Badge Approval** form, click the three dots and select **Open Editor**.

    ![Open form editor](assets/update1.png)

2. Click the **Text** field on the left twice, which adds 2 new text fields at the end of the form.

    ![Add text fields](assets/update2.png)

3. Set the 2 fields as follows:

    | Field Name | Settings |
    |-------|--------|
    | **Country** | Make the field **Read Only** | 
    | **Grouping** | Make the field **Read Only** | 

    Click **Save**.

    ![Text fields added](assets/update3.png)

4. Back in the process, select the **Badge Approval** form.

    Open the **Inputs** tab.

5. Bind the 2 new inputs as follows:

    | Field | Binding |
    |-------|--------|
    | **Country** | **Retrieve business partner > result > Business Partner > Ctry/Reg. for Format** | 
    | **Grouping** | **Retrieve business partner > result > Business Partner > Grouping** | 

    Click **Save**.

    ![Fields bound](assets/update4.png)








### Release and deploy process
1. Click **Release**.

    ![Release](assets/release1.png)

    Click **Release**.

    >Note that after releasing, you are still inside the editable project, not the released version.

2. Click **Show project version** (upper left).

    ![SHow new release](assets/release2.png)

    This will take you to the released version of your project, so you can deploy it.

3. Click **Deploy**.
   
    ![Deploy](assets/release3.png)

    Select the **Public** environment.

    Select **Upgrade** – as you already have a version deployed.

    You get a message that your deployment will update the existing trigger.

    ![Triggers](assets/release4.png)

    Click **Deploy**.

    You now get a new dialog to select values for all the environment variables you created for this project. In this case, you created a destination variable for your action, in order to select the destination for connecting to your backend system.

    ![Destinations](assets/release5.png)

    Select your **S4HANA_Badges** destination, and then click **Deploy**.






### Trigger process
1. Go back to the [**Create Business Partner** app](https://s4-mock-server-with-bp-created-events.cfapps.eu10.hana.ondemand.com/create-bp) we provided to you.

    ![Business partner app](assets/BP1.png)

2. Enter the following:

    | Field | Value |
    |-------|--------|
    | **First Name** | Anything you want | 
    | **Last Name** | Anything you want | 
    | **Country** | Select one of the countries |      
    | **SAP Community Username** | Your user name in the SAP Community |      

    >**IMPORTANT:** You set up your REST Delivery Point to publish only events that contain your community user. So you need to provide your user in order for the event to be delivered to your SAP BTP tenant and to trigger your process.

    ![Add business partner](assets/BP2.png)

    Click **Create**.

    Your business partner is created.

3. Check that the event was received into SAP Build by going to **Monitoring** (in the main SAP Build page).

    Click **Acquired Events**.

    ![Acquired events](assets/BP4.png)

    Click **Business Events**. You should see an event created for your new business partner, including the business partner ID that you saw when you created it.

    ![Business events](assets/BP5.png)









### Monitor process
1. In SAP Build, click **Monitoring**.

2. Click the **Processes and Workflow Instances** tile.

    ![Process and Workflow Instances](assets/monitor1.png)

3. Click on the process instance to see its details.

    In the **Logs** area, you will see the step where the action was started and completed.

    ![Action steps](assets/monitor2.png)

    In the **Context** area, you will see data received from the event, as well as the data retrieved from the action call to our business partner.

    ![Data from business partner](assets/monitor3.png)

4. In your Inbox, see the approval form. It now contains the country and grouping, which you retrieved from the SAP S/4HANA Cloud backend.

    ![Data in the approval form](assets/monitor4.png)


Feel free to finish the process by approving the badge and then acknowledging the notification, as you did in the previous tutorial.





### Further study

- [Actions in SAP Build Process Automation (video)](https://youtu.be/A_o8qwUnXRo)

    <iframe width="560" height="315" src="https://www.youtube.com/embed/A_o8qwUnXRo" frameborder="0" allowfullscreen></iframe>

- [Create an Action Project (Help Portal)](https://help.sap.com/docs/build-process-automation/sap-build-process-automation/create-action-project)

    - [Using $filter in actions](https://help.sap.com/docs/build-process-automation/sap-build-process-automation/managing-input-and-output-parameters-c9a06f9520cc44879f16933e9ab6a7e0)

> **Things to Ponder**
>
>What is the advantage of creating actions?
>
>When creating an action, why configure it? Why not leave all the API calls and all the inputs/outputs?
>
>What is the purpose of a destination variable?
>
>What is another advantage of using actions (related to SAP Build Apps, see this [news item](https://www.youtube.com/watch?v=haaZDIgz7kI&list=PL6RpkC85SLQAVBSQXN9522_1jNvPavBgg&index=26))?
