---
parser: v2
auto_validation: true
primary_tag: software-product>mobile-development-kit-client
tags: [ tutorial>intermediate, operating-system>ios, operating-system>android, topic>mobile, software-product>sap-business-technology-platform, software-product>mobile-development-kit-client, software-product>sap-mobile-services, software-product>sap-build-code, software-product>sap-business-application-studio ]
time: 30
author_name: Jitendra Kansal
author_profile: https://github.com/jitendrakansal
---

# Handle Error Archive in an MDK App
<!-- description --> Create an MDK app to display errors occurred while uploading local changes and implement some logic on how to handle such errors and then let users to fix it from the app by providing correct values.

## Prerequisites
- **Tutorial group:** [Set Up for the Mobile Development Kit (MDK)](https://developers.sap.com/group.mobile-dev-kit-setup.html)
- **Install SAP Mobile Services Client** on your [Android](https://play.google.com/store/apps/details?id=com.sap.mobileservices.client) device or [iOS](https://apps.apple.com/us/app/sap-mobile-services-client/id1413653544)
<table><tr><td align="center"><!-- border -->![Play Store QR Code](img-1.1.1.png)<br>Android</td><td align="center">![App Store QR Code](img-1.1.2.png)<br>iOS</td></tr></table>
(If you are connecting to `AliCloud` accounts, you will need to brand your [custom MDK client](https://developers.sap.com/tutorials/cp-mobile-dev-kit-build-client.html) by allowing custom domains.)


## You will learn
- How to access Error Archive entity set to display local upload failure
- How to handle a logic failure errors
- How to fix these errors

## Intro
You may clone an existing metadata project from the [MDK Tutorial GitHub repository](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/tree/main/4-Level-Up-with-the-Mobile-Development-Kit/1-Handle-Error-Archive-in-an-MDK-App) and start directly with step 6 in this tutorial.

---

You have built an MDK app with offline functionality. In offline store, you make a change to a local record and upload this change (from request queue) to backend but backend prevents this change to accept due to some business logic failure. This error is recorded in an Offline OData specific entity set named as `ErrorArchive`. This entity set has detailed information about the errors. It's now up to developers how they handle such errors and then let users to fix it from the app by providing the correct values. `ErrorArchive` is exposed to the application as an OData entity set and is accessible through the OData API in the same way that the application accesses any other entity sets from the offline store. You can find more details about the `ErrorArchive` Entity Properties in [this](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/features/offline/common/handling-errors-and-conflicts/errorarchive-entity-properties.html) documentation.

![MDK](img-1.0.gif)

 In this tutorial, you need to carry out the following tasks in order to understand how to display and handle such errors:

- Create a new project in SAP Business Application Studio using MDK CRUD Project template
- Access Error pages displaying records rejected by backend
- Create a business logic to find the affected entity
- Navigate to the affected record to handle the error

> For this tutorial, you will use **Mobile Services sample backend** destination. You will modify a `SalesOrderHeaders` record by changing `CurrencyCode` field. Offline store saves this record in request queue database and when you sync it with backend, backend prevents updating this record due to business logic failure. This failure record will be listed in Error list page, from here, you can navigate to details page for more information. You will implement a logic to navigate from details page to the affected record.

### Create a New Project Using SAP Build

This step includes creating a mobile project in SAP Build Lobby. 

1. In the SAP Build Lobby, click **Create** > **Create** to start the creation process.

    <!-- border -->![MDK](img-1.1.png)

2. Click the **Application** tile and choose **Next**.    

    <!-- border -->![MDK](img-1.2.png)

3. Select the **Mobile** category and choose **Next**.

    <!-- border -->![MDK](img-1.3.png)

4. Select the **Mobile Application** to develop your mobile project in SAP Business Application Studio and choose **Next**. 

    <!-- border -->![MDK](img-1.4.png)

5. Enter the project name `mdk_errorarchive` (used for this tutorial) , add a description (optional), and click **Review**. 

    <!-- border -->![MDK](img-1.5.png)
    
    >SAP Build recommends the dev space it deems most suitable, and it will automatically create a new one for you if you don't already have one. If you have other dev spaces of the Mobile Application type, you can select between them. If you want to create a different dev space, go to the Dev Space Manager. See [Working in the Dev Space Manager](https://help.sap.com/docs/build_code/d0d8f5bfc3d640478854e6f4e7c7584a/ad40d52d0bea4d79baaf9626509caf33.html).

6. Review the inputs under the Summary tab. If everything looks correct, click **Create** to proceed with creating your project.

    <!-- border -->![MDK](img-1.5.1.png)

7. Your project is being created in the Project table of the lobby. The creation of the project may take a few moments. After the project has been created successfully, click the project to open it. 

    <!-- border -->![MDK](img-1.6.png)
    
8. The project opens in SAP Business Application Studio.

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

5. In the **Basic Information** step, select the **MDK Template Type** as **CRUD**, leave the other options as they are. Since the option to **Enable Auto-Deployment to Mobile Services After Project Creation** is set to **Yes**, the MDK project will automatically be deployed to the Mobile Services after it is generated. Click **Next** to continue.

    <!-- border -->![MDK](img-2.7.png)  

    >The `CRUD` template generates the offline or online actions, rules, messages and pages to view, update, and manage records. More details on _MDK template_ is available in [help documentation](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/getting-started/mdk/bas.html#creating-a-new-project-cloud-foundry).

6. In the **Data Collections** step, provide the below information and click **Finish**. Data Collections step retrieves the entity sets information for the selected destination.

    | Field | Value |
    |----|----|
    | `Enter a path to service (e.g. /sap/opu/odata/sap/SERVICE_NAME)` | Leave it as it is  |
    | `Select the Service Type` | Leave the default value as `OData` |
    | `Enable Offline` | It's enabled by default |
    | `Select all data collections` | Leave it as it is |
    | `What types of data will your application contain?` | Select `Customers`, `Products`, `SalesOrderHeaders` and `SalesOrderItems` |

    <!-- border -->![MDK](img-2.8.png) 

    Regardless of whether you are creating an online or offline application, this step is needed for app to connect to an OData service. When building an MDK Mobile application, it assumes the OData service created and the destination that points to this service is set up in Mobile Services. 

    >Since you have Enable Offline set to *Yes*, the generated application will be offline enabled in the MDK Mobile client.

    >Data Collections step retrieves the entity sets information for the selected destination.

7. After clicking **Finish**, the storyboard is updated displaying the UI component. The MDK project is generated in the project explorer and automatically deployed to the Mobile Services based on your selections. You will now see a QR code for onboarding the mobile app. Leave the Onboarding dialog box open for the next step.
 
    <!-- border -->![MDK](img-2.9.png) 

    <!-- border -->![MDK](img-2.10.png) 

### Run the Project

>Make sure you are choosing the right device platform tab above. Once you have scanned and on-boarded using the onboarding URL, it will be remembered. When you Log out and on-board again, you will be asked either to continue to use current application or to scan new QR code.

[OPTION BEGIN [Android]]

1. Follow [these steps](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/blob/main/Onboarding-Android-client/Onboarding-Android-client.md) to successfully on-board the MDK client on your Android device.

    After you accept app update, you will see **Main** page with some entity sets being displayed and Offline store will be initialized.

    ![MDK](img-4.1.png)

2. You will modify a Sales Order record, save it locally, sync it to the backend and if backend doesn't accept this change due to some business logic failure, you will view them in Error Archive list.

    Navigate to `SalesOrderHeaders` list, tap either one of the records.

    ![MDK](img-4.2.png)

3. Tap edit icon. Make some changes to `CurrencyCode` value (update it to `EUROOO`) and tap the save icon.

    ![MDK](img-4.3.png)
    ![MDK](img-4.4.png)

    You will see **Entity Updated** toast message. You can always see this updated record reflecting in `SalesOrderHeaders` list which means offline store has accepted this change.

4. Navigate to `Main.page`, tap on the **Sync Changes** option in the user menu to upload local changes from device to the backend and to download the latest changes from backend to the device.

    ![MDK](img-4.5.png)

5. You will see *Upload failed* message, tap on **View Errors** to navigate to the Error Archive list.

    ![MDK](img-4.6.png)
    ![MDK](img-4.7.png)

6. Tapping any record navigates to Error Details page with more information about error.

    ![MDK](img-4.8.png)

    Here in **Error**, you will see `SQLDatabaseException` and in **Request Body**, it shows the record that caused this failure.

    In next step, you will implement how to handle such errors and let users to modify record with correct values.

[OPTION END]

[OPTION BEGIN [iOS]]

1. Follow [these steps](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/blob/main/Onboarding-iOS-client/Onboarding-iOS-client.md) to successfully on-board the MDK client on your iOS device.

    After you accept app update, you will see **Main** page with some entity sets being displayed and Offline store will be initialized.

    ![MDK](img-4.8.png)

2. You will modify a Sales Order record, save it locally, sync it to the backend and if backend doesn't accept this change due to some business logic failure, this record will appear in Error Archive list.

    Navigate to `SalesOrderHeaders` list, tap either one of the record.

    ![MDK](img-4.9.png)

3. Tap **Edit**. Make some changes to `CurrencyCode` value (update it to `EUROOO`) and **Save** it.

    ![MDK](img-4.10.png)
    ![MDK](img-4.11.png)

    You will see **Entity Updated** toast message. You can always see this updated record reflecting in `SalesOrderHeaders` list which means offline store has accepted this change.

4. Navigate to `Main.page`, click on the **Sync Changes** option in the user menu to upload local changes from device to the backend and to download the latest changes from backend to the device.

    ![MDK](img-4.12.png)

5. You will see *Upload failed* message, tap on **View Errors** to navigate to the Error Archive list.

    ![MDK](img-4.13.png)
    ![MDK](img-4.14.png)    

6. Tapping any record navigates to Error Details page with more information about error.

    ![MDK](img-4.15.png)

    Here in **Error**, you will see `SQLDatabaseException` and in **Request Body**, it shows the record that caused this failure.

    In next step, you will implement how to handle such errors and let users to modify record with correct values.

[OPTION END]


### Handle the Affected Records And Modify with Correct Data

On the Error Details page, you will implement how to navigate to respective record to let users to modify the affected record with correct values and will also display the Affected Entity Object. Once the record is modified with the correct values, user can again sync it with backend.


1. Add an **Object Table** control in `ErrorArchive_Detail.page` to display some information like affected entity and id for the affected record.

    Open `Pages` | `ErrorArchive` |`ErrorArchive_Detail.page`, in the Layout Editor, expand the **Controls** | **Data Bound Container** group, drag and drop the **Object Table** control onto the page area.

    <!-- border -->![MDK](img-5.1.gif)

2. In the **Properties** | **Target** pane, choose **String Target** from the dropdown and provide `{AffectedEntity}` value.

    <!-- border -->![MDK](img-5.2.png)

    >`AffectedEntity`: A navigation property that allows applications to navigate from an `ErrorArchive` entity to an entity in the offline store that is affected by the error.

3. In **Appearance** section, provide below properties:

    | Property | Value |
    |----|----|
    | `Description`| leave it empty |
    | `Footnote`| leave it empty |
    | `PreserveIconStackSpacing` | Select `false` from the dropdown|
    | `ProgessIndicator`| leave it empty |
    | `StatusText` | leave it empty  |
    | `Subhead` | `{@odata.id}` |
    | `SubstatusText` | leave it empty  |
    | `Tags` | Click the `item0` and click the trash icon to delete the default item |
    | `Title` | `Edit Affected Entity` |

    <!-- border -->![MDK](img-5.3.png)

    >`@odata.id`: an annotation that contains the entity-id. More details can be found [here](http://docs.oasis-open.org/odata/odata-json-format/v4.0/cs01/odata-json-format-v4.0-cs01.html#_Toc365464691).

4. In **Behavior** section, update below properties:

    | Property | Value |
    |----|----|
    | `AccessoryType`| `DisclosureIndicator` |

    <!-- border -->![MDK](img-5.4.png)

5. In the **Avatar Grid** section of the **Properties** pane, remove the default Avatar.  First, click on the `item0`, a trash icon appears. Click on the trash icon to delete the default item.

    <!-- border -->![MDK](img-5.5.png)

6. In the **Avatar Stack** section of the **Property** pane, remove the default Avatar.  First, click on the `item0`, a trash icon appears. Click on the trash icon to delete the default item.

    <!-- border -->![MDK](img-5.6.png)  

7. When tapping on this Object Table control, you want to bring the affected record so that you can fix business failure by modifying previous changes right there. For this, you will write a business logic to decide which action to call depends on which `@odata.type` is the `affectedEntity` and if there is no handler for an affected entity, app will display a toast message saying this affected entity doesn't have a handle yet.

    In the `ErrorArchive_Detail.page`, select the Object Table control, navigate to the **Events** tab. Click the dotted icon for the `OnPress` property and select the `Create a rule/action`.

    <!-- border -->![MDK](img-5.7.png)  

8. Select **Rule** for the *Object Type* and select the `/mdk_errorarchive/Rules/com_sap_edm_sampleservice_v4` folder path to create a new rule that will be generated in the chosen path.

    <!-- border -->![MDK](img-5.8.png)

    >You may choose a path of your choice. Since the template generates an `ErrorArchive` folder under *Rules*, it is good to keep related files under the respective folder.

    Enter the MDK Rule **Name** `ErrorArchive_DecideWhichEditPage` and click **Finish**.

    <!-- border -->![MDK](img-5.9.png)

    Replace the generated snippet with below code.

    ```JavaScript
    /**
    * Describe this function...
    * @param {IClientAPI} context
    */
    export default function ErrorArchive_DecideWhichEditPage(context) {
        //Current binding's root is the errorArchiveEntity:
        let errorArchiveEntity = context.currentPage.context.binding;
        //Get the affectedEntity object out of it
        let affectedEntity = errorArchiveEntity.AffectedEntity;
        console.log("Affected Entity Is:");
        console.log(affectedEntity);
        let targetAction = null;
        let id = affectedEntity["@odata.id"]; //e.g. SalesOrderHeaders(12345)
        let affectedEntityType = "Unknown Entity Set"; //By default it's unknown type
        if (id.indexOf("(") > 0) {
            //Extracting the entity set type from @odata.id e.g. SalesOrderHeaders
            var patt = /\/?(.+)\(/i;
            var result = id.match(patt);
            affectedEntityType = result[1];
        }
        console.log("Affected Entity Type Is:");
        console.log(affectedEntityType);
        //Here we decide which action to call depends on which affectedEntityType is the affectedEntity
        // You can add more complex decision logic if needed
        switch (affectedEntityType) {
            case "SalesOrderHeaders":
                targetAction = "/mdk_errorarchive/Actions/com_sap_edm_sampleservice_v4/SalesOrderHeaders/NavToSalesOrderHeaders_Edit.action";
                break;
            default:
                //Save the affected Entity's type in client data so that it can be displayed by the toast
                context.getPageProxy().getClientData().AffectedEntityType = affectedEntityType;
                // Show a toast for affectedEntityType that we do not handle yet
                return context.executeAction("/mdk_errorarchive/Actions/ErrorArchive/ErrorArchive_UnknownAffectedEntity.action");
        }
        if (targetAction) {
            let pageProxy = context.getPageProxy();
            //Set the affectedEntity object to root the binding context.
            pageProxy.setActionBinding(affectedEntity);
            //Note: doing 'return' here is important to chain the current context to the action.
            // Without the return the ActionBinding will not be passed to the action because it will consider
            // you are executing this action independent of the current context.
            return context.executeAction(targetAction);
        }
    }
    ```

    >In above code there is a reference to `ErrorArchive_UnknownAffectedEntity.action`, which doesn't exist in your metadata project yet. You will create this action in next step.

9. In the generated `ErrorArchive_DecideWhichEditPage.js` rule, click on the red line. You will notice a yellow bulb icon suggesting some fixes, click on it and then select `MDK: Create action for this reference`, and click `Toast Message Action`.

    <!-- border -->![MDK](img-5.10.gif)

    Provide the below information:

    | Property | Value |
    |----|----|
    | `Message` | `Affected Entity {AffectedEntity/@odata.id} doesn't have handler yet.` |
    | `Duration` | 4 |
    | `Animated` | Select `true` from the dropdown |

    <!-- border -->![MDK](img-5.11.png)

    >If there is no handler for an affected entity, app will display a toast message.

10. Next, add a **Header** section bar to display affected entity information.

    In the Layout Editor, expand the **Controls** | **Section Bar** section, drag and drop the **Header** control onto the **Object Table** control.

    <!-- border -->![MDK](img-5.12.gif)

    Now, bind its **Caption** property to `Affected Entity: {#Page:-Current/AffectedEntity/@odata.type}` target path.

    <!-- border -->![MDK](img-5.13.png)

    >`@odata.type`: an annotation that specifies the type of a JSON object or name/value pair. Its value is a URI that identifies the type of the property or object. More details can be found [here](http://docs.oasis-open.org/odata/odata-json-format/v4.0/cs01/odata-json-format-v4.0-cs01.html#odataType).


### Redeploy the Project

You will now deploy the updated project to your MDK client.

Click the **Deploy** option in the editor's header area, and then choose the deployment target as **Mobile Services** 

<!-- border -->![MDK](img-6.png)

### Update the app

[OPTION BEGIN [Android]]

1. Tap **Check for Updates** in the user menu on the Main page, you will see a _New Version Available_ pop-up, tap **Now**.

    ![MDK](img-7.1.png)

2. In order to access the Error List Detail pages, tap again on **Sync Changes** option in the user menu and once you see *Upload failed* message, tap on **View Errors** to navigate to the Error Archive list.

    There you will find affected entity which couldn't get accepted by backend due to some business logic failure.

    ![MDK](img-4.6.png)
    ![MDK](img-4.7.png)

    >You could add a button on the Main page navigating to the Error Archive List page directly.

3. Tapping any record navigates to Error Details page with more information about error. You have added a business logic to find out which is affected entity and how to navigate to respective record to let users to modify this record with correct values. Once done, user can again sync it with backend. Tap **Edit Affected Entity**.  

    ![MDK](img-7.2.png)

 4. Modify record with correct values.

    ![MDK](img-7.3.png)   

5. Navigate to the Main page and tap on the **Sync Changes** option in the user menu. Record gets upload to the backend successfully.

    ![MDK](img-7.4.png)  

[OPTION END]

[OPTION BEGIN [iOS]]

1. Tap **Check for Updates** in the user menu on the Main page, you will see a _New Version Available_ pop-up, tap **Now**.

    ![MDK](img-7.5.png)

2. In order to access the Error List Detail pages, tap again on **Sync Changes** option in the user menu and once you see *Upload failed* message, tap on **View Errors** to navigate to the Error Archive list.

    There you will find affected entity which couldn't get accepted by backend due to some business logic failure.

    ![MDK](img-4.13.png)
    ![MDK](img-4.14.png)

    >You could add a button on the Main page navigating to the Error Archive List page directly.    

3. Tapping any record navigates to **Error Details** page with more information about error. You have added a business logic to find out which is affected entity and how to navigate to respective record to let users to modify this record with correct values. Once done, user can again sync it with backend. Tap **Edit Affected Entity**.

    ![MDK](img-7.6.png)

4. Modify record with correct values.

    ![MDK](img-7.7.png)   

5. Navigate to the Main page and tap on the **Sync Changes** option in the user menu. Record gets upload to the backend successfully.

    ![MDK](img-7.8.png)  

[OPTION END]

---
