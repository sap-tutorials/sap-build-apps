---
parser: v2
author_name: Daniel Wroblewski
author_profile: https://github.com/thecodester
auto_validation: true
time: 30
tags: [ tutorial>beginner, software-product>sap-build, software-product>sap-build-apps, software-product>sap-build-process-automation]
primary_tag: software-product>sap-build
---
   

# 5 - Add Approval Flow to Process
<!-- description --> Fill in your empty process with conditions, approval workflow and other steps, as part of the SAP Build CodeJam.



## Prerequisites
- You have completed the previous tutorial for the SAP Build CodeJam, [Create a Business Process Project](codejam-04-spa-empty-process).
  
 

## You will learn
- How to create a data type
- How to add an approval form
- How to add a notification form
- How to use a script task
- How to use the Inbox to view your tasks


## Intro
From the previous tutorial, you know how to set up a process project, to create an API trigger, to manually trigger a process, and to monitor the process.

In this tutorial, you will add some real logic to the process, specifically to automatically approve items less than 1000, and require approval for items larger than 1000.

SAP Build Process Automation lets you add different activities and steps to your process, such as:

- **Forms:** Used for notification or to let users enter information.
- **Approval Forms:** Special forms that let approvers indicate they approve or reject all or part of a process request.
- **Actions:** Gets/posts data to and from backend systems.
- **Mail:** Sends emails.

Another major activity is **Automation**, which defines a set of steps to be carried out on a machine or virtual machine, such as performing activities in SAPGUI, UI5 or Microsoft Office apps, opening and manipulating web sites, and much more. You will not include automations in this exercise.


### Create data type
You will want to send a list of items to the process, each with a product, price, quantity and total.

To do this, you can create a data type with all those fields, and specify that you want a list of them.

1. Go back to the SAP Build lobby, and open your project.

    At the top of your project, make sure **Editable** version is selected.

    ![Editable project](images/1-editable.png)

2. On the **Overview** page, click **Create** and select **Data Type**.

    ![Create data type](images/1-data-type.png)

    ![Select data type](images/1-data-type2.png)

    For the name of the data type, enter `Order Item Type`, and then click **Create**.

3. In the **Order Item Type** window, click **New Field**.

    In the **General Information** area on the right, call the field `product`. Keep the type as **String**.

    ![First field](images/1-first-field.png)

4. Repeat by clicking **New Field** for each of the following fields, and entering the name and type:

    | Name | Type |
    |------|------|
    | `quantity` | **Number** |
    | `price`| **Number** |
    | `total` | **Number** |

    >If the **Type** of the fields is not reflected correctly in the table, select the correct **Type** again from the dropdown.

    With all the fields, the screen should look like this (make sure the **Type** is correct):

    ![Complete data type](images/1-finished.png)

    Click **Save** (upper right).






### Update inputs
Now that you have a data type, you want to add to the process another input for receiving a list of items.

1. Open the **Purchase Approval Process** process.  

    >If the process tab is not already open, you can go back to the **Overview** tab and double-click the process. 
    
    >Processes start with this icon:
    
    >![Process icon](images/2-process-icon.png)

2. Open the side panel, and click **Variables**.

    ![Variables](images/2-variables.png)

3. Click **Configure** next to the **Process Inputs**.

    A dialog with the existing inputs is displayed.

4. Click **Add Input**, and enter the following:

    | Name | Type | Required | List |
    |------|------|------|------|
    | `Order Items` | **Order Item Type** | Checked | Checked |

    Click **Apply**.

    ![List](images/2-list.png)

5. Click **Save** (upper right).








### Add a condition step
The process will include a condition that if the total is less than 1000, the purchase is automatically approved. Otherwise, it requires a person to approve it.

1. Click the plus sign, **+**, in the middle of the canvas.

    ![Condition](condition-1.png)

    In the dialog, click **Controls and Events**.

    ![Controls and Events](condition-2.png)

    Then click **Condition**.

    ![Condition](condition-3.png)

    This will add a condition step to the process.

    ![Condition added](condition-4.png)

    >There is a red warning triangle, next to the condition only because any new condition needs to be configured – NOT because you did anything wrong. 

2. Click **Open Condition Editor**.

    A dialog for setting a condition opens.

3.  In the first column, click in the box, and select **Process Inputs > Total**. 

    ![Add total](conditionEditor0.png)

    After selecting the total field, the editor should look like this:

    ![Total added](conditionEditor1.png)

    For the middle field, select **is less than**.

    For the last field, enter `1000`.

    ![Condition complete](conditionEditor2.png)

    Click **Apply**.

    >The warning triangle next to the condition step should now disappear.

4. On the side panel, change the name of the condition to `Check < 1000`.

    ![Condition name](condition-name.png)

    Click **Save** (upper right).




### Add script to get total items
We will need an approval for the purchase, and to help the approver you will calculate the total number of items (sum of all quantities) and provide this in the approval form.

To do this, you will use the Script Task, which lets you write JavaScript snippets to do more complex calculations. And you will need a custom variable to hold the results of your calculation.

1. Click in an empty space on the process canvas, then open the side panel.

    ![Open side panel](script1.png)

2. Select **Variables**, and then next to **Custom Variables** click **Configure**.

    ![Configure custom variables](script2.png)

    Add the following variable:

    | Name | Type | 
    |------|------|
    | `Sum of All Items` |**Number** | 

    Click **Apply**.

    ![Add variable](script3.png)

3. Under the **Default** path for the **Check < 1000** condition, click the **+** sign.

    ![Add script](script4.png)

    Select **Script Task**.

    ![Script Task](script5.png)

4. With the Script Task selected, click **Open Editor**.

    ![Open editor](script6.png)

    Enter the following script in the right-side editor:

    ```JavaScript
    $.context.custom.sumOfAllItems = 0;
    $.context.startEvent.orderItems.forEach(function (item) {
        $.context.custom.sumOfAllItems += item.quantity
    })
    ```

    The task should look like this:

    ![Enter script](script7.png)


5. To test the script, click **Test Variable**.

    Click the **+** sign.

    ![Test script](script8.png)

    For name, enter `$.context.startEvent.orderItems`.

    For the value, enter the following:

    ```JavaScript
    [{
        "product": "Pencils",
        "quantity": 2,
        "price": 14.5,
        "total": 29
    }, {
        "product": "Pens",
        "quantity": 10,
        "price": 2.5,
        "total": 25
    }
    ]
    ```

    Click **Run Test**.

    The result should show you that it added all the item quantities, and set the custom variable to 12.

    ![Run test](script9.png)

    This value can now be used elsewhere in the process.

6. Click **Apply**.





### Add an approval form
If the purchase request meets the conditions for needing approval (total greater than 1000), you will need an approval form, which will be sent to the Inbox of the approver.

1. In the **Purchase Approval Process** process tab, click the **+** sign under the Script Task.

    ![Approval](approval1.png)
    
    Select **Approval**.

    ![Approval step](approval2.png)

    Select **Blank Approval**.

    ![Blank approval](approval3.png)
    
    Name the form `Approval Form`.

    Click **Create**.

    You will now have an approval form in your process.

    ![Approval form added](approval4.png)

    >You will get a red triangle warning, because you haven't yet specified the approval form and who will receive it. But you will do these soon.

2. Open the form editor by clicking the three dots in the **Approval Form** tile, and then click **Open Editor**.

    ![Approval editor](approvalform1.png) 

3. In the form editor, click **H1**, and set the text to `New Order - Needs Approval`.

    ![Header](approvalform2.png)

    >**TIP:** To quickly add fields to forms, do the following:
    >
    >- Copy the text for the field's name.
    >
    >- Click the corresponding square icon for that field type.
    >
    >- Paste in the name.
    >
    >- Set any settings, like **Read Only**.

4. Add the following fields.

    | Type | Name | Extra Settings |
    |------|------|------|
    | Text | `Business Partner` | Read Only | 
    | Text | `Business Partner Full Name` | Read Only | 
    | Text | `BP Grouping` | Read Only | 
    | Text | `Order ID` | Read Only | 
    | Table | `Order Items` | Read Only | 
    | Number | `Total Items` | Read Only | 
    | Text Area | `Additional Information` |  | 
    
    ![Approval form fields](approvalform3.png)

    >Make sure all but the last field are read-only.

6. Select the **Order Items** table and click the **+** icon.

    ![Add field](table1.png)

    Add a text field.

    ![Add text field](table2.png)
    
    Name the field `Product`.
    
    ![Product](table3.png)
    
    Repeat for the following additional fields:

    | Type | Name | 
    |------|------|
    | Number | `Quantity` | 
    | Number | `Price` | 
    | Number | `Total` | 

    In the end the table should look like this:

    ![Table complete](table4.png)

7. Click **Save**.







### Add notification form
Now you will send a notification form if the order is approved.

1. Go back to the **Purchase Approval Process** process tab.

2. Click the **+** icon under **Approve** under **Approval Form**,  and then select **Form > Blank Form**.

    ![Notification form](note1.png)

    For the form name, enter `Approval Notification`, and click **Create**. 

3. Open the form editor by clicking the three dots in the **Approval Notification** tile, and then click **Open Editor**.

    ![Open form editorAlt text](note2.png)

4. In the form editor, just add a **H1** element, and set it's text to `Order Approved`.

    ![Quick form](note3.png)

5. Click **Save** (upper right).





### Set the bindings
When you create a process, you create generic artifacts but you need to specify the values for the fields in each instance of the artifact.

For example, for an approval form, you need to specify who will be the approver for each instance of the form. Or, for a form with different fields, you must specify from where to take the data to fill those fields, again, for each instance of the form.

1. Open the **Purchase Approval Process** tab.

2. Click on **Approval Form**.

    ![Binding](binding1.png)

    Configure the fields as follows.
    
    >You likely will need to copy the text we give you below to a plain text editor (e.g., Notepad), and then from there copy into the fields, or type in the text.

    | Tab | Field | Source/Binding |
    |-----|-------|----------------|
    | **General** | **Subject** | `Approval Request for Order: ` and then click in the box and select **Process Inputs > Order ID**.<div>&nbsp;</div>![Subject](binding2.png)
    | **General** | **Recipients > Users** | Click in the box and select **Process Metadata > Process Started By**. 
    | **Inputs** | **Order ID** | Click in the box and select **Process Inputs > Order ID**. 
    | **Inputs** | **Order Items** | Click in the box and select **Process Inputs > Order Items**. 
    | **Inputs** | **Total Items** | Click in the box and select **Custom Variables > Sum of All Items**. 

    Expand **Order Items**, and you will see that the system automatically mapped the correct input fields to each of the table fields in form. 
    
    ![Mapping](binding3.png)

    >If the fields are not automatically mapped, check the data type of **Price**, **Quantity** and **Total** fields (should be **Number**) created within the **Order Items** data type earlier in this tutorial.

    You'll also notice that the warning for the approval form disappeared. 

3. Click the **Approval Notification** form, and configure as follows:

    | Tab | Field | Source/Binding |
    |-----|-------|----------------|
    | **General** | **Subject** | `Your order was approved: ` and then click in the box and select **Process Inputs > Order ID**. 
    | **General** | **Recipients > Users** | Click in the box and select **Process Metadata > Process Started By**. 

4. You can send the same approval form for when the request should be automatically approved.

    Under the **If** of the **Condition** step – meaning, the request is for a small amount of money – click the plus sign, **+**.

    ![Same binding](binding4.png)

    Select **Form**, and instead of creating a new form, select **Available Forms > Approval Notification**.

    ![Reuse form](binding5.png)

    It is added as **Approval Notification 1**.

5. Just as you added the binding for the first notification form, you need to do the same for this one.

    With **Approval Notification 1** form selected and the side panel open, configure the step as follows:

    | Tab | Field | Source/Binding |
    |-----|-------|----------------|
    | **General** | **Subject** | `Your order was AUTOMATICALLY approved: ` and then click in the box and select **Process Inputs > Order ID**. 
    | **General** | **Recipients > Users** | Click in the box and select **Process Metadata > Process Started By**.

    >Notice you made different subjects for each of the 2 instances of the notification form.

6. Click **Save**.








### Release and Deploy
When you are complete with the changes to your project, you must release and deploy another version.

1. Click **Release**.

    Since this is your second release, you see an option to describe the nature of the new release (e.g., major or minor), and to provide a comment on what has changed.

    ![Release dialog](images/release-dialog.png)

    Accept all the settings and click **Release**.

2. Click **Show project version**.

    ![Deploy start](deploy-show-version.png)

    This will open the latest released version of the project.

3. Click **Deploy**.

    Select the **Public** environment and click **Upgrade**.

    ![Deploy start](deploystart.png)

4. In the **Effect on Triggers** dialog, click **Deploy**.
    
    ![Deploy](deploy.png)

    The project will now show as deployed version 1.0.1.

    ![New version](deployfinal.png)







### Test the process
Note that all forms will be sent to **Process started by** – which is you. You will act as both the approver and the requester.

1. Go back to the **Control Tower**.

2. As you did before, navigate to **Environments > Public > Processes and Workflows** .

    ![Return to Lobby to Choose Monitor](14f_Choose_Monitor_on_Lobby.png)

3. With your process selected, click **Start New Instance**.

    ![Start New Process Instance](15e_Start_New_Instance.png)

    This opens the dialog that lets you trigger a process – for testing  – without using a form, API call, or external event.


4. In the dialog, delete the JSON and replace it with the following:

    ```JavaScript
    {
    "businessPartner": "11",
    "newStatus": "APPROVED",
    "orderId": "100000",
    "total": 500,
    "orderItems": [
        {
            "product": "Pencils",
            "quantity": 2,
            "price": 14.5,
            "total": 29
        },
        {
            "product": "Pens",
            "quantity": 10,
            "price": 2.5,
            "total": 25
        }
        ]
    }
    ```

    Click **Start New Instance and Close**.

    ![New Instances](15_Choose_Processes_Workflows.png)

    If all goes well you will get a message **Instance started**.

>**Potential problems**
>
>- If you do not provide all the values that are required in the process (i.e., in one of the bindings or steps), then you will get an error.
>
>- If you provide a number value as a string, or a date value in the wrong format, you will get an error.









### Monitor the Process  
Once triggered, you can monitor the process instance from the **Monitor** section of the **Monitoring** tab.

1. You can use the shortcut of the **Show Instances** from the **Manage** tab to show your instances.

    ![Show instances](showinstances.png)

    Since this process instance added notifications and an approval step, it won't automatically be completed and you will see it with the default filter.

    ![Running process](images/test-2.png)

3. Click on your process instance.

    In the details section, you will see:
    
    - Status is **Running**.

    - Under **Logs**, you can see a condition was completed, and you can see that because the total was under 1000, it was automatically approved.

        >The entire process is not yet completed because the notification form must still be acknowledged.

    - Under **Context**, you can see all the data you provided in the JSON when you started the instance.
    
    ![Monitoring](images/test-3.png)

4. Open the Inbox by clicking the **My Inbox** icon in the header.

    ![My Inbox](images/test-4-inbox.png)
    
5. In your inbox, you will see a list of forms on the left. 

    Since you only have 1, it is already open, and you can see that the automatic approval notification was sent (no approval is needed).
   
    ![Alt text](images/test-5-approval-form.png)

    In addition, there is a **Submit** button at the bottom of the item so you can mark this notification as complete.

    Click **Submit**.

    >Once you complete a form, it is removed from your inbox.

6. Now go back to the previous browser tab and go to the  **Monitoring** tab of the SAP Build lobby. 

    You can use the **Refresh** icon to get the latest information.

    ![Refresh](images/test-6.png)

    In the logs, you can see that the notification was completed, and therefore the process instance is completed.
    
    You can also see the status at the top as **Completed**.
    
7. You can test the process again – go back to **Monitoring > Manage**, click your process and click **Start New Instance** – this time with the following JSON, which has an amount above 1000, meaning it will require an approval: 

    ```JavaScript
    {
        "businessPartner": "11",
        "newStatus": "APPROVED",
        "orderId": "100000",
        "total": 1500,
        "orderItems": [
            {
            "product": "Pencils",
            "quantity": 2,
            "price": 14.5,
            "total": 29
            },
            {
            "product": "Pens",
            "quantity": 10,
            "price": 2.5,
            "total": 25
            }
        ]
    }
    ```

    Now if you go to check your latest instance, you will see that the script task was executed and the approval step is activated.

    ![Approval step](images/test-again-1.png)

    If you look at the context, you will see our script task ran and set the custom variable to the sum of all the quantities.

    ![Script task](images/test-again-1a.png)

    And if you open the Inbox and refresh, you will now have an approval form with **Submit/Reject** buttons at the bottom. You can also see the sum of the quantities in the **Total Items** field, the result of the script task.

    ![Approval form](images/test-again-2.png)

    If you approve, you can refresh the inbox and you will get the notification form – this time without it saying automatically approved.

    ![Notify](notify.png)