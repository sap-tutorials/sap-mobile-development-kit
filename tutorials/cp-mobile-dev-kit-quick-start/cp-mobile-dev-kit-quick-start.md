---
parser: v2
auto_validation: true
primary_tag: software-product>mobile-development-kit-client
tags: [ tutorial>beginner, operating-system>ios, operating-system>android, topic>mobile, software-product>sap-business-technology-platform, software-product>mobile-development-kit-client, software-product>sap-mobile-services, software-product>sap-business-application-studio]
time: 15
author_name: Jitendra Kansal
author_profile: https://github.com/jitendrakansal
---

# Quick Start with the Mobile Development Kit (MDK)
<!-- description --> Create and examine your first mobile (offline) and web application using the MDK template connecting against a sample service.

## Prerequisites
- **Tutorial group:** [Set Up for the Mobile Development Kit (MDK)](group.mobile-dev-kit-setup)
- **Install SAP Mobile Services Client** on your [Android](https://play.google.com/store/apps/details?id=com.sap.mobileservices.client) or [iOS](https://apps.apple.com/us/app/sap-mobile-services-client/id1413653544) device
<table><tr><td align="center">![Play Store QR Code](img-0.1.png)<br>Android</td><td align="center">![App Store QR Code](img-0.2.png)<br>iOS</td></tr></table>
(If you are connecting to `AliCloud` accounts, you will need to brand your [custom MDK client](cp-mobile-dev-kit-build-client) by allowing custom domains.)

## You will learn
  - How to create an MDK sample app using a template in SAP Business Application Studio
  - How to deploy an MDK app to Mobile Services and run it in mobile client
  - How to deploy an MDK app to Cloud Foundry and run it as a Web application

---

### Create a new MDK project in SAP Business Application Studio

This step includes creating the mobile development kit project in the editor.

1. Launch the [Dev space](cp-mobile-bas-setup) in SAP Business Application Studio.

2. Click **New Project from Template** on Get Started page.

    ![MDK](img-1.1.png)

    >If you do not see the `Get Started` page, you can access it by typing `>get started` in the center search bar.

    ![MDK](img-1.2.gif)

3. Select **MDK Project** and click **Start**. If you do not see the **MDK Project** option check if your Dev Space has finished loading or reload the page in your browser and try again.

    ![MDK](img-1.3.png)

    >This screen will only show up when your CF login session has expired. Use either `Credentials` OR  `SSO Passcode` option for authentication. After successful signed in to Cloud Foundry, select your Cloud Foundry Organization and Space where you have set up the initial configuration for your MDK app and click Apply.

    >![MDK](img-1.4.png)

4. In *Basic Information* step, provide the below information and click **Next**:

    | Field | Value |
    |----|----|
    | `MDK Template Type`| Select `CRUD` from the dropdown |
    | `Your Project Name` | Provide a name of your choice. `MDKApp` is used for this tutorial |
    | `Your Application Name` | <default name is same as project name, you can provide any name of your choice> |
    | `Target MDK Client Version` | Leave the default selection as `MDK 23.4+ (For use with MDK 23.4 or later clients)` |
    | `Choose a target folder` | By default, the target folder uses project root path. However, you can choose a different folder path |

    ![MDK](img-1.5.png)

5. In *Service configuration* step, provide the below information and click **Next**:

    | Field | Value |
    |----|----|
    | `Data Source` | Select `Mobile Services` from the dropdown |
    | `Mobile Services Landscape` | Select `standard` from the dropdown |
    | `Application Id` | Select `com.sap.mdk.demo` from the dropdown (this app was configured as per [Set Up Initial Configuration for an MDK App](cp-mobile-dev-kit-ms-setup) tutorial) |
    | `Destination` | Select `SampleServiceV4` from the dropdown |
    | `Enter a path to service` | Leave it as it is |
    | `Enable Offline` | It's enabled by default |

    ![MDK](img-1.6.png)

    Regardless of whether you are creating an online or offline application, this step is needed for app to connect to an OData service. When building an MDK Mobile application, it assumes the OData service created and the destination that points to this service is set up in Mobile Services. For MDK Web application, destination is set up in SAP BTP cockpit.

    Since we have Enable Offline set to Yes, the generated application will be offline enabled in the MDK Mobile client and will run as online in Web environment.

6. In the *Data Collections* step, select `Customers`, `Products`, `PurchaseOrderHeaders`, `PurchaseOrderItems` `SalesOrderHeaders` and `SalesOrderItems`. Click **Finish** to complete the project creation.

    ![MDK](img-1.7.png)

7. After clicking **Finish**, the wizard will generate your MDK Application based on your selections. You should now see the `MDKApp` project in the project explorer.


### Get familiar with generated project structure

This is how the project structure looks like within the workspace.

![MDK](img-2.1.png)

These are the [metadata definitions](https://help.sap.com/doc/69c2ce3e50454264acf9cafe6c6e442c/Latest/en-US/docs-en/reference/schemadoc/App.schema.html) available in the editor and the format in which these metadata definitions are stored in the editor. Just to brief on some of these:

- **`InitializeOffline.action`**: For Mobile applications, this action binds the application to the Mobile Services Offline OData server and downloads the required data to the offline store on the mobile device. For Web applications, it will initialize the service to be consumed in online mode.

- **`DownloadOffline.action`** and **`UploadOffline.action`**: These actions are applicable to Mobile client only. Using app initialization, data is downloaded to the offline store. If you want to have the application download any updated data from the backend server or upload changed data to the backend server, these actions will be needed.

- **`Success & Failure Message action`**: Here are some messages showing up in the app on a successful or failure of data initialization, sync etc.

- **`Main.page`**: This is the first page of your MDK application that is shown. For this application you will use this as a launching page to get to application functionality.

- **`OnWillUpdate.js`**: This rule is applicable to Mobile client only. MDK applications automatically download updates and apply them to the client without the end-user needing to take any action. The `OnWillUpdate` rule empowers the user to run business logic before the new definitions are applied. This allows the app designer to include logic to prompt the user to accept or defer applying the new definitions based on their current activity. For example, if the end-user is currently adding new customer details or in the middle of a transaction, they will be able to defer the update. The app will prompt again the next time it checks for updates.

- **`Web`**: In this folder, you can provide web specific app resource files and configurations.

- **`Application.app`**: this is the main configuration file for your application from within SAP Business Application Studio. Here you define your start page (here in this tutorial, it is main.page), action settings for different stages of the application session lifecycle, push notifications, and more.

>Open the application settings in the application editor by clicking the `Application.app`.

>![MDK](img-2.2.png)


### Deploy the application

So far, you have learned how to quickly get started with developing an MDK application in the SAP Business Application Studio editor. Now, you will deploy the application definitions to Mobile Services and Cloud Foundry to use it in the Mobile client and Web application respectively.

1. Right-click `Application.app` and select **MDK: Deploy**.

    ![MDK](img-3.1.png)

2. Select the deploy target as **Mobile & Cloud**. The MDK editor will deploy the metadata to Mobile Services (for the Mobile application) followed by to Cloud Foundry (for the Web application).

   ![MDK](img-3.2.gif)

>First web deployment takes 2-3 minutes as it creates five service instances for the application. You can find the details of these instances in the space cockpit.

>-	XSUAA

>- destination

>- connectivity

>- HTML Repo host

>- HTML repo runtime


Ensure that you see successful messages for both deployments.

![MDK](img-3.3.png)


### Display the QR code for onboarding the Mobile app


SAP Business Application Studio has a feature to display the QR code for onboarding in the Mobile client. Click on `Application.app` to open it in MDK Application Editor, and then click the **Application QR Code** icon.

![MDK](img-4.1.png)

The On-boarding QR code is now displayed.

![MDK](img-4.2.png)

>Leave the Onboarding dialog box open for the next step.


### Explore the Demo Mode (Optional)

Ensure that you have downloaded the SAP Mobile Services Client from the public store, as mentioned in the prerequisites. This client provides a demo mode with Demo App and Mentor App.

**Demo App**: The Demo app is intented for you to explore the MDK capabilities. This app connects to the same Mobile Services sample backend to which your MDK app is connected.

**Mentor App**: The MDK Mentor app is interactive documentation that helps designers and developers discover the capabilities of the SAP Mobile Development Kit. You can view live previews of the UI components and change parameters to see the effects immediately. 

In both applications, the user popover menu provides an option to easily switch between the Demo and Mentor Apps. When you are done exploring, you can log out to return to the MDK Welcome screen, which resets any changes made and allows you to onboard to your backend application.

[OPTION BEGIN [Android]]

>Ensure that you choose the correct device platform tab above.

![MDK](img-4.3.png)

[OPTION END]

[OPTION BEGIN [iOS]]

>Ensure that you choose the correct device platform tab above.

![MDK](img-4.4.png)

[OPTION END]


### Run the app

[OPTION BEGIN [Android]]

>Ensure that you choose the correct device platform tab above. Once you have scanned and onboarded using the onboarding URL, it will be remembered. If you log out and onboard again, you will be prompted to either continue using the current application or scan a new QR code.

Follow [these steps](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/blob/main/Onboarding-Android-client/Onboarding-Android-client.md) to successfully on-board the MDK client on your Android device.

After accepting the app update, you will see a list of entities on the **Main** page, along with a user menu that includes options such as syncing changes, accessing support, checking for updates, and resetting the app. An offline store will be initialized. By tapping any entity, you will navigate to a list page. If you select one of the items, the detail page will be displayed, allowing you to create, update, or delete the record. This record will be saved to the offline request queue database. You can navigate back to the main page and press the **Sync Changes** option in the user menu to upload any local changes to the backend. Once the upload is successful, the app will also download data from the backend to the offline store, ensuring both sides have the same dataset.

![MDK](img-5.1.gif)

Additionally, you can search through all properties of the objects displayed in the section by entering them manually or using a barcode scanner. For instance, in the Products list, you can scan the barcode to search for products belonging to the *MP3 Players* category.

![MDK](img-5.2.gif)

[OPTION END]

[OPTION BEGIN [iOS]]

>Ensure that you choose the correct device platform tab above. Once you have scanned and onboarded using the onboarding URL, it will be remembered. If you log out and onboard again, you will be prompted to either continue using the current application or scan a new QR code.

Follow [these steps](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/blob/main/Onboarding-iOS-client/Onboarding-iOS-client.md) to successfully on-board the MDK client on your iOS device.

After accepting the app update, you will see a list of entities on the **Main** page, along with a user menu that includes options such as syncing changes, accessing support, checking for updates, and resetting the app. An offline store will be initialized. By tapping any entity, you will navigate to a list page. If you select one of the items, the detail page will be displayed, allowing you to create, update, or delete the record. This record will be saved to the offline request queue database. You can navigate back to the main page and press the **Sync Changes** option in the user menu to upload any local changes to the backend. Once the upload is successful, the app will also download data from the backend to the offline store, ensuring both sides have the same dataset.

![MDK](img-5.3.gif)

Additionally, you can search through all properties of the objects displayed in the section by entering them manually or using a barcode scanner. For instance, in the Products list, you can scan the barcode to search for products belonging to the *MP3 Players* category.

![MDK](img-5.4.gif)

[OPTION END]

[OPTION BEGIN [Web]]

>For this tutorial, the MDK web app will connect to the SAP Mobile Services' sample backend, which will register your user in Mobile Services application. If you encounter any registration count limit-related errors, please note that there is a limitation of [total 10 user registrations per app in trial accounts](https://help.sap.com/viewer/468990a67780424a9e66eb096d4345bb/Cloud/en-US/16439fd40a014138abc5dc262e816be5.html).

1. Click the highlighted button to open the MDK Web application in a browser. If prompted, enter your SAP BTP credentials.

    ![MDK](img-5.5.png)

    >You can also open the MDK web application by accessing its URL from the `.project.json` file.
    ![MDK](img-5.6.png)

    You will notice the list of entities on the **Main** page along with a user menu in the top right corner. The application data service will be initialized. Click on any entity; it will navigate to the detail page where you can create, update, and delete a record.

    ![MDK](img-5.7.gif)

[OPTION END]

Once you complete this tutorial, you can continue with [these tutorials](mission.mobile-dev-kit-get-started) to create an MDK app from scratch.



---
