---
parser: v2
author_name: Ian Thain
author_profile: https://github.com/ithain
auto_validation: true
time: 30
tags: [ tutorial>beginner, software-product>sap-business-technology-platform,software-product>sap-build, software-product>sap-build-apps--enterprise-edition]
primary_tag: software-product>sap-build
---
  

# Set Up Prerequisites for SAP Build CodeJam
<!-- description --> Get ready for your SAP Build CodeJam now!

## Prerequisites
- Do this before you come to CodeJam ⏱️
- Bring your laptop 💻
- Bring your device 📱
- Bring your brain 🧠
- Be ready to have FUN! 🤗






## You will learn
- How to set up the SAP BTP trial account
- How to set up SAP Build Apps
- How to set up SAP Process Automation
- How to get access to and create a destination for the SAP Gateway Demo System (ES5)







## Intro






### Create SAP BTP trial account
If you do not already have an SAP BTP Trial account, follow this tutorial: [Get a Free Account on SAP BTP Trial](https://developers.sap.com/tutorials/hcp-create-trial-account.html)

<!-- border -->
![Get ES5 account tutorial](BTPTut.png)






### Install SAP Build Apps
To install SAP Build Apps on your SAP BTP trial account, watch and follow along with this video from **Daniel Wroblewski**.

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZpQM2B1v2GY" frameborder="0" allowfullscreen></iframe> 






### Install SAP Build Process Automation
To install SAP Build Process Automation, watch and follow along with this video from **Daniel Wroblewski**. 

> STOP the video at **1:10 mins**. We do **NOT** want you to install Desktop Automation bots etc.

<iframe width="560" height="315" src="https://www.youtube.com/embed/2gB7ipo8TNY" frameborder="0" allowfullscreen></iframe> 






### Get ES5 account
Follow the tutorial [Create an account to the SAP Gateway Demo System (ES5)](https://developers.sap.com/tutorials/gateway-demo-signup.html).

<!-- border -->![Get ES5 account tutorial](es5tut.png)






### Create destination for ES5
Open the [SAP BTP Cockpit](https://account.hanatrial.ondemand.com/trial/#/home/trial) where you will create connectivity between SAP BTP and the SAP Gateway Demo system account.

1.  In the left navigation panel, under **Connectivity**, click **Destinations**.

    <!-- border -->
    ![Open destinations](3-open-destinations.png)

2. Click **Create Destination**.

    <!-- border -->
    ![New destination](4-create-destination.png)


3.  Add the following destination properties:

    |  Field     | Value
    |  :------------- | :-------------
    |  Name           | `ES5-Shop` 
    |  Type          | `HTTP`
    |  Description    | `SAP Gateway ES5`
    |  URL           | `https://sapes5.sapdevcenter.com/sap/opu/odata/sap/EPM_REF_APPS_SHOP_SRV`
    |  Proxy Type          | `Internet`
    |  Authentication    | `BasicAuthentication`
    |  User Name          | Your ES5 Gateway user
    |  Password    | Your ES5 Gateway password

    Make sure that the **Use default JDK truststore** checkbox is checked.

5. Enter the following **Additional Properties**. Click the **New Property** button each time to add a new property.

    >Also make sure that you enter the values correctly, for example, the value 'true' must always be lowercase.

    |  Field     | Value
    |  :------------- | :-------------
    | `WebIDEEnabled`          | `true`   
    | `HTML5.DynamicDestination`          | `true`
    | `AppgyverEnabled`          | `true`


6. Click **Save**.

7. Test the connectivity to the new destination by clicking **Check Connection**.
   
   You should receive **200 – OK**. 







### Congratulations!
Congratulations, you are now ready for our **SAP Build CodeJam**. Have a safe journey and the SAP Advocates Team looks forward to seeing you on the day of your event.

Make sure you keep watching your specific **CodeJam event page** [here](https://groups.community.sap.com/t5/sap-codejam/eb-p/codejam-events).
