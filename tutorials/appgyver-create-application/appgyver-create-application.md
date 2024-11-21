---
auto_validation: true
time: 10
tags: [ tutorial>beginner, topic>mobile, software-product>sap-business-technology-platform]
primary_tag: software-product>sap-build-apps
author_name: Daniel Wroblewski
author_profile: https://github.com/thecodester
parser: v2
---
  
# Create an Application with SAP Build Apps
<!-- description --> Create an application with SAP Build Apps on SAP BTP. The application, created in a browser, can be used with the mobile preview app to scan physical barcodes on food packaging to display calorific information.


## Prerequisites
- Access to an SAP Build Apps system – follow these instructions to install SAP Build Apps on your trial system. 
    - For video instructions, see the following: <div>&nbsp;</div><iframe width="560" height="315" src="https://www.youtube.com/embed/ZpQM2B1v2GY" frameborder="0" allowfullscreen></iframe>
    - For written instructions, do ONLY **Step 2** of [Set Up Prerequisites for SAP Build CodeJam](https://developers.sap.com/tutorials/codejam-0-prerequisites.html#76cc6857-c70a-4e56-a03a-41e6043a67fe) **→ YOU ONLY HAVE TO DO STEP 2 (install SAP Build Apps) OF THIS TUTORIAL!!!**
- Download the SAP Build Apps Previewer App on a smart phone or tablet: [iOS](https://apps.apple.com/us/app/sap-appgyver-preview/id1585856868) / [Android](https://play.google.com/store/apps/details?id=com.sap.appgyver.preview.release).


## You will learn
  - How to create a no-code project with SAP Build Apps
  - How to add and edit visual elements in the SAP Build Apps Composer tool

## Intro
>**IMPORTANT:** Make sure to follow the above prerequisites for installing SAP Build Apps -- the booster will install SAP Build Apps and the Cloud Identity Service, as well as assign you the needed roles. 

In this mission you will learn how to create a no-code application using the SAP Build Apps Composer tool. The application you create will enable you to scan a barcode on a smartphone and retrieve information from a public API. To do this, you will need to download the Preview app (available through [iTunes](https://apps.apple.com/us/app/sap-appgyver-preview/id1585856868) and [Google Play store](https://play.google.com/store/apps/details?id=com.sap.appgyver.preview.release)).

The application you'll create across the tutorials will read barcodes from food packaging and display information about the product using the Open Food Facts API:

![Diagram of scanning a food item from a mobile app](OpenFoodFactsDiagram.png)

>**IMPORTANT:** Though you can use any SAP Build Apps version, these tutorials are based on a SAP BTP trial account.

---

### Did you set up SAP Build Apps correctly?
Yes, no one reads the documentation.

So you decided to skip the instructions and the prerequisites and set up SAP Build Apps by yourself, like a homeowner thinking they can save money by fixing their own plumbing. Ever see the movie "The Money Pit" with Tom Hanks and Shelley Long. It did not go well. 

![The Money Pit](moneypit.png)

SAP Build Apps is simple to install, but does require you to use a custom identity provider, so it is worth taking an extra 2 minutes to make sure everything is fine. We have tried to make it as simple as possible by giving you a choice:

- [2-minute video](https://www.youtube.com/watch?v=ZpQM2B1v2GY)
- [Written instructions](https://developers.sap.com/tutorials/codejam-0-prerequisites.html#76cc6857-c70a-4e56-a03a-41e6043a67fe) – the written instructions are in a single step -- **Step 2** -- in our SAP Build CodeJam prerequisites tutorial. **You only have to do Step 2, Install SAP Build Apps**.

>While we're on the subject of CodeJams, we would like to bring the SAP Build CodeJams to you –- either the beginner workshop or the advanced-topics workshop. If you are interested, see our [blog](https://community.sap.com/t5/sap-codejam-blog-posts/so-you-want-to-host-a-codejam-everything-you-need-to-know/ba-p/221415) about CodeJams and add a comment that you want to host a CodeJam.

>![SAP Build CodeJam](IMG_7196small.JPG)


### Create SAP Build Apps project

1. Open the SAP Build lobby in your trial account by doing the following:

    - Open the SAP BTP cockpit by going to [https://account.hanatrial.ondemand.com/cockpit](https://account.hanatrial.ondemand.com/cockpit).

    - Click **Go to Your Trial Account**.

    - Under Subaccounts, click the **trial** tile.     

        ![Subaccount](start1.png)

    - **Under Instances and Subscriptions**, click the button next to SAP Build Apps.
    
        ![Instance](start2.png)
        
        This opens the SAP Build lobby.

        ![SAP Build lobby](start3.png)

2. Within the SAP Build lobby, click **Create**.
   
    ![Create](startnew1.png)
   
    Click **Build an Application**. 
   
    ![Create](startnew2.png)

    Click **SAP Build Apps**. 
   
    ![Create](startnew2a.png)

    Click **Web & Mobile Application**. 

    ![Create](startnew3.png)

3. Enter `Scanner Application` for the project name, and an optional description, then click **Create**.

    ![Create](startnew4.png)

    Your SAP Build Apps project is created, and the default **Home page** is displayed. Once created, projects can be accessed again at any time from the SAP Build lobby.

    ![New project](startnew5.png)

4. Since we want to create a mobile app, open the device dropdown and select **Mobile**.

    ![Mobile](startnew6.png)





### Understand SAP Build Apps

![Editor](composerPro.png)

When working with SAP Build Apps, the majority of your time will be spent in the app builder area. This area allows you to complete key tasks such as, but not limited to, the following:

- Define your app's structure and navigation logic

- Build pixel-perfect user interfaces

- Create complex logic with visual programming

- Integrate with external data resources

- Bind data to your components to create dynamic views, and more

For more detailed coverage of the features available in the app builder area, view the [SAP Build Apps documentation](https://help.sap.com/docs/BUILD_APPS/431746e4c663458aa68d9754b237bfc6/daece9f87abf4f7187a14ae0b1f8b2ab.html).



### Edit app interface
You'll now start to create a basic layout for your application, starting with editing text.

1. Select the **Title** component (the one that says **Headline**) by clicking it once.

    ![Select title](select1.png)

    Click the text to select the text.

    ![Select text](select2.png)

    Copy-and-paste or type in the following:

    ```Text
    Barcode Scanner
    ```

    ![Paste text](select3.png)

2. Double-click the **Text** component to highlight the text, and paste in the following:

    ```Text
    Scan a barcode of a food product using your smartphone
    ```

    ![Text field](select4.png)

    >Instead of editing text directly inside a component, you can also set the text using the **Content** property in the **Properties** tab to the right. This option is needed if you set the text based on a formula or variable.




### Add scan button

Next, you'll need to add a scan button which, when tapped, will open the camera device on your smartphone.

1. Locate the **Button** component (found under **Core > Forms**) and drag and drop this underneath the paragraph/text field.

    ![Adding a button](AddButton.png)

2. Edit the button label by clicking the word **Button** inside the button and typing `Scan`.

    ![Edit button text](EditButtonText.png)

3. Click **Save** (upper right).

    ![Save Project](SaveProject.png)

The application is now saved and available to preview using the preview app on your smartphone.

 

### Preview app on your device
>👉 **IMPORTANT:** Depending on which SAP Build Apps you are using – with the community edition or SAP BTP trial -- you will select different buttons to get started with the preview app.

>- If you are using the community edition, the first thing you do is click the **SAP Build Apps** tile.

>- If you are using SAP BTP trial (region US20), you will click **Other login options**.

>The instructions below assume you are using a trial account, but also provide alternatives if you are using an SAP BTP account in region EU10.


1. Assuming you are using an SAP BTP trial account, open the SAP Build Apps preview app on your mobile device, and click **Other login options**.

    ![Open preview app](preview1.PNG)

    Select the US10 region from the dropdown list.

    ![Select US10](preview2.PNG)
    
    This will display a pin code.

    ![Pin code](preview3.png)  

    >If you are using an account in the EU10 region, from the first screen simply click the **SAP Build Apps** tile and this will display a pin code.


2. Go back to SAP Build Apps on your desktop, and open the **Launch** tab.
    
    ![Launch](launch1a.png)
    
    Click **Open Preview Portal**.

3. Enter the pin code, and press **Enter** or click **Confirm pin**.

    ![Alt text](Launch2a.png)

    This will refresh the preview app and display a list of your apps. 

4. Click **Open** for the `Scanner Application` project.

    ![My apps](IMG_3953.PNG) 

    Your application starts to run.

    ![Preview of app](IMG_5465.PNG)

