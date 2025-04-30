---
parser: v2
author_name: Daniel Wroblewski
author_profile: https://github.com/thecodester
auto_validation: true
time: 20
tags: [ tutorial>beginner, software-product>sap-build, software-product>sap-build-apps, software-product>sap-build-process-automation]
primary_tag: software-product>sap-build
---
  
 

# 8 - Create a Work Zone Site and Embed Your App
<!-- description --> Create a site for accessing your application using SAP Build Work Zone, standard edition, as part of the SAP Build CodeJam.


## Prerequisites
- You have completed the previous tutorial for the SAP Build CodeJam, [Add Approval Flow to Process](codejam-07-build-deploy-app).





## You will learn
- How to create an SAP Build Work Zone site
- How to embed your shopping app in your site



## Intro



### Add role for SAP Build Work Zone
To enable you to reach SAP Build Work Zone from the SAP Build Loby, you need to add a role to your user.

1. Go back to the BTP Cockpit.

    Go to **Security > Users**.

    Click on your user with **Custom IAS tenant**.

    ![Open user](images/role1.png)

2. In the side panel, click the 3 dots, and then click **Assign Role Collection**.

    ![Assign role collection](images/role2.png)

3. Search for the **Launchpad_Admin** role, select it, and click **Assign Role Collection**.

    ![Launchpad_Admin role](images/role3.png)

    >You must log out of SAP Build and log back in for the new role collection to take effect.




### Open SAP Build Work Zone, standard edition
1. In the SAP Build lobby, click **Create > Create**.

    ![Open Work Zone](images/wz-open1.png)

2. Select **Business Site**, and then click **Next**.

    ![Build site](images/wz-open2.png)

    >If you get a **Not Subscribed** indicator, make sure you have the **LaunchpadAdmin** role and log off and log back into SAP Build.
    >
    >![Not subscribed](images/wz-open2a.png)
    >
    >Log off with the "avatar" at the top right (yours will have your initials).
    >
    >![Logg out](images/wz-open2b.png)


3. Select **Admin Console**, and then click **Open**.

    ![Open Admin console](images/wz-open3.png)

    The Admin Console opens.

    ![Admin console](images/wz-open4.png)



### Create Site

1. Click **Create Site**.

    ![Create site](images/wz-create-site1.png)

2. Enter `Purchasing` as the site name, and click **Create**.

    ![Create](images/wz-create-site2.png)

    The site settings screen opens in a new tab.

3. Click **Edit** in the top-right corner.

    ![Edit](images/wz-create-site3.png)

4. Under **Display**, select **Spaces and Pages - New Experience**.

    ![Select new experience](images/wz-create-site4.png)

4. Click **Save**.

5. Go back to the site directory in the previous browser tab.

    You will see your new site tile.

    ![New site tile](images/wz-create-site6.png)







### Refresh HTML5 apps

1. Open the Channel Manager.

    ![Open channel manager](images/wz-add-app1.png)

2. Click the **Update Content** icon.

    ![Update content](images/wz-add-app2.png)

    It will show **Updating ...** and take a few minutes to update. When **Updated** is displayed, continue to the next step.

 

 


### Add app to work zone
1. Open the Content Manager.

    ![Open content manager](images/wz-add-app3.png)

2. Click **Content Explorer**.

    ![Open content explorer](images/wz-add-app4.png)

3. Click **HTML5 Apps**.

    ![HTML5 Apps](images/wz-add-app5.png)

4. Select your **ShoppingApp**, and then click **Add**.

    You will get a success essage at the bottom of the page. 

    ![Select app](images/wz-add-app6.png)

5. Use the breadcrumb to go back to the **Content Manager**.

    ![Navigate back](images/wz-add-app7.png)

    There you will see your app.

    ![App is added](images/wz-add-app8.png)



### Assign app to everyone
1. In the **Content Manager**, click on the **Everyone** role.

    ![Open everyone](images/wz-everyone1.png)

2. Click **Edit**.

    ![Edit everyone](images/wz-everyone2.png)

3. In the **Assignment Status** column for your app, toggle the switch to on (green).

    ![Turn on app](images/wz-everyone3.png)

    It should then look like this:

    ![App on](images/wz-everyone4.png)


4. Click **Save**.






### Create a page
1. Go back to the **Content Manager** main page with the breadcrumb.

    Click **Create**, and then **Page**.

    ![Create page](images/wz-page1.png)

2. Set the title to **Overview**.

    ![Set title](images/wz-page2.png)

3. Click **Add Section**.

    ![Add section](images/wz-page2a.png)

    Call the section **Purchasing**.

    ![Create section](images/wz-page3.png)

4. Click **Add Widget**.

    ![Add widget](images/wz-page3a.png)

    Select your **ShoppingApp**, and then click **Add (1)**.

    ![Add app](images/wz-page5.png)

6. Click **Save** for the page.

You get a preview of the page.

![Preview](images/wz-page6.png)


### Create a space
1. Go back to the **Content Manager** with the breadcrumb.

    Click **Create**, and then **Space**.

    ![Create space](images/wz-space1.png)

2. Set the title to **Home**.

    ![Set space title](images/wz-space2.png)

3. In the Pages tab, for your page, toggle the **Assignment Status** column to assign (green).

    ![Add page to space](images/wz-space3.png)

4. Click **Save**.




### Add space to Everyone
1. Go back to the **Content Manager** with the breadcrumb.

    Double-click **Everyone** to open it.

    ![Open Everyone](images/wz-role1.png)

2. Click **Edit**.

3. Under **Spaces**, toggle the **Assignment Status** column for your new space, so it is on (green).

    ![Add space to Everyone](images/wz-role2.png)

4. Click **Save**.





### Open your site
1. Go to the Site Directory.

    ![Open Site Directory](images/wz-preview0.png)

2. Open your site.

    ![Open site](images/wz-preview1.png)

    You'll see your site, space, page, and app.

    ![Preview](images/wz-preview2.png)

3. Click the tile, and your app should open.

    You will see at the top the navigation from SAP Build Work Zone to return you to your site.

    ![App preview with navigation](images/wz-preview3.png)

