---
parser: v2
auto_validation: true
primary_tag: software-product>mobile-development-kit-client
tags: [ tutorial>beginner, operating-system>ios, operating-system>android, topic>mobile, software-product>sap-business-technology-platform, software-product>mobile-development-kit-client, software-product>sap-mobile-services, software-product>sap-business-application-studio ]
time: 15
author_name: Jitendra Kansal
author_profile: https://github.com/jitendrakansal
---


# Start Your MDK Application in the Editor
<!-- description --> Use the MDK editor to create a multi-channel (mobile and web) application.

## Prerequisites
- **Tutorial group:** [Set Up for the Mobile Development Kit (MDK)](https://developers.sap.com/group.mobile-dev-kit-setup.html)
- **Install SAP Mobile Services Client** on your [Android](https://play.google.com/store/apps/details?id=com.sap.mobileservices.client) device or [iOS](https://apps.apple.com/us/app/sap-mobile-services-client/id1413653544)
<table><tr><td align="center">![Play Store QR Code](img-1.1.1.png)<br>Android</td><td align="center">![App Store QR Code](img-1.1.2.png)<br>iOS</td></tr></table>
(If you are connecting to `AliCloud` accounts, you will need to brand your [custom MDK client](https://developers.sap.com/tutorials/cp-mobile-dev-kit-build-client.html) by allowing custom domains.)

## You will learn
  - How to create an MDK app using a template in SAP Business Application Studio
  - How to deploy an MDK app to Mobile Services and run it offline enabled in a mobile client
  - How to deploy an MDK app to Cloud Foundry and run it as a web application

---

### Create a new MDK project in SAP Business Application Studio

This step includes creating the mobile development kit project in the editor.

1. Launch the [Dev space](https://developers.sap.com/tutorials/cp-mobile-bas-setup.html) in SAP Business Application Studio.

2. Click **New Project from Template** on the `Get Started` page.

    ![MDK](img-1.1.png)

    >If you do not see the `Get Started` page, you can access it by typing `>get started` in the center search bar.

    ![MDK](img-1.2.gif)

3. Select **MDK Project** and click **Start**. If you do not see the **MDK Project** option check if your Dev Space has finished loading or reload the page in your browser and try again.

    ![MDK](img-1.2.png)

    >This screen will only show up when your CF login session has expired. Use either `Credentials` OR  `SSO Passcode` option for authentication. After successful signed in to Cloud Foundry, select your Cloud Foundry Organization and Space where you have set up the initial configuration for your MDK app and click Apply.

    >![MDK](img-1.4.png)

4. In *Basic Information* step, provide the below information and click **Next**:

    | Field | Value |
    |----|----|
    | `MDK Template Type`| Select `Base` from the dropdown |
    | `Your Project Name` | Provide a name of your choice. `DemoSampleApp` is used for this tutorial |
    | `Your Application Name` | <default name is same as project name, you can provide any name of your choice> |
    | `Target MDK Client Version` | Leave the default selection as `MDK 23.4+ (For use with MDK 23.4 or later clients)` |
    | `Choose a target folder` | By default, the target folder uses project root path. However, you can choose a different folder path |

    ![MDK](img-1.3.png)

    >The `Base` template creates the offline or online actions, rules, messages and an empty page (`Main.page`). After using this template, you can focus on creating your pages, other actions, and rules needed for your application. More details on _MDK template_ is available in [help documentation](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/getting-started/mdk/bas.html#creating-a-new-project-cloud-foundry).

5. In *Service Configuration* step, provide the below information and click **Next**:

    | Field | Value |
    |----|----|
    | `Data Source` | Select `Mobile Services` from the dropdown |
    | `Mobile Services Landscape` | Select `standard` from the dropdown |
    | `Application Id` | Select `com.sap.mdk.demo` from the dropdown |
    | `Destination` | Select `SampleServiceV4` from the dropdown |
    | `Enter a path to service` | Leave it as it is |
    | `Enable Offline` | It's enabled by default |

    ![MDK](img-1.5.png)

    >Regardless of whether you are creating an online or offline application, this step is needed for the app to connect to an OData service. When building an MDK application, it assumes the OData service created and the destination that points to this service is set up in [Mobile Services](https://developers.sap.com/tutorials/cp-mobile-dev-kit-ms-setup.html) (for Mobile consumption) and in [Cloud Foundry cockpit](https://developers.sap.com/tutorials/cp-mobile-dev-kit-ms-setup.html#7ecb8e88-6cb0-406f-a038-e83a55bd8360) (for Web consumption).

    >**Enable Offline** option allows MDK mobile app to be offline enabled. This configuration will be ignored on Web environment and the MDK web application will be treated as online only.


6. In **Data Collections** step, select `Customers`, `Products`, `SalesOrderHeaders` and `SalesOrderItems` data collections. Click **Finish**.

    ![MDK](img-1.6.png)

    >Data Collections step retrieves the entity sets information for the selected destination.

    After clicking Finish, the wizard will generate your MDK Application based on your selections. You should now see the `DemoSampleApp` project in the project explorer.


### Get familiar with generated project structure


This is how the project structure looks like within the workspace.

![MDK](img-2.0.png)

These are the [metadata definitions](https://help.sap.com/doc/69c2ce3e50454264acf9cafe6c6e442c/Latest/en-US/docs-en/reference/schemadoc/App.schema.html) available in the editor and the format in which these metadata definitions are stored in the editor. Just to brief on some of these:

- **`InitializeOffline.action`**: For Mobile applications, this action binds the application to the Mobile Services Offline OData server and downloads the required data from the `SampleServiceV4` to the offline store on the mobile device. For Web applications, it will initialize the `SampleServiceV4` service to be consumed in online mode.

- **`DownloadOffline.action`** and **`UploadOffline.action`**: These actions are applicable to Mobile client only. Using Mobile app initialization, data is downloaded to the offline store. If you want to have the application download any updated data from the backend server or upload changed data to the backend server (`SampleServiceV4`), these actions will be needed.

- **`Success & Failure Message action`**: Here are some messages showing up in the app on a successful or failure of data initialization, sync etc.

- **`Main.page`**: This is the first page of your MDK application that is shown. For this application you will use this as a launching page to get to application functionality.

- **`OnWillUpdate.js`**: This rule is applicable to Mobile client only. MDK applications automatically download updates and apply them to the client without the end-user needing to take any action. The `OnWillUpdate` rule empowers the user to run business logic before the new definitions are applied. This allows the app designer to include logic to prompt the user to accept or defer applying the new definitions based on their current activity. For example, if the end-user is currently adding new customer details or in the middle of a transaction, they will be able to defer the update. The app will prompt again the next time it checks for updates.

- **`Web`**: In this folder, you can provide web specific app resource files and configurations.

- **`Application.app`**: this is the main configuration file for your application from within SAP Business Application Studio. Here you define your start page (here in this tutorial, it is main.page), action settings for different stages of the application session lifecycle, push notifications, and more.

### Deploy the application

So far, you have learned how to quickly build an MDK application in the SAP Business Application Studio editor. Now, you will deploy the application definitions to Mobile Services and Cloud Foundry to use it in the Mobile client and Web application respectively.

1. Right-click `Application.app` and select **MDK: Deploy**.

    ![MDK](img-3.1.png)

2. Select deploy target as **Mobile & Cloud**.

   MDK editor will deploy the metadata to Mobile Services (for Mobile application) followed by to Cloud Foundry (for Web application). 
   
   - For Mobile Services deployment, choose Yes or No if you want to enable source for debugging the deployed bundle.
   - For Cloud Foundry deployment, enter application ID, application version and select web runtime deployment option.

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

### Run the app


[OPTION BEGIN [Android]]

>Ensure that you choose the correct device platform tab above. Once you have scanned and onboarded using the onboarding URL, it will be remembered. If you log out and onboard again, you will be prompted to either continue using the current application or scan a new QR code.

Follow [these steps](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/blob/main/Onboarding-Android-client/Onboarding-Android-client.md) to successfully on-board the MDK client on your Android device.

After accepting the app update, you will see the **Main** page, along with a user menu that includes options such as syncing changes, accessing support, checking for updates, and resetting the app. An Offline store will be initialized. **Since you selected the Base template during the project creation, it generated this empty page without any UI controls. In the next tutorials, you will add some UI controls to this page and create more pages.**

![MDK](img-5.1.png)

[OPTION END]

[OPTION BEGIN [iOS]]

>Ensure that you choose the correct device platform tab above. Once you have scanned and onboarded using the onboarding URL, it will be remembered. If you log out and onboard again, you will be prompted to either continue using the current application or scan a new QR code.

Follow [these steps](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/blob/main/Onboarding-iOS-client/Onboarding-iOS-client.md) to successfully on-board the MDK client on your iOS device.

After accepting the app update, you will see the **Main** page, along with a user menu that includes options such as syncing changes, accessing support, checking for updates, and resetting the app. An Offline store will be initialized. **Since you selected the Base template during the project creation, it generated this empty page without any UI controls. In the next tutorials, you will add some UI controls to this page and create more pages.**

![MDK](img-5.2.png)

[OPTION END]

[OPTION BEGIN [Web]]

1. Click the highlighted button to open the MDK Web application in a browser. If prompted, enter your SAP BTP credentials.

    ![MDK](img-5.3.png)

    >You can also open the MDK web application by accessing its URL from the `.project.json` file.
    ![MDK](img-5.4.png)

    You will notice the **Main** page and a user menu on the top right corner. The application data service will be initialized. **Since you selected the Base template during the project creation, it generated this empty page without any UI controls. In the next tutorials, you will add some UI controls to this page and create more pages.**

    ![MDK](img-5.5.png)

[OPTION END]


---
