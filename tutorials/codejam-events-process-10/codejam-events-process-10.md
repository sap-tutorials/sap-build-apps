---
parser: v2
author_name: Daniel Wroblewski
author_profile: https://github.com/thecodester
auto_validation: true
time: 30
tags: [ tutorial>intermediate, software-product>sap-build, software-product>sap-integration-suite, software-product>sap-build-process-automation]
primary_tag: software-product>sap-build
---

# 10 - Call 3rd-Party System and Wait for Callback

<!-- description --> Add a "Wait for an API Call" to integrate an external system by using an action to call the system and then waiting for the system to trigger resumption of the process instance.   

## Prerequisites
- You have completed the previous tutorial for the event-based processes CodeJam, [Add 2nd Approval With Custom Variables, Conditions and Form Events](codejam-events-process-9).

## You will learn
- How to pause process instance with "Wait for an API Call"
- How to call trigger to resume process instance









### Create destination to Access Pro (3rd-party system)
Remember, our action will just describe the type of service we want to access, but we need to create a destination to indicate the actual backend service to call.

1. Download the destination template. 

    Click [Destination template](https://github.com/sap-tutorials/sap-build-apps/blob/main/tutorials/codejam-events-process-10/AccessProService), and then click the download button.

    ![Download destination](assets/destination1.png)

2. Go to the SAP BTP cockpit for your subaccount.

    >You can always return to your trial account cockpit by going to [https://account.hanatrial.ondemand.com/](https://account.hanatrial.ondemand.com/) and then opening your subaccount.

    Click **Connectivity > Destinations**.

    ![Import destination](assets/destination2.png)

    Click **Import Destination**, and select the file you downloaded.
    
    ![Import destination](assets/destination3.png)

3. Click **Save**.

4. Click **Check Connection**, and you should get a **200** status code.

    ![Test destination](assets/destination4.png)






### Add destination to Control Tower
You created a destination, but you must let SAP Build Process Automation know that you want to allow it to be used in deployed processes.

1. In the main SAP Build page, click **Control Tower**.

2. Click **Destinations**.

    ![Add destination to Control Tower](assets/destination-add1.png)

3. Click **Add**.

    ![Click Add](assets/destination-add2.png)

    Select the **AccessProService** destination.

    ![Select destination](assets/destination-add3.png)

    Click **Next**.

    Keep **All Environments**, and click **Add Destination**.

    ![Destination added](assets/destination-add4.png)

The destination should now appear in the list of destinations that can be used with your processes.

![Destination added](assets/destination-add5.png)




### Create action to generate badge
1. In the SAP Build menu, click **Actions**.

    ![Open Actions](assets/action1.png)

2. Click **Create**.

    ![Create](assets/action2.png)

    Scroll down and select **SAP Business Accelerator Hub**.

    ![Odata Destinations](assets/action3.png)

    Select **AccessProService**.

    ![AccessProService](assets/action4.png)

3. You will now see a list of all the entities and all the API calls you can make, only for your information.

    ![Available calls](assets/action5.png)

    Click **Next**.

4. Name the action as follows:

    | Field | Value | 
    |------|--------|
    | **Name** | Access Pro Service |
    | **Description** | A 3rd-party service for managing the creation of employee/visitor badges. |

    ![Name action](assets/action6.png)

    Click **Create**.

    You will now see the operations available for this service, and you will need to decide which ones to expose (to "your" developers).

5.  Select the **POST /Badges** call, since you will want to create a new badge request during your process.

    ![Select API](assets/action7.png)

    Click **Add**.

    You now get the action where you can modify the inputs and outputs, or add additional API calls to this action. We will keep the inputs and outputs as is.

6. Under **Inputs**, select **ID** and click **Remove**.
   
    >The CAP service will generate an ID automatically, so we do not want to be forced to supply that field.
   
    ![Remove field](assets/action8.png)

    Click **Save**.

    ![Save action](assets/action8a.png)

7.  Click **Release**, then click **Release** to confirm..

    ![Release action](assets/action9.png)

    Click **Publish**, and then click **Publish** to confirm.

    ![Publish action](assets/action10.png)







### Store instance ID
We will want the 3rd-party system to call our process back, and we need to supply to it the process instance ID. So first we must store the value. 

1. Go back into your process, and make sure you have the Editable version open.

    With nothing selected (just click in an open space), open the side panel.
   
    Under **Variables**, and then under **Custom Variables**, click **Configure**.

    ![Configure variable](assets/variable1.png)

2. Click **Add Variable**.

    Set the name to **instanceID**.
    
    >The identifier is automatically generated and the type is automatically String.

    Click **Apply**.

    ![Add variable](assets/variable2.png)

3. Above the notification step, add a script task.

    ![New step](assets/variable3.png)

4. Set the name to **Get instance ID**.

    ![Rename](assets/variable4.png)

5. Open the editor.

    Add the following line to the editor.

    ```JavaScript
    $.context.custom.instanceid = $.info.workflowInstanceId
    ```

    ![Create script](assets/variable5.png)

    Click **Apply**.  

6. Click **Save** (upper right). 







### Add action to generate badge 
1. Under the script task, click the plus sign, **+**, and add an action.

    ![Add action step](assets/addaction1.png)

2. Click **Browse All Actions**.

    ![Browse All Actions](assets/addaction2.png)

    Select the **Add new entity to Badges** (as part of Access Pro Service project), and click **Add**.

    ![Add action](assets/addaction3.png)

3. In the side panel, create a destination variable.

    Click inside the **Destination Variable** field, and click **Create Destination Variable**.

    ![Destination variable](assets/addaction4.png)

    Name the variable **AccessProDest**, and click **Create**.

    ![New variable](assets/addaction5.png)

4. Under inputs, bind the input fields:

    | Field | Value | 
    |------|--------|
    | **businesspartnerId** | Process Inputs > data > BusinessPartner |
    | **communityId** | Process Inputs > data > YY1_SAPCommunityDisplayName |
    | **firstname** | Process Inputs > data > FirstName |
    | **lastname** | Process Inputs > data > LastName |
    | **processInstanceId** | Custom Variables > instanceID |
    | **status** | **Approved** |

5. Click **Save**. 






### Add "Wait for an API Call"
1. Under the action, click the plus sign, **+**.

    ![Add step](assets/wait1.png)

2. Click **Controls and Events**.

    ![Controls and Events](assets/wait2.png)

    Scoll down and select **Wait for an API Call**.

    ![Wait for an API Call](assets/wait3.png)

    Click **Create an API Call**.

    ![Create an API Call](assets/wait4.png)

3. Call the API trigger **AccessProCallback**, and click **Create**.

     ![Wait Trigger](assets/wait5.png)
   
4. With the new step selected, open the side panel.
   
    Select the **Outputs** tab, and click **Configure**.

    ![Configure wait](assets/wait6.png)

5. Click **Add Ouput**.

    Set the name to **badgeId**, and click **Apply**.

    ![Add output](assets/wait7.png)

6. Click **Save**.


### Modify notification form
Our 3rd-party system will call back to our process instance, and send a badge number. Let's add that to the notification form.

1. Click the 3 dots next to your notification form, and select **Open Editor**.

    ![Open form](assets/form1.png)

2. Add a **Paragraph** and set its text to:
   
    ```Text
    Your badge number is:
    ```
    
    Add a **Text** field. Call it **Badge Number** and set to read only.

    ![New form fields](assets/form2.png)

    Click **Save**.

3. Back in the process, with your form selected, open the side panel.
   
    Under **Inputs**, bind the **badgeId** from the **Wait for Badge System** step to the new form field.

    ![Bind field](assets/form3.png)

4. Click **Save**.
   






### Add API key
In order to call an SAP Build Process Automation trigger – either a standard trigger or a wait trigger – you must send an API key with adequate permissions.

1. Go to the **Control Tower**, and click **Environments**.

    ![Environments](assets/apikey1.png)

    Select the **Public** environment.

    ![Public](assets/apikey2.png)

2. In the **API Keys** tab, click **Add API Key**.

    ![Add key](assets/apikey3.png)

3. Call the key **AccessProCallbackAPIKey**, and click **Next**.

    ![BadgeKey](assets/apikey4.png)

    Enable the following scopes:

    - trigger_read
    - trigger_execute

    Click **Next**.

    ![Add scopes](assets/apikey5.png)

    On the **Review** page, click **Add**.

    ![Add scopes](assets/apikey6.png)

4. A key will be generated. 

    >**IMPORTANT:** Make sure to copy and save it in a safe place.

    ![Add scopes](assets/apikey7.png)







### Release and deploy process
1. Click **Release**.

    ![Release](assets/release1.png)

    Confirm by clicking **Release**.

2. Click **Show project version** (upper left).

    This will take you to the released version of your project, so you can deploy it.

    ![Released version](assets/release2.png)

3. Click **Deploy**.

    ![Deploy](assets/release3.png)

    Select the **Public** environment.

    Select **Upgrade**.

    Note that it will show you that a new trigger will be created for the **Wait for an API Call** step.

    ![New trigger](assets/release4.png)

4. Click **Deploy**.

    You will now have a new destination to select, since you created a destination variable for the action to create a new badge request. 

    >This is in addition to the one you selected in previous deployments for the S/4HANA backend.

    ![Destinations](assets/release5.png)

    Select the **AccessProService** destination, and make sure the **S4HANA_Badges** destination is selected.

5. Click **Deploy**.

6. After deploying, go to the **Control Tower > Environments**, and go to the Public environment.

    ![Check triggers](assets/release6.png)

    Click **Triggers**, and you will see the trigger created for the Wait for an API Call. The trigger is called **AccessProCallback**.

    ![Wait trigger](assets/release7.png)

    Click **View**

    ![View trigger](assets/release8.png)

    You will see the connection details for the 3rd party system to call our process instances to resume them.

    Copy the URL and put it in a safe place, where you put your API key. You will need these when we use the 3rd-party system.

    >**IMPORTANT:** Your URL will be different than the screenshot.

    ![Trigger URL](assets/release9.png)


    


### Further study

- [New "Wait for API Call" Integrates Processes with External Systems (Blog)](https://community.sap.com/t5/technology-blogs-by-sap/new-quot-wait-for-api-call-quot-integrates-processes-with-external-systems/ba-p/13992848)




>**Things to Ponder**
>
>How did you get the process instance ID to pass to the 3rd-party system?
>
>WHat other details about your process can you get this way?
>
>What other integrations could you imagine with the "Wait for an API Call"?
>
>WHat other scopes can you specify for an API key?

