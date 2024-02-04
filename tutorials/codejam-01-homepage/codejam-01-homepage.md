---
parser: v2
author_name: Shrinivasan Neelamegam
author_profile: https://github.com/neelamegams
auto_validation: true
time: 30
tags: [ tutorial>beginner, software-product>sap-business-technology-platform,software-product>sap-build, software-product>sap-build-apps--enterprise-edition]
primary_tag: software-product>sap-build
---
  
# Create the Product List Page
<!-- description --> Import a skeleton project for the SAP Build CodeJam, and create a simple product list page based on ES5 data.


## Prerequisites
- You have completed the previous tutorial for the SAP Build CodeJam, [Set Up Prerequisites for SAP Build CodeJam](codejam-0-prerequisites).


## You will learn
- How to create a new SAP Build Apps project
- How to import a project
- How to get data from a destination
- How to add components to a page
- How to bind data to UI elements
- How to change component styles
- How to preview your app

## Intro
In this exercise, you will be importing an already created skeleton app to your SAP Build Apps lobby. We've provided the skeleton app so you do not have to create all the UI elements for all the pages.

For this page, the product list page, you will design the entire UI, including styles and layout. For all pages, you will create the data connections, variables, logic, and navigation.

For this product list page, we will connect to the ES5 service to get names, descriptions and images of products. At the end of this exercise, your homepage (product list page) will look something like this.

![Home page](images/restyle-6-preview.png)




### Import the skeleton app project

1. Download this skeleton project from this link [ShoppingApp.zip.gpg](https://github.com/sap-tutorials/sap-build-apps/blob/main/tutorials/codejam-01-homepage/ShoppingApp.zip.gpg).

2. Go to SAP Build lobby and click the **Import** button.

    ![Import button](images/1-import-button.png)

3. Click **Browse files** to select the project file you downloaded. After a few seconds, it will be uploaded.

    ![Upload Project and Import](images/2-browse-files-select-import.png)

    Select the file, and click **Import**.

    The project should now be added to the lobby.
    
    ![Project added](images/3-project-imported-lobby.png)    
    
4. Click the project **ShoppingApp** to open it.




### Enable BTP Authentication
You need to enable SAP BTP authentication because you want to use SAP BTP destinations, and users need to be authenticated to use them.

SAP BTP destinations are connections to backend services – each specifies the location of a backend and how the user will be authenticated. The destinations can be used by the various services within SAP BTP, including SAP Build Apps.

1. With your new project open, go to the **Auth** tab.

2. Click **Enable Authentication**.

    ![Enable authentication](images/4-enable-btp-auth.png)

3. Select **SAP BTP Authentication**.

    ![Select BTP authentication](images/5-select-btp-auth.png)
    
    Click **OK**.

4. Click **Save** (upper right).






### Enable a data entity
Since you have already performed the steps in the prerequisites tutorial, you should have a destination to the ES5 shopping API so you can retrieve a list of products.

1. Open the **Data** tab.

2. Click **Add Integration**.
   
    ![Enable data entities](images/6-datatab-integrations.png)

3. Click **BTP Destinations**.
    
    ![BTP destinations](images/6a-datatab-integrations.png)

5. Find the **ES5-Shop** destination, and click it.

    ![ES5-Shop](images/6b-datatab-integrations.png)

    This will show all the entities available in this service.

    ![Entities](images/6c-datatab-integrations.png)

4. Click **Install Integration** (top right).
   
5. Select the **Products** entity, and click **Enable Data Entity**.

    ![Enable entities](images/6d-datatab-integrations.png)

    You should see the entity as **Enabled**.

6. Click **Save** (upper right).

    Click **Exit** (upper right) to return to the previous – **Data** – screen. 

    You should now see the **Products** entity in the **Data** tab.

    ![Products](images/products.png)




### Create a data variable
Data variables are used for holding data from external data sources, such as SAP systems or third-party APIs. They come with a logic canvas that automatically comes with logic for retrieving the data – you do not have to worry about this logic. You can "bind" the data in the variable with your UI components in order to display the data. 

All this makes it possible to build SAP extensions that interact with and enhance core systems.

1. Switch to the **UI Canvas** tab and then toggle to **Variables**.

    ![Variables](images/9-switch-uicanvas-toggle-variables.png)

    >If you get a big text box saying Welcome to variables, you can read it but then you can close it by clicking the X.

    >![Data Variables](images/9a-data-variable-docs.png)

2. Access the **Data Variables** tab on the left of the screen.

3. Click on **Add Data Variable** and choose **Products**.
   
    ![Add New Data Variables](images/9b-data-variable-new.png)

    Keep the default selections shown on the right pane as it is.

4. Click **Save** (upper right).
   




### Add UI elements to the homepage

1. Toggle back to **View**.
   
    ![View](images/9-switch-uicanvas-toggle-variables.png)

2. From the **Core** tab of the left pane, drag a **Container** component from under **Layout > Container** onto the canvas.

    >Container components let you group components and configure the group of components as a single unit.

    With the container selected, in the **Properties** tab, change the **Component display name** to `Container - Products List`.

    ![Drag Container on Canvas](images/11-drag-container-uicanvas.png)

3. Into this container, drag and drop a **Title** field. The result should look like this:

    ![Drag Title inside container](images/12-drag-title-container.png)

    >It may be easier to drag it into the **Tree** view on the lower right, so you can put it precisely where you want. The **Tree** makes it easier to select specific components and to create a hierarchy of components on the page.

4. Click the **Title** component, and in the **Properties** tab on the right pane, change the **Content** text to `Buy some hardware today!`. 

5. From the **Core > Lists** of the left-side components pane, drag a **Large image list item**  into the **Container - Products List** container (after the title component).

    ![Drag Large image list item inside Container](images/13-drag-largeimagelistitem-container.png)

6. Select the container **Container - Products List** again, open the **Style** tab from the right pane, click the dropdown icon for the **Layout Container**, and click **Edit**.

    ![Layout Container - Edit](images/12a-layout-container-edit.png)

    >Each component has a default style, plus additional alternative built-in styles you can choose. In addition, you can make changes to the current style, which changes the style for the current instance of the component only.

    >If you want, you can update the default style with your changes so it affects all components, or you can save your changes to a new style. 

    Expand the **padding** settings, set the padding on all 4 sides to 24px by clicking each rectangle, going to **Theme** tab, and selecting the **XXL** size.
   
    ![Style Theme - Padding](images/12b-padding-xxl.png)

    Let’s save the style by scrolling up in the **Style** tab, clicking **New Style**, entering **Layout List Container**, and clicking **OK**. This saves the new style in the **Style** tab and now can be used on other containers in your app.
  
    You should be able to see some nice padding around the content.
    
    ![Padding around content](images/12c-nice-padding-around.png)

7. Click **Save** (upper right).






### Bind data variable to UI elements
We created a data variable for the **Products** data and now we need to bind the data from this data variable to the UI components added to the canvas to display the products in the list.

1. Select the **Large Image List Item** on the canvas.
   
2. In the **Properties** tab, set the following by clicking on the icon next to each field:

    ![Binding icon](images/14-icon-opens-modal.png)

    Clicking on these icons shows different options for data binding.

    ![Binding options](images/14a-modal-options.png)

    Do the bindings to the UI elements as follows:

    | Field | Binding |
    |-------|---------|
    | Repeat with | **Data and Variables > Data variable > Products1** |
    | Title label | **Data item in repeat > current > Name** |
    | Description text | **Data item in repeat > current > Description** |
    | Image source | Formula > `'https://sapes5.sapdevcenter.com' + repeated.current.ImageUrl` |

    >The formula for the image may appear in red as an error, but it will still work. Go ahead and save it!

    >One of the strengths of the formula editor is that it checks the data types of the properties you are setting to try to determine if the formula is compatible. Here, it is expecting a value defined as an image URL, but our text – which is a valid URL – will work just as well.

3. Click **Save** (upper right).

You may now test the app to see the Products list.






### Test the App

1. Go to the **Launch** tab, then **Open preview portal** Portal.

    ![Launch Page](images/17-launch-tab.png)

    Click **Open web preview**.

    ![Open Web Preview](images/17a-open-web-preview.png)

    You will get a list of apps in your lobby – at his point you should have just one. 
    
    ![Open App Tile](images/17c-select-codejamapp-silver.png)
    
    There is a search bar to help you find your app when you become a master and build dozens and dozens of apps.

2. Click the **ShoppingApp** tile.
   
    You should be able to view the list of products.
   
    ![List of Products](images/18-view-product-list.png)





### Restyle the image
In the preview, you might have noticed that the product images are a little large for the space provided. Let's fix this.

1. In your project, click the **UI Canvas** tab.

    Double-click the **Large Image List Item** component. 

    ![Restyle](images/restyle-1.png)

    This opens the Component Style Editor for the **Large Image List Item** component, a "custom" component built from many other basic components (more on custom components later). In this editor, you can access all the subcomponents and control their styles.

2. Select the **Image** component (either on the canvas or in the **Tree** view).

    ![Image](images/restyle-2.png)

3. In the **Layout** tab on the right, do the following:

    - Change **Right gap** to 24px.
  
        ![Right gap](images/restyle-3.png)

    - Under **Width and Height**, change the **Custom** values to **Set width** and **Set height**, and set them both to 64px. Delete all the other values so it looks like this:

        ![Width and height](images/restyle-4.png)

    - Under **Position** (scroll down some more in the **Layout** tab), set the left spacing to **L 16px**.

        ![Left spacing](images/restyle-5.png)
        
4. Click **Exit** to exit the Component Style Editor.

5. Click **Save** (upper right). 

    Once you click **Save**, if you have a tab where you previously opened a web preview of your app, that tab will automatically update – no need to go back to the **Launch** tab. Instead of opening up a new web preview, you can just navigate to that preview tab and see the updates.

    Navigate back to the preview tab, and you should see the app updated with the improved spacing.

    ![Preview](images/restyle-6-preview.png)





   
    



