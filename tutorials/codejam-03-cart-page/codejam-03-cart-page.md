---
parser: v2
author_name: Shrinivasan Neelamegam
author_profile: https://github.com/neelamegams
auto_validation: true
time: 20
tags: [ tutorial>beginner, software-product>sap-business-technology-platform,software-product>sap-build, software-product>sap-build-apps--enterprise-edition]
primary_tag: software-product>sap-build
---
  

# Configure the Shopping Cart Page
<!-- description --> Set up the shopping cart page – including variables, binding, styling, and logic – as part of the SAP Build CodeJam.


## Prerequisites
- You have completed the previous tutorial for the SAP Build CodeJam, [Configure the Product Details Page](codejam-02-product-detail-page).


## You will learn
- How to setup a destination to a CAP service
- How to connect to a CAP service
- How to set up filter condition on a data variable
- How to bind repeating data to the UI elements
- How to add a navigation menu





## Intro
In this exercise, you will be enhancing the **Cart** page of the SAP Build CodeJam app with data and functionality.

Specifically, you will retrieve data from our CAP service – which maintains the data for our cart and later for the order once it is requested – and bind that data to your UI components. Also, you will implement logic for deleting data from the CAP service.






### Create a destination for our CAP service
To save you time we have given you almost all the information for the destination in a file you can import.

>**IMPORTANT: ** Before you begin this step, you will need to get the URL for the service from the CodeJam instructor.


1. Download the destination definition file [`CodeJamOrdersService`](https://github.com/sap-tutorials/sap-build-apps/raw/main/tutorials/codejam-0-prerequisites/CodeJamOrdersService).

2. In the SAP BTP cockpit, click **Connectivity >  Destinations**.

    <!-- border -->
    ![Open destinations](3-open-destinations.png)

3. Click **Import Destination**, and then select the `CodeJamOrdersService` file you downloaded.

    The draft destination will be filled in except for the URL.

    <!-- border -->
    ![New destination](4-create-destination.png)


4.  Enter the URL from your instructor.

    Click **Save**.

5. Test the connectivity to the new destination by clicking **Check Connection**.
   
   You should receive **200 – OK**. 









### Create data resource for CAP service
In order to fetch data from the CAP service, we need to create a data resource for the CAP service.

1. Back in your **ShoppingApp** project, open the **Data** tab.

    Click **Add Integration**.

    ![Data resource](images/resource-1.png)

2. Click **BTP Destinations**, and then select the **CodeJamOrdersService** destination.

    ![Destination](images/resource-2.png)

    You will see a list of entities for this OData service.

    ![Entities](images/resource-3.png)

3. Click **Install Integration**.

4. With the `Orders` entity selected, click **Enable Data Entity**.

    ![Enable entity](images/resource-4.png)

5. With the `OrderItems` entity selected, click **Enable Data Entity**.

6. Click **Save** (upper right).

7. Click **Exit** to return to the previous screen. 
   
You should now be able to see **Orders** and **OrderItems** appearing under the **Integrations > CodeJamOrdersService** section.

![New entities](images/resource-5.png)






### Create data variable
Now that you we have defined the connection to the CAP service, let's create a data variable, which will automatically include the logic for making the call as well as holding the retrieved data.

1. Navigate to the **Cart** page by clicking on the **Home pages** link on the upper-left corner, below the app name.

    Select the **Cart** tile.

    ![Cart page](images/data-1.png)

2. Toggle to **Variables**.

3. Click **Data Variables** on the left side. 

    Click **Add Data Variable** and choose **OrderItems**.

    ![Add New Data Variables](images/1b-data-variable-new.png)
   
    With **OrderItems** data variable, selected, in the right pane, the type should already be set to **Collection of data records**. Keep this type.

    ![Alt text](images/1d-data-variables-orderitems.png)

4. Select the **X** next to **Filter condition**, then select **Object with properties**.

    ![Object with properties](images/1e-data-variables-object-properties.png)
   
5. Click **Add Condition**.

    ![Add condition](images/1f-add-condition.png)

    Set up the condition as follows:
   
    | Property | Condition type | Compared value |
    |----------|----------------|---------------|
    | **order_ID** | **equal** | `6c25e827-15c2-4e7f-be1a-89fb4304d4fa`  |

    >**IMPORTANT:** The order ID represents some dummy data we entered into the service so you will see some data in the UI. Later, you will update the condition so you only see your own data.
    >
    >DO NOT delete any order items.

    Click **Save**.

5. Click **Save** (upper right).
   






### Bind variables to cart items
In order to display the data in the cart from the `OrderItems` entity, you need to map the backend data that is in the data variable to the UI components.


1. Toggle back to **View**.
   
2. Select the **Text - Product** text component (not the header). 

    You can click on the text component on the canvas, or you can select it under the Tree View: **Row - Cart Item > Cell #1 > Text - Product**.

    ![Bind product](images/bind-1.png)

    Once selected, in the **Properties** pane, click the binding icon for the **Content** property.

    Choose **Data item in repeat > current > product**.

    Click **Save**.

3. Repeat the binding for **Quantity**, **Price** and **Total** as follows:

    | UI Component    |   Tree View Location  |   Binding  |
    | --- | --- | --- |
    | Quantity    |    Cell #2 > Text - Quantity   |  Data item in repeat > current > quantity    |
    | Price    |   Container 10 > Text - Price   |  Data item in repeat > current > price   |
    |  Total   |   Container 11 > Text - Total |   Data item in repeat > current > total   |

4. Select the **Cart Total: $** field and click the binding icon for its **Content** property.

    ![Cart total binding](images/bind-2.png)

    Instead of **Static value**, change the binding type to **Formula** and replace the formula with the following formula:
    
    ```JavaScript
    "CART TOTAL: $" + FORMAT_LOCALIZED_DECIMAL(NUMBER(SUM(MAP(data.OrderItems1,item.price * item.quantity))), "en", 2,2)
    ```

    Click **Save** twice. 


5. Click **Save** (upper right).




### Add navigation menu item
Now that we have set up the **Cart** page, we need to let users get to the page. There are 2 ways users can navigate between pages.

We can can create a button or other UI component, and then create logic to respond to a button click or other event, and then use the **Open Page** flow function. That is what we did to navigate from the product list page to the product detail page.

But users might want to navigate to the **Cart** page at any time. So we can create a static menu that is available all the time. Every new app comes with a menu and a single item to return to the home page. In this step, you will create a new menu item for the **Cart** page. 

1. Click the **Navigation** tab.

    You will see the **Home page** menu item.

    ![Navigation](images/nav-1.png)

2. Click **Add item**.

    This will add a menu item, but likely not to the page we want.

3. Select the new menu item, and set the following properties:

    | Icon    |   Tab name  |   Page  |
    | --- | --- | --- |
    | `shopping-basket`    |   `Cart`     | `Cart` |

    ![Cart](images/nav-2.png)

4. Click **Save** (upper right)







### Test the app
Return to your web preview.

>Once you saved your app, the page should refresh and you should see the **Cart** menu item below the Homepage icon on the left navigation menu.

Click **Cart**.

The **Cart** page should open and you will see the order items for the dummy cart. 

![Dummy cart](images/test-1.png)










