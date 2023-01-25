---
parser: v2
author_name: Daniel Wroblewski
author_profile: https://github.com/thecodester
auto_validation: true
time: 20
tags: [ tutorial>beginner, software-product>sap-business-technology-platform,software-product>sap-build, software-product>sap-build-apps--enterprise-edition, software-product>sap-build-process-automation]
primary_tag: software-product>sap-build
---
 

# Create the Product Page UI with Ratings/Comments
<!-- description -->Create the user interface for the product page, which shows the product and lets users add a rating and comments. 

## You will learn
- How to create a form with containers
- How to stylize components
- How to trigger a custom event
- How to paginate data

## Intro
In the previous tutorial, you set up all of the data variables and global logic for the page. In this tutorial, you will create the actual UI, plus any required additional logic.



### Display product name and current rating
1. Go back to the Product Page page.

2. Drag onto the canvas a container, and in the container add title, star rating (in **By Me** tab), and text components.

    ![Container tree](part1-container1.png) ![Container UI](part1-container2.png)

3. Set the container layout to horizontal, and set the align components to middle.

4. For the title, set the **Content** to **Data and Variables > Data Variables > Products1 > Productname**.

    - Set its width to **Fit to content**.

5. For the star rating:

    - Set **Maximum value to 5.
    - Set the **Value** to the following formula:
        ```JavaScript
        DEFAULT(FIND_BY_KEY(appVars.Ratings, "productID", STRING(params.productID)).avg,0)
        ```
    - Set its width to **Fit to content**.
    - Set **View only?** to true.

6. For the text component, set the **Content** to the following formula:

    ```JavaScript
    FORMAT_LOCALIZED_DECIMAL(DEFAULT(FIND_BY_KEY(appVars.Ratings, "productID", STRING(params.productID)).avg,0), "en", 1,1) 
    ```





### Let users rate product
1. Add a List divider component next onto the canvas.

    Double-click the list divider, which opens the style editor. Change the following properties under **Style**:

    - **Border color** to Positive (under **Follow smart content color**)
    - **Border width** to **Thick**
    - Click **Exit**.

2. Copy and paste the list divider so you have 2 lines in a row.

    >The lines look like boxes but will be displayed as lines.

3. In between the dividers, add a container, with a text and star rating component, so it looks like this.

    ![Let user rate](RatingComponent.png)

    Change the layout of the container to horizontal.

4. For the text box, change the width to **Fit content**.

    Edit the current style, and change **Typography > Text size** to small text.

    Set the Content to `Rate this product`.

5. For the star rating, change the following bindings:

    | Field | Value | 
    |-------|---------|
    | Maximum value | 5 | 
    | Value | **Page variable > rating** | 
    | View only? | **Page variable > RatingComplete** | 

6. Click on the star rating, and open the logic pane. 

    Change the event to **Component: Star rating > Property 'Value' changed**.
    
    Add flow functions after the event so the logic pane looks like this:

    ![Logic to add rating](Rating-logic-overview.png)

7. Set the bindings for the flow functions.

    - For **If condition**, set the condition to the following formula:
  
        ```JavaScript
        pageVars.rating != 0
        ```

    - For **Create record**, set the resource to Rating.

        For the Rating record, click **Custom object** and set the following:

        | Field | Value | 
        |-------|---------|
        | Rating | **Page variable > rating** | 
        | RatingBy | Any text (I wrote static text `daniel`). In the future, you would get the current user and bind that. | 
        | ProductID | **Page parameter > productID** | 
        | DateCreated | Formula `NOW()` | 

        ![Create record](Rating-create-record.png)

    - For **Set page variable**, set the variable **RatingComplete** to true.

    - For **Execute cloud function**, set the function to AverageRating.

    - For **Set app variable**, set the **Ratings** variable to the following formula:

        ```JavaScript
        outputs["Execute cloud function"].values.ratings
        ```

8. Click **Save**.

9. Click **Save** (upper right).

You can now test this functionality. Open the app, navigate to the product page of one of the products, and give it a rating.

![Rating test](Rating-test.png)

After clicking a rating of 4, the overall rating of the product changes to 4. At the same time, the rating component is disabled so you cannot rate the product again.

If you want, you can go back to the product list, then navigate again to this product, and give another rating, and see what happens.

![Rating test](Rating-test2.png)







### Create form to add a comment
1. Under the second list divider add the following:
   
   - Text component
   - Input field
   - Button

    ![Comment form](comment-form.png)

2. For the text field, change the text to `Add Comment`.

    In the Layout tab, change the bottom gap to 1px.


3. For the input box, do the following:

    - Remove the **Label** text.
    - Bind **Value** to **Page variable > newcomment**.

4. For the button, change the **Label** to `Post`.

5. Select the button, and open the logic pane.

    Set up flow functions as follows:

    ![Comment logic](comment-logic.png)

6. Configure the flow functions as follows:

    - For the Create record:

        Set the resource to **Comment**, and then set the **Comment** record as follows:

        For the Comment record, click **Custom object** and set the following:

        | Field | Value | 
        |-------|---------|
        | Comment | **Page variable > newcomment** | 
        | ProductID | **Page parameter > productID** | 
        | CommentedBy | Any text (I wrote static text `daniel`). In the future, you would get the current user and bind that. | 
        | DateCreated | formula `NOW()` | 

        ![Create record](Rating-create-record.png)

    - For the bottom Set page variable:
  
        Set the variable to **newcomment**. Leave everything else alone so that it gets set to an empty string.

    - For the top Set page variable:

        Set the variable to **pagesize**, and set the assigned value to the following formula:
        
        ```JavaScript
        pageVars.pagesize + 1
        ```

    - For the Trigger event, set the **Triggered event** to **Product Page / Refresh Comments** event.

7. Click **Save**.






### Display comments
1. Below the button, add the following:

    - Text component
    - Container
        Inside the container add 2 text components.
    - Button (under the container)

    In the Tree view, it should look like this:

    ![Tree for comment list](commentlist-tree.png)

2. For the first text component:

    - Set the **Content** to `No posts yet`.
    - Set **Visible** (iun the Advanced properties) to the formula:

        ```
        IS_EMPTY(data.Comment1)
        ```

3. For the container:

    - Edit the **Layout Container** style and change padding to 4px all around.
    - Under Effects, enable the shadow, set shadow to `Content Shadow 1` and set the shadow color to `#AF9E8D`.
    - Under **Properties** tab, set **Repeat with** to **Data variable > Comment1**. 

4. For the first text component in the container:
    - Set the text size to small.
    - Set the Content to the following formula:

        ```JavaScript
        FORMAT_DATETIME_LOCAL(repeated.current.dateCreated, "YYYY-MM-DD HH:mm:ss") + " - " + repeated.current.commentedBy
        ```

5. For the second text component in the container:

    - Set the Content to **Data item in repeat > comment**.

6. For the button:

    - Set Content to `More`.
    - Set **Visible** to the following formula:

        ```
        pageVars.totalcount>pageVars.pagesize
        ```

7. Select the **More** button, and open the logic pane.

    - Add **Set page variable**, and set **pagesize** to the following formula:

        ```
        pageVars.pagesize + 3
        ```

    - Add a Trigger event and set the **Trigger event** to **Product Page / Refresh Comments** event.

    ![More button logic](more-logic.png)



### Run the app
Go to the Launch tab, and click Open Preview Portal.

Click **Open wen preview**, and select the Social media project.






