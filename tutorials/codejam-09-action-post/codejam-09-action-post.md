---
parser: v2
author_name: Shrinivasan Neelamegam
author_profile: https://github.com/neelamegams
auto_validation: true
time: 20
tags: [ tutorial>beginner, sap-conversational-ai>sap-build, sap-conversational-ai>sap-build-apps, sap-conversational-ai>sap-build-process-automation]
primary_tag: sap-conversational-ai>sap-build
---
  

# 9 - Create Action Project to Post Data to CAP Service
<!-- description --> Create an action project to update our CAP service and call the action from your process when the order is approved, as part of the SAP Build CodeJam.



## Prerequisites
- You have completed the previous tutorial for the SAP Build CodeJam, [Create Action to Get Data from SAP S/4HANA Cloud](codejam-08-action-get).





## You will learn
- How to create an action to update data
- How to use an action in your process





## Intro
In the previous tutorial, you created and used an action to retrieve data from an SAP S/4HANA Cloud system.

In this tutorial, you will create another action, this time to connect to our CAP service and update an entity.








### Enable destination in processes
SAP Build Process Automation enables administrators to control which destinations – and, therefore, which backend systems – can be connected to processes during runtime. 

You already created the destination for the CAP service because you needed it in the app. In this step, you will enable the destination to be used in your process.

1. Go to the SAP Build main page.

2. Go to **Control Tower**, and then click **Backend Configuration > Destinations**.

    ![Add destination](add-dest1.png)

3. Click **New Destination**.

    ![New destination](add-dest2.png)

    Select your destination, **CodeJamOrdersService**.

    Click **Add**.

    ![Add](add-dest3.png)

    The destination will be added to the list of destinations that can be used within your processes for runtime.


























### Create action project

1. Go back to the SAP Build lobby, and click **Connectors > Actions**.

    ![Select Actions](images/1-actions-from-lobby.png)

2. Choose **Create**.

    ![Create Actions](images/1a-actions-create.png)

3. Select **Live API > Other BTP Destinations**.
   
    ![Select OData](images/1b-select-live-other-btp-destinations.png)

    Select **CodeJamOrdersService**.

    ![Select CodeJam Orders](images/1c-select-codejamordersservice.png)
    
    >CodeJamOrdersService was already configured as a destination in the SAP BTP cockpit – with the additional property `sap.applicationdevelopment.actions.enabled` – and hence is visible here.

    After selecting the destination, you will see a list of all the entities and all the API calls you can make.

    Click **Next**.

4. Enter `OrderManagement-Actions` for the project name and description.
    
    Click **Create**.

    ![Project Name](images/1d-provide-projectname-create.png)

    You will now see the operations available for this service, and you will need to decide which ones to expose.

5. In the **Orders** entity, you want to only update the **status** field. Hence, in the popup, you have to select the **PATCH** method of **/Orders({ID})** API. 
    
    Select the **Filter actions** dropdown, filter option and choose **PATCH**. 
    
    ![Patch Update](images/2-update-entity-orders.png)

    Now, under **Orders** there will appear just one API. Select it.

    >SAP Build Actions will open with the selected APIs. You can always go back and add additional APIs to expose.

    Click **Add**.

    The PATCH method to update the Orders entity is now selected and the action project is saved.

    ![Path operation](images/2-update-entity-orders-final.png)






### Test action
Let's make sure everything is connected properly and test the action.

1. Click the **Test** tab.

    Under **Connectivity > Destination**, select the destination **CodeJamOrdersService** from the dropdown options.

    ![Test](test1.png)

2. Under the **Test Input Values**, enter `6c25e827-15c2-4e7f-be1a-89fb4304d4fa` for the **ID** field.
   
    ![Test Input Values](test2.png)
   
3. Click **Test**.

    Once the execution is successful, you should see **200: OK** response.

    No data was changed since we did not supply any values for any of the fields.

    >If for some reason you entered an order ID that did not exist, the service will create it, and you will get a **201: OK** status code.

    >If for some reason you entered a value that was not a GUID, you will get **400: BAD REQUEST** or **404: NOT FOUND** status code.









### Release and publish action
You will now release the action project to create a version and then publish this version in the action repository. It is these published action that you will connect to from your process.

1. Click **Release** (top right).

    ![Release Actions](images/4-release-actions-project.png)

2. In the release popup, enter the **Release Notes** of your choice.

    ![Release Notes](images/4a-release-notes.png)

    >You have the option to enter release notes.

    Click **Release** again, to confirm.

    Notice the label has changed to **Released**.
    
3. Click **Publish to Library** (top right).

    ![Publish](images/4b-publish-to-library.png)

    Click **Publish** to confirm.

    After publishing the actions project, notice the label has changed to **Published**.

    ![Published Actions](images/4c-published-status.png)









### Add action to process

You have already set up the approval form as part of the tutorial [Add Approval Flow to Process](codejam-06-spa-approval). 

Now, you will add the action so that if the order is approved, the action will update the status of the order.

![Approval form Approve flow](images/5-approvalform-approve-flow.png)

1. Go back to your **Purchase Approval** project, and open the process.

    Make sure the **Editable** version is selected.

    ![Open project](open-project.png)

2. Click on the plus sign, **+** below the **Approval Notification** form.

    ![Add action](add-action1.png)

    In the pop-up, choose **Action**.

    ![Invoke Action](images/5b-actions-on-pop-up.png)

3. In the **Browse library** pop-up, select the **Update entity in Orders** tile.

    >If you are on a system with lots of actions, use the search text to find your **OrderManagementService** action, or select your project from **Projects** dropdown.

    ![Select action](add-action2.png)

    Click **Add**.

    The action gets added and a side pane opens.

    ![Action added with Side pane](images/5d-action-added-sidepane-open.png)

    >You will see a red warning icon. This is only because you still need to bind data to the action, for example, the order ID.

4. In the side pane, click the **Destination variable** dropdown, and choose **Create Destination Variable**.

    >A destination variable creates a way that you can deploy your process to different systems – dev, test, production – and assign different destinations for this action to each environment, without having to change the process itself. 

    ![Create variable](add-action3.png)
    
    Enter **MyCAPDest** in the **Identifier** field, and click **Create**.

    >Destination variable identifiers cannot have spaces.

    ![Destination variable](images/5e-create-destination-variable.png)


5. In the side pane, go to the **Inputs** tab.

    Click in the **ID** field (this is one of the inputs defined in the action). You must bind which order to update.
    
    Choose **Process Inputs > Order ID**.

    ![Inputs tab](images/5f-input-tab-orderid.png)

    Click in the **status** field, and then choose **Process Inputs > New Status**.

6. Let's do one more thing to make the process more efficient.

    You updated the CAP service to change the status of our order when an approver approves the purchase. But you also want to update the order status when it is automatically approved.
    
    Under the **Approval Notification 1** form, click the plus sign, **+**.

    ![Add go-to](goto1.png)
    
    Select **Controls and Events > Go-to-Step**.

    ![Go-to step](goto2.png)

    Select the **Update entity in Orders** step.

    ![Update orders](goto3.png)

    Your process flow is update.

    ![Process flow](goto4.png)

7. Click **Save** (upper right).










### Release and deploy process

1. Click **Release** (top right).

    ![Release after actions](images/6-release-after-action.png)

    In the pop-up, for **Version Comment**, enter `Added Action to update Orders entity`.
    
    Click **Release**. 

    ![Version comment](images/6a-version-comment.png)

2. Click **Deploy**.

    Select the **Public** environment, and click **Upgrade**.

    ![Select environment](deploy1.png)



    The triggers that will be deployed will be displayed. There is nothing to do here but confirm.
    
    Click **Deploy**.

    ![Deploy triggers](deploy2.png)

    Now you will see the environment variables for which you need to provide values. 

    ![Destinations](deploy3.png)

    Select the destination you created for accessing SAP S/4HANA and for the CAP service.

    Click **Deploy**.

The status of the project changes to **Deployed**. Click the SAP logo at the upper left to return to SAP Build Lobby.







### Build My Orders screen
In order to better see the status of all my orders and cart, you will create in 5 minutes a page for checking the status of your orders.

1. Open your SAP Build Apps project.

2. Open the **My Orders** page. 

    ![My Orders](myorders1.png)

3. Click **Marketplace**, and search for `basic table`.

    ![alt text](myorders3.png)

    Select the basic table, and click **Install**.

4. On the UI canvas, under the **Installed** tab on the left, drag an instance of the basic table onto the canvas.
    
    ![alt text](myorders4.png)

5. In the **Properties** pane on the right, click **Configure**.

    Select **Orders**.

    ![Orders](myorders5.png)

    Click **Properties**, then click the **X** next to **Filter conditions**.

    ![Filter](myorders6.png)

    Select **Object with properties**, then **Add Condition**.

    Set a condition where **customer** is equal to the following formula:

    ```JavaScript
    systemVars.currentUser.email
    ```

    ![Customer](myorders7.png)

    Click **Save** twice.

    Click **Save and Exit** (top right.)

6. Click the **Navigation** tab.

    Click **Add Item**, and set the new item to the following.

    | Icon    |   Tab name  |   Page  |
    | --- | --- | --- |
    | `credit card`    |   `My Orders`     | `My Orders` |

7. Click **Save** (upper right).






### Test process

Go into your app, select the **Notebook Basic 18**, and add it to your cart.

Go to the **My Orders** page and you will see your cart.

![Cart](testapp1.png)

Go to the **Cart** page, and you will see your cart with the item you just selected.

Select the business partner **100000**, and then click **Purchase**.

![Cart item](testapp2.png)

Go back to the **My Orders** page, and you will see that your cart now has a total – 1570 – and its status is now **REQUESTED**.

![Requested](testapp3.png)

Open your Inbox, and you can see the approval form.

![Approval form](testapp4.png)

Click **Approve**. The form disappears. Refresh the Inbox and you should see the notification form.

![Notification form](testapp5.png)

Click **Submit**. The process continues to update the status of the order, and then completes.

Check the process in the monitor tab, and you can see all the steps completed.

![Process complete](testapp6.png)

Back in your app, refresh the page and check the **My Orders** page again, and you will see the status of the order updated.

![Order approved](testapp7.png)

>You will also notice that there is a new cart. Since you submitted the cart and changed it's status to **REQUESTED**, the app no longer had a cart. And when the app returned to the home page, it created a new cart in the CAP service.