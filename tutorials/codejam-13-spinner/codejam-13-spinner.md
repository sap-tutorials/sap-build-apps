---
parser: v2
author_name: Ian Thain
author_profile: https://github.com/ithain
auto_validation: true
time: 10
tags: [ tutorial>beginner, software-product>sap-build, software-product>sap-build-apps--enterprise-edition, software-product>sap-build-process-automation]
primary_tag: software-product>sap-build
---
  

# 13 - Implement Spinner for Product Details Page 
<!-- description --> Set up and use a spinner control to hide UI until all data is available to display, as part of the SAP CodeJam.



## Prerequisites
- You have completed the previous tutorial for the SAP Build CodeJam, [Enable App to Filter Products](codejam-12-filtering).





## You will learn
- How to set up and use a spinner control
- How to delay displaying data until it is retrieved




## Intro
As you know your app has a product list and product details pages, but have you noticed while testing today that for a brief moment, after selecting a product, you can see empty or null fields on the product detail page?

![Product Detail page](images/empty-details.png)

In this exercise, you will enhance the **Product details** page by adding a spinner and only showing the fields once they are populated with the data.






### Add spinner variable
1. Go to the **Product Details** page.

2. Toggle to **Variables**.
    
3. Create a new page variable, and set its properties to the following:
    
    | Name | Type |
    |-------|-------|
    | **Variable name** | `showSpinner` | 
    | **Variable value type** | True/false |
    | **Initial value** | True |

    ![Page Variables on Product Details](images/spinner-var.png)






### Add spinner to UI
1. Toggle back to **View**

2. Drag the spinner control from the **Core** components tab – currently the very first component – and place it on the page.
   
    ![Placing the Spinner](images/add-spinner.png)
   
    >It will be easier to place it by dragging and dropping on the UI canvas

   
3. With the spinner selected, go to the **Advanced Properties > Visible** property.

    Click the binding icon, and set the binding to **Data and Variables > Page Variable > showSpinner**.

    This is the variable you created in the previous step.
   
    ![Visible Property under Advanced](images/show-spinner.png)






### Set visibility of UI
1. In the Tree View, select the **Product Details** container.

2. Under **Advanced Properties**, set the **Visible** property to a **formula**, and set the formula to the following:

    ```JavaScript
    !pageVars.showSpinner
    ```
   
    ![Visible Property under Advanced](images/visibility-data.png)

    This will set it to the opposite of the spinner. So when the spinner is visible, the product details UI is not, and vice versa.





### Add logic to hide the spinner
1. Toggle to **Variables**.

2. Select **Data Variables**, then select **Products1**.

    Open the logic canvas.

3. In the logic canvas, add a **Set page variable** flow function to the end of the flow.

    Set **Variable Name** to **showSpinner**.

    Set **Assigned value** to **False**.
   
    ![Visible Property under Advanced](images/data-hide-spinner.png)


4. Click **Save** (upper right).





### Test the app
1. Launch the app.

2. Select a product, which will take you to the **Product Details** page.

    ![Select product](images/IMG_5473.PNG)

3. On the **Product Details** page, you will first see the spinner and nothing else on the page.
   
    ![Spinner displayed](images/IMG_5479.PNG)

    When the data is returned, the spinner will disappear and the product details will be displayed

    ![UI displayed](images/IMG_5480.PNG)

