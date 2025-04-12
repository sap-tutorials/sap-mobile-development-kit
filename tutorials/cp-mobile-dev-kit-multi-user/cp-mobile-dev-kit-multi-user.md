---
parser: v2
auto_validation: true
primary_tag: software-product>mobile-development-kit-client
tags: [ tutorial>advanced, operating-system>ios, operating-system>android, topic>mobile, software-product>sap-business-technology-platform, software-product>mobile-development-kit-client, software-product>sap-mobile-services, software-product>sap-build-code, software-product>sap-business-application-studio ]
time: 35
author_name: Jitendra Kansal
author_profile: https://github.com/jitendrakansal
---

# Enable Multi-User Mode for MDK Application
<!-- description --> Set up the Mobile Development Kit client to enable the multi-user mode (one device, multiple users with secure access).


## Prerequisites
- **Tutorial**: [Set Up for the Mobile Development Kit (MDK)](https://developers.sap.com/group.mobile-dev-kit-setup.html)
- **Install SAP Mobile Services Client** on your [Android](https://play.google.com/store/apps/details?id=com.sap.mobileservices.client) device or [iOS](https://apps.apple.com/us/app/sap-mobile-services-client/id1413653544)
<table><tr><td align="center"><!-- border -->![Play Store QR Code](img-1.1.1.png)<br>Android</td><td align="center">![App Store QR Code](img-1.1.2.png)<br>iOS</td></tr></table>
- **Download the latest version of Mobile Development Kit SDK if you want to enable multi-user mode in MDK custom client** either from the SAP community [trial download](https://developers.sap.com/trials-downloads.html?search=Mobile+Development+Kit) or [SAP Software Center](https://me.sap.com/softwarecenter) if you are a SAP Mobile Services customer. This is required you to build a branded client.

## You will learn
  - How to define an event on user switching
  - How to configure multi-user settings in mobile service admin UI
  - How to create an offline enabled MDK application that supports multiple users on the same device


## Intro
You may clone an existing metadata project from the [MDK Tutorial GitHub repository](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/tree/main/5-Brand-Your-Customized-App-with-Mobile-Development-Kit-SDK/3-Enable-MultiUser-Mode-for-MDK-Application) and start directly with step 3 in this tutorial.

---
Multiple user mode is designed for enterprise scenarios where, for example, an employee uses a shared device during their shift. They may go offline while working in the field and return the device at the end of their shift. The next day, the employee might use a different device, while the device used previously could be assigned to another user. This is why it is called multiple user mode. In offline scenarios, multiple user mode ensures that the initial user's data, stored offline, is synchronized with the server before the second user logs in. This process preserves the previous user's changes to the data.

![MDK](img-8.4.png)
![MDK](img-8.5.png)

### Create a New Project Using SAP Build Code

This step includes creating a mobile project in SAP Build Lobby. 

1. In the SAP Build Lobby, click **Create** > **Create** to start the creation process.

    <!-- border -->![MDK](img-1.1.png)

2. Click the **Build an Application** tile.    

    <!-- border -->![MDK](img-1.2.png)

3. Click the **SAP Build Code** tile to develop your project in SAP Business Application Studio, the SAP Build Code development environment, leveraging the capabilities of the services included in SAP Build Code.

    <!-- border -->![MDK](img-1.3.png)

4. Click the **Mobile Application** tile. 

    <!-- border -->![MDK](img-1.4.png)


5. Enter the project name `multiuserapp` (used for this tutorial) , add a description (optional), and click **Create**. 

    <!-- border -->![MDK](img-1.5.png)
    
    >SAP Build Code recommends the dev space it deems most suitable, and it will automatically create a new one for you if you don't already have one. If you have other dev spaces of the Mobile Application type, you can select between them. If you want to create a different dev space, go to the Dev Space Manager. See [Working in the Dev Space Manager](https://help.sap.com/docs/build_code/d0d8f5bfc3d640478854e6f4e7c7584a/ad40d52d0bea4d79baaf9626509caf33.html).

6. Your project is being created in the Project table of the lobby. The creation of the project may take a few moments. After the project has been created successfully, click the project to open it. 

    <!-- border -->![MDK](img-1.6.png)
    
7. The project opens in SAP Business Application Studio, the SAP Build Code development environment.

    <!-- border -->![MDK](img-1.7.png)   

    >When you open the SAP Business Application Studio for the first time, a consent window may appear asking for permission to track your usage. Please review and provide your consent accordingly before proceeding.
    >![MDK](img-1.8.png) 

### Configure the Project Using Storyboard

The Storyboard provides a graphical view of the application's runtime resources, external resources, UI of the application, and the connections between them. This allows for a quick understanding of the application's structure and components.

- **Runtime Resources**: In the Runtime Resources section, you can see the mobile services application and mobile destination used in the project, with a dotted-line connected to the External Resources.
- **External Resources**: In the External Resources section, you can see the external services used in the project, with a dotted-line connection to the Runtime Resource or the UI app.
- **UI Application**: In the UI Applications section, you can see the mobile applications.

1. Click on **+** button in the **Runtime Resources** column to add a mobile services app to your project. 

    <!-- border -->![MDK](img-2.1.png) 

    >This screen will only show up when your CF login session has expired. Use either `Credentials` OR  `SSO Passcode` option for authentication. After successful signed in to Cloud Foundry, select your Cloud Foundry Organization and Space where you have set up the initial configuration for your MDK app and click Apply.

    >![MDK](img-2.2.png) 

2. Choose `myapp.mdk.demo` from the applications list in the **Mobile Application Services** editor.

    <!-- border -->![MDK](img-2.3.png)  

3. Select `com.sap.edm.sampleservice.v4` from the destinations list and click **Add App to Project**.

    <!-- border -->![MDK](img-2.4.png)  

    >You can access the mobile services admin UI by clicking on the Mobile Services option on the right hand side.

    In the storyboard window, the app and mobile destination will be added under the Runtime Resources column. The mobile destination will also be added under the External Resources with a dotted-line connection to the Runtime Resource. The External Resource will be used to create the UI application.

    <!-- border -->![MDK](img-2.5.png)      

4. Click the **+** button in the UI application column header to add mobile UI for your project.

    <!-- border -->![MDK](img-2.6.png)     

5. In the **Basic Information** step, provide the below information and click **Next**. You will modify the generated project in next step and will deploy it later.

    | Field | Value |
    |----|----|
    | `MDK Template Type` | `CRUD`  |
    | `Enable Auto-Deployment to Mobile Services After Creation` | Select `No` |

    <!-- border -->![MDK](img-2.7.png)  

    >The `CRUD` template generates the offline or online actions, rules, messages and pages to view, update, and manage records. More details on _MDK template_ is available in [help documentation](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/getting-started/mdk/bas.html#creating-a-new-project-cloud-foundry).

6. In the **Data Collections** step, provide the below information and click **Finish**. Data Collections step retrieves the entity sets information for the selected destination.

    | Field | Value |
    | `Enter a path to service (e.g. /sap/opu/odata/sap/SERVICE_NAME)` | Leave it as it is  |
    | `Select the Service Type` | Leave the default value as `OData` |
    | `Enable Offline` | It's enabled by default |
    | `Select all data collections` | Leave it as it is |
    | `What types of data will your application contain?` | Select `Customers` and `Products` |

    <!-- border -->![MDK](img-2.8.png) 

    Regardless of whether you are creating an online or offline application, this step is needed for app to connect to an OData service. When building an MDK Mobile application, it assumes the OData service created and the destination that points to this service is set up in Mobile Services. For MDK Web application, destination is set up in SAP BTP admin UI.

    >Since you have Enable Offline set to *Yes*, the generated application will be offline enabled in the MDK Mobile client and will run as online in Web environment.

    >Data Collections step retrieves the entity sets information for the selected destination.

7. After clicking **Finish**, the storyboard is updated displaying the UI component. The MDK project is generated in the project explorer based on your selections.
 
    <!-- border -->![MDK](img-2.9.png) 

### Define an event when user switches

MDK provides an [`OnUserSwitch`](https://help.sap.com/doc/3642933ef2e1478fb1578ef2acba4ae9/Latest/en-US/reference/schemadoc/App.schema.html#onuserswitch) event that is called when the user is switched when the client is operating in Multi-User mode. This event is triggered after any pending Offline OData transactions from previous user are successfully synced.

The purpose of the `OnUserSwitch` event is to inform the application that a user switch has taken place. Upon receiving this event, the application can execute any necessary actions to handle the user switch. At a minimum, this would involve running a Download action to update the offline store with the new user-specific data, while removing the data associated with the previous user.

Since the underlying offline database is shared, it is the responsibility of the application developer to ensure that all necessary steps are taken to reinitialize the application for the new user to continue functioning properly.


1. Click the **Application.app** to open it in MDK Application Editor and then and select the link icon for the `OnUserSwitch` event.

    <!-- border -->![MDK](img-2.10.png) 

2. Bind it to the `SyncStartedMessage.action`.

    <!-- border -->![MDK](img-2.11.png)  

    >For this tutorial, since we are using the Sample Service, which does not contain any user-specific data, we will simply trigger a sync to demonstrate that the event has been triggered.

### Deploy the Project

Now that the MDK application is created, you will Deploy the Project definitions to Mobile Services to use in the Mobile client.

1. Switch to the `Application.app` tab, click the **Deploy** option in the editor's header area, and then choose the deployment target as **Mobile Services**.

    <!-- border -->![MDK](img-2.12.png)

2. Select deploy target as **Mobile Services**.

    <!-- border -->![MDK](img-2.13.png)

    If you want to enable source for debugging the deployed bundle, then choose **Yes**.

    <!-- border -->![MDK](img-2.14.png)

    You should see **Deploy to Mobile Services successfully!** message.

    <!-- border -->![MDK](img-2.15.png)


### Enable multi-user mode in SAP Mobile Services

1. Open SAP Mobile Services UI, click **Mobile Applications** **&rarr;** **Native/MDK** **&rarr;** click `myapp.mdk.demo` app.

    <!-- border -->![MDK](img-3.1.png)

2. Under Assigned Features, click **Client Settings**.

    <!-- border -->![MDK](img-3.2.png)

3. Scroll through the page and enable the *Allow Upload of Pending Changes from Previous User (Enable Multiple User Mode)* checkbox under the **Shared Devices** section. This flag ensures that any pending offline changes from previous user are securely uploaded to the OData service. This is especially vital when the previous user has not uploaded the offline changes, and the app is being switched to a new user.

    <!-- border -->![MDK](img-3.3.png)

4. As the Multiple user mode is enabled, you must configure **Passcode Policy**, if not done before.

    <!-- border -->![MDK](img-3.4.png)

5. Once the passcode policy is set, click **Save**.    

### Configure Security trust

In a Multi-user scenario the client will automatically upload any pending transactions from the previous user before completing the user switch. In order for Mobile Services to perform this upload it needs to get a token for the previous user.  To do this `XSUAA` needs to trust Mobile Services. This is done by importing the Mobile Services key to the Trust Configuration in the BTP subaccount.

1. In SAP Mobile Services admin UI, click **Settings** **&rarr;** **Security**.

    <!-- border -->![MDK](img-4.1.png)

2. Click **Metadata Download**. 

    <!-- border -->![MDK](img-4.2.png)

3. Define the metadata expiration date up to ten years, or you can accept the one-year default date. Click **Download**. 

    <!-- border -->![MDK](img-4.3.png)

    >In case the metadata has been expired, end user may encounter an error while switching. You need to re-download the Mobile Services key and import it to the Trust Configuration in the BTP subaccount again.

4. Save the `SAMLMetadata.xml` file locally on your machine. 

5. Navigate to your sub account on SAP BTP. In the Side navigation bar, click **Security** **&rarr;** **Trust Configuration** and click **`New SAML Trust Configuration`**.

    <!-- border -->![MDK](img-4.4.png)

6. Click **Upload**, and select the XML file downloaded in the previous step, as a trusted identity provider. Enter a name, disable the **Available for User Login** checkbox and then click **Save**.

    <!-- border -->![MDK](img-4.5.png)

    >The trusted Identity Provider is not a real identity provider and cannot be used for user login. Disabling the **Available for User Login** checkbox ensures the identity provider does not appear on the XSUAA login page.
    >For more information, see [documentation](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/features/security/admin/config.html#configuring-security-trust).

### Test metadata configuration

Find out, if the metadata has been established successfully as a trusted provider.

Switch to the SAP Mobile Services UI. Click **Settings** **&rarr;** **Security** **&rarr;** **Test** .

<!-- border -->![MDK](img-5.1.png)

You should see a success message.

<!-- border -->![MDK](img-5.2.png)

>If the trust configuration is not established correctly, you may see a message like *SAML metadata might not have been imported as trusted identity provider* and the multi-user functionality in MDK client will not work.

### Create Your MDK Client (required only for branded clients)

If you are using SAP Mobile Services Client (public store) to test the multi-user functionality, you can skip this step.

1. Follow steps 1 to 3 from [Build Your Mobile Development Kit Client Using MDK SDK](https://developers.sap.com/tutorials/cp-mobile-dev-kit-build-client.html) and make sure to set the  `MultiUserEnabled` property to `true`. 

      <!-- border -->![MDK](img-6.1.png)

2. Create your MDK client either using MDK SDK by following the step 4 from [Build Your Mobile Development Kit Client Using MDK SDK](https://developers.sap.com/tutorials/cp-mobile-dev-kit-build-client.html) tutorial OR using SAP Cloud Build Service by following [Build Your Mobile Development Kit Client Using Cloud Build Service](https://developers.sap.com/tutorials/cp-mobile-dev-kit-cbs-client.html) tutorial.

### Run the MDK Client

Choose an option above based on whether you are testing multi-user functionality in the SAP Mobile Services Client (public store) or in a branded client built using MDK SDK or Cloud Build service. 

[OPTION BEGIN [SAP Mobile Services Client]]

>For an existing single user app it is required to reset the client and re-login before multi-user capability can be enabled.

1. Switch to the SAP Business Application Studio UI. SAP Business Application Studio has a feature to display the QR code for onboarding in the Mobile client. Open the `Application.app` and click the **Application QR Code** icon in the editor's header area.

    <!-- border -->![MDK](img-7.1.png)

    The Onboarding QR code is now displayed. You will notice that the `multiUser` parameter in the generated QR code is set to *true*. This is due to the flag *Allow Upload of Pending Changes from Previous User* being set in step 3 mobile services admin UI. The MDK client will set the user mode based on the scanned QR code. 

    <!-- border -->![MDK](img-7.2.png)

    >Alternatively, you can scan the QR code generated under *APIs* > *In-app Scanning Code* in the mobile services admin UI.

2. Make sure you have downloaded the **SAP Mobile Services Client** on your device from the App Store or Google Play as covered in the pre-requisites.

3. Onboard to the application. If you are using an Android device, follow [these steps](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/blob/main/Onboarding-Android-client/Onboarding-Android-client.md) or if you are using an iOS device, follow [these steps](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/blob/main/Onboarding-iOS-client/Onboarding-iOS-client.md).

[OPTION END]

[OPTION BEGIN [Branded Client]]

>For an existing single user app it is required to rebuild the client with above mentioned setting if the client doesn't allow scanning a QR to onboard. Once the client on a device is updated the user will have to reset the client and re-login before multi-user capability can be enabled.

Follow step 4 from [Build Your Mobile Development Kit Client Using MDK SDK](https://developers.sap.com/tutorials/cp-mobile-dev-kit-build-client.html) to run and on-board your branded client on your device. 

[OPTION END]

### Test a multi-user scenario

[OPTION BEGIN [Android]]

>Ensure that you choose the correct device platform tab above. 

1. After accepting the app update, you will see a list of entities on the **Main** page, along with a user menu that includes options such as syncing changes, accessing support, checking for updates, and resetting the app. An offline store will be initialized. By tapping any entity, you will navigate to a list page. If you select one of the items, the detail page will be displayed, allowing you to create, update, or delete the record. This record will be saved to the offline request queue database. You can navigate back to the main page and press the **Sync Changes** option in the user menu to upload any local changes to the backend. Once the upload is successful, the app will also download data from the backend to the offline store, ensuring both sides have the same dataset.

    ![MDK](img-8.1.png)

2. Make any change in your app. For example, tap **Customers** **&rarr;** tap any record **&rarr;** edit and save the changes. 

    ![MDK](img-8.2.png)

3. Navigate back to the main page. The logged in user should ideally sync the local changes before logging out. Let's assume that user has forgotten to sync the changes and has logged out from the app. Tap on the **Logout** option in the user menu on the main page.

    ![MDK](img-8.3.png)

4. In the **Sign-In** screen, tap on the **Switch or add user** option. 

    ![MDK](img-8.4.png)

    >This screen also appears when you send the app in the background and bring it in foreground or swipe to close and relaunch it.

5. When switching users, the device needs to be connected to the network. Click on the highlighted icon to add a new user.

    ![MDK](img-8.5.png)

    >Ensure all users being onboarded have access to the BTP account. You can configure this in the users section under security on your SAP BTP sub-account.
    ><!-- border -->![MDK](img-8.6.png)

6. Follow the onboarding flow for the second user. To protect your data from being accessed by another user, do not use the same passcode.

7. After confirming the passcode screen, you will notice a **Sync in Progress** screen. This ensures that any local changes made by the previous user are not lost if they forgot to upload their work at the end of the shift and the device is picked up by another user.

    ![MDK](img-8.7.png)

8. After a successful sync, the `OnUserSwitch` event has triggered.

    ![MDK](img-8.7.1.png)
    ![MDK](img-8.1.png)

9. Navigate to the customer record when previous user made changes. You should notice the updated data.

[OPTION END]

[OPTION BEGIN [iOS]]

>Ensure that you choose the correct device platform tab above. 

1. After accepting the app update, you will see a list of entities on the **Main** page, along with a user menu that includes options such as syncing changes, accessing support, checking for updates, and resetting the app. An offline store will be initialized. By tapping any entity, you will navigate to a list page. If you select one of the items, the detail page will be displayed, allowing you to create, update, or delete the record. This record will be saved to the offline request queue database. You can navigate back to the main page and press the **Sync Changes** option in the user menu to upload any local changes to the backend. Once the upload is successful, the app will also download data from the backend to the offline store, ensuring both sides have the same dataset.

    ![MDK](img-8.8.png)

2. Make any change in your app. For example, tap **Customers** **&rarr;** tap any record **&rarr;** edit and save the changes. 

    ![MDK](img-8.9.png)

3. Navigate back to the main page. The logged in user should ideally sync the local changes before logging out. Let's assume that user has forgotten to sync the changes and has logged out from the app. Tap on the **Logout** option in the user menu on the main page.

    ![MDK](img-8.10.png)

4. In the **Sign-In** screen, tap on the **Switch or add user** option. 

    ![MDK](img-8.11.png)

    >This screen also appears when you send the app in the background and bring it in foreground or swipe to close and relaunch it.

5. When switching users, the device needs to be connected to the network. Click on the highlighted icon to add a new user.

    ![MDK](img-8.12.png)

    >Ensure all users being onboarded have access to the BTP account. You can configure this in the users section under security on your SAP BTP sub-account.
    ><!-- border -->![MDK](img-8.6.png)

6. Follow the onboarding flow for the second user. To protect your data from being accessed by another user, do not use the same passcode.

7. After confirming the passcode screen, you will notice a **Sync in Progress** screen. This ensures that any local changes made by the previous user are not lost if they forgot to upload their work at the end of the shift and the device is picked up by another user.

    ![MDK](img-8.13.png)


8. After a successful sync, the `OnUserSwitch` event has triggered.

    ![MDK](img-8.14.png)
    ![MDK](img-8.8.png)

9. Navigate to the customer record when previous user made changes. You should notice the updated data.

[OPTION END]

You have learned how to enable an Mobile Development Kit Client to support multi-user login capability. This means a Mobile Development Kit Client app on a device can now securely be shared across multiple users. Features such as adding a new user, switching between user sessions, searching for a particular user are supported by the client. Find more information about Multi User in MDK in [help documentation](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/features/onboarding/mdk/multi-user.html).

---
