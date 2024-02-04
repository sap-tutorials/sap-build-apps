---
parser: v2
author_name: Daniel Wroblewski
author_profile: https://github.com/thecodester
auto_validation: true
time: 10
tags: [ tutorial>beginner, software-product>sap-business-technology-platform,software-product>sap-build, software-product>sap-build-apps--enterprise-edition, software-product>sap-build-process-automation]
primary_tag: software-product>sap-build
---
  

# Expand Process to Add Rework Flow
<!-- description --> Use a custom variable and Go-To Step to expand your workflow so the approver can ask the submitter to rework it, as part of the SAP Build CodeJam.



## Prerequisites
- You have completed the previous tutorial for the SAP Build CodeJam, [Create Action to Post Data to CAP Service](codejam-09-action-post).





## You will learn
- How to define and set custom variables
- How to use the Go-To Step
- How to modify labels on **Approve** and **Reject** buttons of an approval form.
- How to create a rework flow




## Intro
In order for you to learn about custom variables and Go-To steps, you will expand slightly the process you created to include a rework flow – the ability for approval to reject a request but allow the submitter to modify it.

Here is the current process, annoted with what you will do.

![Overview](overview.png)

Here's the flow:

- The **approval** can approve or reject the request, as before. But the approver can also reject the request but indicate that they will allow the submitter to modify the request.
- The **submitter** will get a form to modify the request. In this case, the submitter can now add an explanation for the request.
- The **submitter** can choose to provide an explanation and return the form to the approver, or can simply cancel the request.
- The **approver**, if they get a reworked form, can again choose to approve, reject, or reject and return the form to the submitter.






### Add custom variable
The approver has a field in the approval form to add **Additional Information** when sending the request to the submitter for rework.

The problem is we want this field to be sent to the submitter, bu also returned back to the approval form so the approver knows what they asked for from the submitter. The problem is that the approval form cannot bind a field o one of its own fields.

To solve this, we create a custom variable.

1. Open the process, and open the side panel.

2. Click **Variables**, and then open **Cusom Variables**.

    ![Custom variables](custom1.png)

3. Next to **Custom Variables**, click **Configure**.

4. Click **Add Variable**, and enter `Additional Information` for the **Name**. The identifier is filled in automatically, and leave the type.

    ![New variable](custom2.png)
    
    Click **Apply**.

5. Close the side panel.







### Modify approval form
You need to modify the approval form to enable the approver to indicate that they want to allow rework, and to add a field to show the explanation from the submitter.


1. Click the three dots next to **Approval Form**, and click **Open Editor**.

    ![Open editor](approval1.png)

2. Just before the **Additional Information** field, add a checkbox and call it `Rework`.

    ![Rework](approval2.png)

3. Just after the **Additional Information** field, ad another text area, and call it `Explanation`. Set it to **Read Only**.

    ![Explanation](approval3.png)

4. Click the **Button Settings** icon at the bottom of the form. 

    Under **Reject**, select from the dropdown **Custom**, and enter in the box `Reject/Rework`.

    ![Button texts](approval4.png)

5. Click **Save** (upper right).





### Add condition
Now you need to check whether the approver enabled rework. For this, you add a condition step.

1. Under **Reject** under the approval form, click the plus sign, **+**, and select **Flow Logic > Condition**.

    ![Condition](condition1.png)

    This adds a condition.

    ![Condition result](condition2.png)

2. In the side panel, change the name to `Rework?`. 

3. Click **Open Condition Editor**.

4. Click in the first field, and under **Approval Form**, select **Rework**.

    ![Rework field](condition3.png)

    Click the last field, and select **true**.

    ![True](condition4.png)
    
    Click **Apply**.







### Add rework form
Now you need to provide a form for the submitter to provide an explanation. Since the submitter wants to see all the same information that the approver got to see, we start by duplicating the approval form.


1. In the **Overview** tab, click the three dots next to the **Approval Form** artifact, and select **Duplicate**.

    ![Duplicate](reworkform1.png)

    In the dialog, change the name to `Rework Form`, and click **Duplicate**.

    ![Name form](reworkform2.png)

2. The data at the top of the form can be kept as is, but we have to not allow the submitter to change the **Additional Information** but we do have to allow them to change **Explanation**.

    Click the **Additional Information** text area, and on the right make it **Read Only**.

    ![Field read only](reworkform3.png)

    Click on the **Explanation** text area, uncheck **Read Only**, and check **Required**.

    ![Field required](reworkform4.png)

    Finally, click on the three dots next to the **Rework** checkbox, and click **Delete**.

    ![Delete checkbox](reworkform5.png)

3. Click the **Button Settings** icon at the bottom of the form. 

    Under **Reject**, select from the dropdown **Custom**, and enter in the box `Retry`.

    Under **Reject**, select from the dropdown **Custom**, and enter in the box `Cancel Request`.

    ![Buttons](reworkform6.png)

4. Click **Save** (upper right).









### Add go-to step
1. Back in the process, Under **If** under the **Rework?** condition, click the plus sign, **+**, and select **Approval > Rework Form**.

    ![Add rework form](process1.png)

2. In the side, panel, under **General** set the following bindings:

    | Field | Value |
    |-------|-------|
    | Subject | `This request needs rework - ` and then select **Process Inputs > Order ID** | 
    | Recipients | **Process Metadata > Process Started By** |

2. In the side, panel, under **Inputs** set the following bindings:

    | Field | Value |
    |-------|-------|
    | Additional Information | **Approval Form > Additional Information** | 
    | BP Grouping |  |
    | Business Partner |  | 
    | Business Partner Full Name |  |
    | Order ID | **Process Inputs > Order ID** |
    | Order Items | **Process Inputs > Order Items**  |

3. Under **Approve** under the **Rework Form**, click the plus sign, **+**.

    Select **Controls and Events > Go-to Step**.

    Select **Approval Form**. 

    The flow diagram will update to show you the revised flow.


4. Select he **Approval Form**.

    Under **Inputs**, bind the **Additional Information** field to **Custom Variables > Additional Information**. 

    Under **Outputs**, select the **Additional Information** custom variable, and then set it to **Approval Form > Additional Information**.





### Test app
NEEDS UPDATING

Go into your app, select the **Notebook Basic 18**, and add it to your cart.

Go to the **Cart** page (left-side menu), and click **Purchase**.

Your process should now be triggered. You can see the new order in the CAP service.

![New order](images/test-2.png)

Open the Inbox from the SAP Build lobby, and see the approval form from the order you just created.

![Approval form](images/test-3.png)

Click **Approve**. Refresh the Inbox and you will get the approve notification. YOu can click **Submit** to complete the process.

This is similar to the previous test, except now the CAP service has been updated. If you view your order in the CAP service, you will see it is now approved.

![Updated](images/test-4.png)