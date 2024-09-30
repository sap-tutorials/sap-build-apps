---
parser: v2
author_name: Daniel Wroblewski
author_profile: https://github.com/thecodester
auto_validation: true
time: 25
tags: [ tutorial>beginner, software-product>sap-business-technology-platform,software-product>sap-build, software-product>sap-build-apps--enterprise-edition]
primary_tag: software-product>sap-build
---
  

# Create SAP Build App to Trigger Workflow
<!-- description --> Trigger a workflow created in SAP Build Process Automation from an app created with SAP build Apps.

## Prerequisites
- You set up SAP Build App and entered the lobby.
- You set up SAP Process Automation, and created the process described in the tutorial group [Create a Sales Order Process and Enable Triggering Via API](https://developers.sap.com/group.sap-build-apps-process-trigger-enable.html).
- You know the `definitionId` for the process you created.
- You have set up the destination to SAP Build Process Automation, as described in [Create Destination to Trigger Process from any Service](https://developers.sap.com/tutorials/spa-create-service-instance-destination.html).




## You will learn
- How to create a simple UI
- How to create a form
- How to stylize your UI
- How to trigger from your app a workflow in SAP Build Process Automation



## Intro
This tutorial shows you how to use SAP Build Apps to create and stylize a simple, one-page app that triggers an SAP Build Process Automation workflow.

Specifically, the app lets the user enter sales order details and then send these to a process for approval. (In a real-world scenario, the sales order would then be created in S/4HANA Cloud, but that is not part of our flow.)

Your app will look something like this:

![Your app](page1-goal.png)

&nbsp;

>**Before You Begin:** You will need:
>
>- The name of the SAP BTP destination to your SAP Build Process Automation instance. The destination **MUST** be configured with the URL of the entire path of the SAP Build Process Automation [Workflow Instances API](https://api.sap.com/api/SPA_Workflow_Runtime/resource).

>   **For workshops, the destination will be created for you and the name of the destinations will be provided to you**. 
>
>- The `definitionId` for your specific workflow, which you can find within your trigger in SAP Build Process Automation project. You can get this by going to your triggers in the deployed version of your project, and clicking **View**.

>   Copy the payload to a text editor, and there you can copy just the definition ID.

>   ![Definition ID](definitionId.png)


---

### A few important UI concepts

Before you begin, we want to help you understand how apps are developed with SAP Build Apps.

Every app is built on a UI canvas – the canvas is the background of the app. On the canvas, you drag and drop components: text fields, input boxes, toggle bars, images and many other components you want the user to interact with.

![Canvas](canvascomponents.png)


Every component has properties, for example: the text to display, the value of the input box and so forth. You select the component and then its properties appear in the **Properties** pane to the right.

![Properties](properties.png)

Each property is bound to something, for example:
* Static text
* A variable (whose value can be updated elsewhere)
* Data from a backend
* A formula

For the property, you click the binding button ...

![Binding button](bindingbutton.png)

... and then select from a menu of bindings.

![Bindings](bindingbutton2.png)

Finally, for every component there are events, for example, when a user taps a button or enters data in an input box. You can capture these events and then perform an action – like display a dialog, create data in a backend, change the value of variables and much more.

This is done on the logic canvas. Select a component (or no component if you want to capture app events), and then open the logic canvas.

![Logic canvas](logiccanvas.png)

You then drag and drop flow functions and connect them to events. The flow functions are executed whenever that event occurs. And, of course, each flow function has properties to bind 😺.

![Flow functions](flows.png)


### Create a new app project
Go to the SAP Build lobby, and click **Create**.

![Create project](new-project-create.png)

Select **Build an Application**.

![Build an application](new-project-appgyver.png)   

Select **Web & Mobile Application**.

![Select project type](new-project-web-project.png)

>**IMPORTANT:** In the next step, if you are in a workshop, please use a unique identify for the name of your app, such as `Create Sales Order 001 DBW` – based on your SAP BTP user name or and based on your initials.
 
For the project name, enter `Create Sales Order`, then click **Create**.
  
![Name project](new-project-name.png)





### Create the sales order page
By default your new application contains a page with title and text fields. In this step, you will focus on turning this page into your app – by adding components like input boxes and buttons, and then styling them. 

This is known as creating the user interface, also known as the UI.

>Note that there are 2 tabs for this tutorial step. 
>
>- You can do the first tab, **Basic steps**, which we recommend and which will teach you about UI components, stylizing them, and organizing them on the page.
>
>- You can instead do the second tab, **Import Project**, which will bypass the nitty gritty of adding and stylizing components, and instead teach you how to import an SAP Build Apps project into an existing project. This way is much faster.
 

[OPTION BEGIN [Basic Steps]]

1. Select the text field, and click the **X** to delete it.

    ![Delete text](UI-delete-text.png)

2. Click on an open area (this brings up the **Properties** tab for the app instead of for a specific component), and in the **Properties** tab, change the **Page name** to `Create Sales Order`.

    ![Page name](UI-page-name.png)

4. Click the title field, and in the **Properties** tab change the **Content** text to `Sales Order Workflow`.

    ![Title](UI-title.png)

5. To the canvas, drag a Container component. 

    >Container components let you group components and configure the collection of the components as a single unit.

    With the container selected, in the **Properties** tab, change the **Component display name** to `Form`.

    ![Container](UI-container.png)

    Still with the container selected, open the **Style** tab, click the dropdown icon for the Layout Container, and click **Edit**. 

    ![Container style](UI-container-style.png)

    - For the background color, select **Level 4 background**.

    - For padding, set the padding on all 4 sides to 16px by clicking each rectangle, going to **Theme**, and selecting the **L** size.

        ![Padding](UI-padding.png)

    - For **Effects**, create a shadow by setting these properties:

        | Field | Value |
        |-------|----------|
        | **Enable Shadow**    | True   |
        | **Shadow size**    | Content Shadow 0   |
        | **Shadow color**    | `#8e8989`<div>&nbsp;</div>Click the **X**, and then select **Static color**. You can then copy and paste in the color code, or use the color picker to choose a color. ![Color picker](UI-color.png)|

    - Let's save the style by scrolling up in the **Style** tab, clicking **New Style**, entering `Layout Form Container`, and clicking **OK**. 

        ![New style](newstyle.png)

    This saves the new style in the **Style** tab.

6. Into this container ( **Form** ), drag in another container.
   
    >It may be easier to drag it into the **Tree** view on the lower right, so you can put it precisely where you want. The **Tree** makes it easier to select specific components and to create a hierarchy of components on the page.

    Inside the new container add a text and input field. The result should look like this:

    ![Inside container](UI-container2.png)

    Select the new, inner container ( **Container1** from the **Tree** view), go to **Layout** tab, and under **Layout** set the container to **Horizontal**. Then, set **Align components** to middle.

    ![Container layout](UI-container-layout.png)
    
    Select the text field, go to the **Layout** tab, then **Width and Height**, and set the width to exactly 75px.

    ![Text width](UI-text-width.png)
    
    Select the input field, and in the **Properties** tab, delete `Label` from the **Label** property.  

7. From the **Tree** view, select the inside container ( **Container1** ).

    ![Copy](copy1.png)
 
    Duplicate the container by clicking on the **Duplicate** icon.
   
    ![Duplicate](copy2.png)
 
    Repeat this 2 more times, until you have 4 fields.

    ![Multiple fields](UI-multiple-fields.png)

8.  Click on each field label and change it to the following field names, in this order:

    | Fields | 
    |-------|
    | Customer    | 
    | Material    | 
    | Amount    | 
    | Delivery Date    | 

    ![Name fields](UI-name-fields.png)

9.  At the bottom of the page (outside the outside container), add a button.

    In the **Properties** tab, set the **Label** to `Get Approval`.

    ![Add button](UI-button.png)

    In the **Style** tab, next to **Primary Button**, click the dropdown button and select **Edit**.

    For **Background color**, change the color `Brand` to `Highlight`.

10. Click **Save** (upper right).

[OPTION END]

[OPTION BEGIN [Import Project]]

We really want you to see some of things related to stylizing you can do with SAP Build Apps. But we understand that stylizing a UI may be tedious for some people.

If you really want, you can skip doing the stylizing and instead import the project already stylized. If you want to take this route, do the following:

1. Download the file [`Sales-Order-Trigger.zip.gpg`](https://github.com/sap-tutorials/sap-build-apps/raw/main/tutorials/build-apps-workflow-trigger/Sales-Order-Trigger.zip.gpg).

2. Click the 3 dots next to **History** in the upper-right corner.

     ![Skip UI](replace.png)

3. Select **Replace**.

4. Select the file you downloaded.

5. Click **Replace**

Click **Done** and you can go to the next step.

[OPTION END]





### Enable SAP BTP authentication
You need to enable SAP BTP authentication because you want to use SAP BTP destinations, and users need to be authenticated to use them.

SAP BTP destinations are connections to backend services – each specifies the location of a backend and how the user will be authenticated. The destinations can be used by the various services within SAP BTP, including SAP Build Apps.  

SAP BTP authentication also has the benefit of requiring authentication in your app and reusing the built-in SAP BTP authentication mechanism.

1. Go to the **Auth** tab.

2. Click **Enable Authentication**.

    ![Auth](auth.png)

3. Select **SAP BTP Authentication**.

    On the confirmation popup, click **OK**. 

4. Click **Save** on the upper right corner.







### Create data resource to process
As part of setting up SAP Build Process Automation, you created an SAP BTP destination that points to SAP Build Process Automation APIs, so that you can trigger workflows.

ANd when you created the process and then deployed, you further published it so it can be discoverable by SAP Build Apps.

We will now tell our project to use this process.

1. Open the **Data** tab, at the top of the page.

2. Click **Add Integration**.
   
    ![Add integration](trigger-1.png)

3. Click **Library**.

    ![Open library](trigger-2.png)

4. Click the process you published.
   
    You can see the name of the process, as well as the project name.
   
    ![Select process](trigger-3.png)
 
5. Click **Enable process**.

    ![Enable process](trigger-4.png)

6. Click **Save** (upper right).








### Create page variable
You need a place to put the values the user will enter, so that you can later pass them to the process. For this, you can create a page variable.

1. Back on the UI canvas (click the **UI Canvas** tab at the top), select **Variables**.

2. On the left, click **Page Variables**.

    >If you get a big text box saying **Welcome to Variables**, you can read it but then you can close it by clicking the **X**.

    >![Date variable documentation](data-variable-docs.png)
   
3. Click **Add Page Variable**. 

    Change the name of the variable to **SalesOrderDetails**.

    Make the variable of type **Object**, and remove the **id** field.

    ![Page variable](pagevar-1.png)

4. To the page variable, add fields to the object by doing the following:
   
    - Selecting page variable.

    - Add the name of the new field on the right in **Add New Property**, then press **Enter**.

    - Select the new field, and change its type.

    Do this for the following fields:

    | Name | Type |
    |-----|------|
    | amount | Number |
    | customer | Text |
    | deliverydate | Date text |
    | material | Text |

5. Click **Save** (upper right).







### Bind page variable to UI elements
We created a variable for the sales order data, but now we need to get the data entered by the user into the variable. We do this by binding the variable to UI elements, specifically, to the input boxes.

Whenever someone types into the input box, the value is automatically copied into the variable.


1. Go back to **View** so you can see the UI canvas.
   
2. Click on the first input field (for **Customer**).

    In the **Properties** tab, click the **X** next to the **Value** field, and select **Data and Variables > Page Variables > SalesOrderDetails > customer**.

    ![Binding input field](bind-input1.png)

    Click **Save**

3. Click on the second input field (for **Material**).

    In the **Properties** tab, click the **X** next to the **Value** field, and select **Data and Variables > Page Variables > SalesOrderDetails > material**.

    ![Binding input field](bind-input2.png)

    Click **Save**

4. Click on the third input field (for **Amount**).

    In the **Properties** tab, click the **X** next to the **Value** field, and select **Data and Variables > Page Variables > SalesOrderDetails > amount**.

    Click **Save**

5. Click on the fourth input field (for **Delivery Date**).

    In the **Properties** tab, click the **X** next to the **Value** field, and select **Data and Variables > Page Variables > SalesOrderDetails > deliverydate**.

    Click **Save**

6. Click **Save** (upper right).






### Add logic to trigger workflow
We need to set up the logic so when someone clicks the **Get Approval** button (an event), we send the sales order data to our SAP Build Process Automation workflow using the data resource we previously created. 

1. Click on the **Get Approval** button, and open the logic canvas by clicking **Add logic to Button1** at the bottom right.

    ![Open logic canvas](logic-open1.png)

2. Drag a **Trigger Process** flow function onto the canvas, and connect the component tap event to it.
   
    ![Create record](logic-create-record.png)

3. Click on the **Trigger Process** flow function and configure it in the **Properties** pane on the right.

    For **Process**, this should already be set to **Order Process**, since you have only one process enabled.

    ![Create record binding](logic-create-record-binding.png)
    
    For **Input Parameters**, click **Custom object** and bind the fields for amount, material, custom, and delivery date to the page variable you just created.

    For example, click **Custom object**.

    ![Logic for binding](extra-logic1.png)

    Click on the **ABC** binding type, and select **Data and Variables > Page Variables > SalesOrderDetails > material**, then click **Save**.

    ![More logic for binding](extra-logic2.png)

    Do this for customer (ship to party), amount and delivery date.

    Click **Save**.

4. Drag a **Toast** flow function onto the canvas, and connect the **top** output of the **Trigger process** flow function to it.

    ![Toast](bind-toast.png)

5. Click on the **Toast** flow function and configure it in the **Properties** pane on the right.

    For **Toast message**, click on the **ABC**, and then select **Formula > Formula**.

    ![Toast configure](logic-toast-configure.png)

    Erase the quotation marks, and enter the following formula:

    ```JavaScript
    "Triggered process with ID: " + outputs["Trigger process"].newProcessInstance.id
    ```

    Click **Save**.

6. Click **Save** (upper right).










### Run app
1. Click the **Launch** tab, and then **Open Preview Portal**.

    ![Launch](launch.png)

3. Click **Open web preview** (left).

4. Select your project, **Select Order Trigger**.

    ![Launch preview](launch-preview.png)

5. Enter the following values in your form:

    | Field | Value |
    |-------|-------|
    | Customer | `Joe's Bikes` |
    | Material | `HT-1000` |
    | Amount  | `100000` |
    | Delivery Date  | `2023-03-31` |

6. Click **Get Approval**. When the toast message appears take a screenshot.

Your process should be triggered and require approval (since the amount is 100,000 or above).

You should see the toast message indicating the workflow was triggered, and with the process instance ID.

![Preview toast](launch-toast.png)

You can also see the results of the call in SAP Build Process Automation.

Go to the **Monitoring** tab from the lobby, then under **Monitor** section, access the **Process and Workflow Instances** tile. The first one should be the one you just triggered.

- You can see the new process instance.
- You can see the process ID is the same as in the toast message in the app.
- You can see the context, which is the values sent with the API (4 of them, in yellow, you entered in the input fields and the others were hardcoded in the formula for the **Create record** flow function).
- You can also see the execution log, which shows that the process stopped at the approval step (since it was at least 100,000) and is awaiting approval. If you expand the approval step you can see more information, including who the approval request was sent to.

![Preview in process automation](launch-preview-SPA.png)


