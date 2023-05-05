---
parser: v2
auto_validation: true
primary_tag: software-product>mobile-development-kit-client
tags: [ tutorial>intermediate, operating-system>ios, operating-system>android, topic>mobile, software-product>sap-business-technology-platform, software-product>mobile-development-kit-client, software-product>sap-mobile-services, software-product>sap-business-application-studio]
time: 25
author_name: Jitendra Kansal
author_profile: https://github.com/jitendrakansal
---

# Use OData Annotations to Add CRUD Functionality to an MDK App
<!-- description --> Generate a fully functional CRUD multi-channel application based on OData annotations.

## Prerequisites
- **Tutorial group:** [Set Up for the Mobile Development Kit (MDK)](group.mobile-dev-kit-setup)
- **Install SAP Mobile Services Client** on your [Android](https://play.google.com/store/apps/details?id=com.sap.mobileservices.client) device or [iOS](https://apps.apple.com/us/app/sap-mobile-services-client/id1413653544)
<table><tr><td align="center"><!-- border -->![Play Store QR Code](img-1.1.png)<br>Android</td><td align="center">![App Store QR Code](img-1.2.png)<br>iOS</td></tr></table>
(If you are connecting to `AliCloud` accounts then you will need to brand your [custom MDK client](cp-mobile-dev-kit-build-client) by allowing custom domains.)

## You will learn
  - How MDK editor parses OData Annotations for a given OData service
  - How to create a fully functional multi-channel application

## Intro
You may clone an existing project from [GitHub repository](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/tree/main/4-Level-Up-with-the-Mobile-Development-Kit/5-Use-OData-Annotations-to-Add-CRUD-Functionality-to-an-MDK-App) and start directly with step 5 in this tutorial but make sure you complete step 2.

---


Mobile Development Kit brings OData annotations capabilities to your multi-channel applications. MDK editor supports generating List-Detail pages based on annotations. List-Detail pages are similar to a Master-Detail page, but it is two pages instead of one. The MDK editor parses existing annotations to give you a huge leap forward in your multi-channel applications.

![MDK](img-1.0.gif)

### Understand the SAP Fiori Elements


If you are a Fiori app designer, you may already be familiar with OData annotations and smart templates.

SAP Fiori elements provide designs for UI patterns and predefined templates for common application use cases. App developers can use SAP Fiori elements to create SAP Fiori applications based on OData services and annotations. With little or no coding, you can create SAP Fiori applications. UI5 has a Web solution, named smart templates, that builds a starter application by parsing the annotations in your OData service.

You can also check out more information on the Fiori elements [List Report](https://experience.sap.com/fiori-design-ios/article/list-report/) and [Smart templates](https://experience.sap.com/fiori-design-web/smart-templates/)


### Add annotation information in the backend destination


Sample backend in SAP Mobile Services provides annotation functionality for **Products**. If you add annotation path in given backend endpoint, the same annotation information can be leveraged by MDK editor to generate related CRUD pages.

Make sure you have already configured an app in Mobile Services cockpit and have added Sample service as per step 3 in [this](cp-mobile-dev-kit-ms-setup) tutorial.

1. In SAP MDK Demo App configuration, click **Mobile Connectivity**.

    <!-- border -->![MDK](img-2.1.png)

2. Click **Edit** icon to add annotation path to the `SampleServiceV2` destination.

    <!-- border -->![MDK](img-2.2.png)

3. In following steps, let the existing settings as it is.

    In **Annotations** step, click **Add Annotation URL** to add OData Annotations to the Sample service.

    Provide the below information, click on the empty area to trigger the Next button clickable and then click **Next**:

    | Field | Value |
    |----|----|
    | `Annotation Name`| `Product` |
    | `Path/File` | `/annotations/Products` |

    <!-- border -->![MDK](img-2.3.png)

4. In the following steps, let the default settings as it is. Click **Finish**.

    Navigate to the `SampleServiceV2` destination info, you can see that OData Annotation information is updated in the `SampleServiceV2` destination.

    <!-- border -->![MDK](img-2.4.png)


### Create a new MDK project in SAP Business Application Studio


This step includes creating the mobile development kit project in the editor.

1. Launch the [Dev space](cp-mobile-bas-setup) in SAP Business Application Studio.

2. Click **Start from template** on the `Get Started` page.

    <!-- border -->![MDK](img-3.1.png)

    >If you do not see the `Get Started` page, you can access it by typing `>get started` in the center search bar.

    <!-- border -->![MDK](img-1.2.gif)

3. Select **MDK Project** and click **Start**.

    <!-- border -->![MDK](img-3.2.png)

    >If you do not see the **MDK Project** option check if your Dev Space has finished loading or reload the page in your browser and try again.

    >This screen will only show up when your CF login session has expired. Enter your login credentials, click Sign in. After successful signed in to Cloud Foundry, select your Cloud Foundry Organization and Space where you have set up the initial configuration for your MDK app and click Apply.

    ><!-- border -->![MDK](img-1.4.png)

4. In *Basic Information* step, select or provide the below information and click **Next**:

    | Field | Value |
    |----|----|
    | `MDK Template Type`| Select `Base` from the dropdown |
    | `Your Project Name` | Provide a name of your choice. `MDK_Annotations` is used for this tutorial |
    | `Your Application Name` | <default name is same as project name, you can provide any name of your choice> |
    | `Target MDK Client Version` | Leave the default selection as `MDK 6.0+ (For use with MDK 6.0 or later clients)` |
    | `Choose a target folder` | By default, the target folder uses project root path. However, you can choose a different folder path |

    <!-- border -->![MDK](img-3.3.png)

    >More details on _MDK template_ is available in [help documentation](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/getting-started/mdk/bas.html#creating-a-new-project-cloud-foundry).  


5. In *Service configuration* step, provide or select the below information and click **Finish**:

    | Field | Value |
    |----|----|
    | `Data Source` | Select `Mobile Services` from the dropdown |
    | `Mobile Services Landscape` | Select `standard` from the dropdown |
    | `Application Id` | Select `com.sap.mdk.demo` from the dropdown |
    | `Destination` | Select `SampleServiceV2` from the dropdown |
    | `Enter a path to the OData service` | Leave it as it is |
    | `Enable Offline` | Choose `No` |   

    <!-- border -->![MDK](img-3.5.png)

    Regardless of whether you are creating an online or offline application, this step is needed for the app to connect to an OData service. When building an Mobile Development Kit application, it assumes the OData service created and the destination that points to this service is set up in Mobile Services and SAP Business Technology Platform.

    Since you will create an online based app, hence _Enable Offline Store_ option is unchecked.

6. After clicking **Finish**, the wizard will generate your MDK Application based on your selections. You should now see the `MDK_Annotations` project in the project explorer.


### Add MDK Annotation component to MDK project


1. Right-click `Application.app` and select **MDK:New Annotation Component**.

    <!-- border -->![MDK](img-4.1.png)

2. In the **Annotation Selection** step, MDK editor fetches annotation details. Select **Product** Annotation and click **Next**.

    <!-- border -->![MDK](img-4.2.png)

3. In **Template Customization** step, select **CRUD** Template and click **Finish**.

    <!-- border -->![MDK](img-4.3.png)

    In MDK project, you will see new pages, actions, rules have been generated for **Products**.

    <!-- border -->![MDK](img-4.4.png)

    You will also notice a button control has been generated on the `Main.page` to navigate to `Product_List.page`.

    Pages, actions and rules created are a starting point. You can edit those pages and make it your own.  At this point the MDK editor is no longer reading the annotations from OData.

    >If the OData designer updates the backend services data schema (annotations), the MDK pages will stay as originally created. It will not automatically update the pages or overwrite your changes. You are *disconnected* from the annotations at this point.



### Deploy the application


So far, you have learned how to build an MDK application in the SAP Business Application Studio editor. Now, you will deploy the application definitions to Mobile Services and Cloud Foundry to use it in the Mobile client and Web application respectively.

1. Right-click `Application.app` and select **MDK: Deploy**.

    <!-- border -->![MDK](img-5.1.png)

2. Select deploy target as **Mobile & Cloud**.

    MDK editor will deploy the metadata to Mobile Services (for Mobile application) followed by to Cloud Foundry (for Web application).

    <!-- border -->![MDK](img-5.2.png)

    You should see successful messages for both deployments.

    <!-- border -->![MDK](img-5.3.png)

### Display the QR code for onboarding the Mobile app


SAP Business Application Studio has a feature to display the QR code for onboarding in the Mobile client.

Click the **Application.app** to open it in MDK Application Editor and then click the **Application QR Code** icon.

<!-- border -->![MDK](img-6.1.png)

The On-boarding QR code is now displayed.

<!-- border -->![MDK](img-6.2.png)


>Leave the Onboarding dialog box open for the next step.




### Run the app


[OPTION BEGIN [Android]]

>Make sure you are choosing the right device platform tab above. Once you have scanned and on-boarded using the onboarding URL, it will be remembered. When you Log out and onboard again, you will be asked either to continue to use current application or to scan new QR code.

1. Follow [these steps](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/blob/master/Onboarding-Android-client/Onboarding-Android-client.md) to on-board the MDK client.

    After you accept app update, tap on the **Products** to navigate to the Products List page.

    ![MDK](img-6.3.png)

2. In following pages, you can create a new record, modify an existing record, open the product image and even delete the record.

    ![MDK](img-6.4.png)
    ![MDK](img-6.5.png)

[OPTION END]


[OPTION BEGIN [iOS]]

>Make sure you are choosing the right device platform tab above. Once you have scanned and on-boarded using the onboarding URL, it will be remembered. When you Log out and onboard again, you will be asked either to continue to use current application or to scan new QR code.

SAP Business Application Studio has a feature to display the QR code for onboarding in the Mobile client.

1. Click the **Application.app** to open it in MDK Application Editor and then click the **Application QR Code** icon.

    <!-- border -->![MDK](img-6.1.png)

    The On-boarding QR code is now displayed.

    <!-- border -->![MDK](img-6.2.png)

3. Follow [these steps](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/blob/master/Onboarding-iOS-client/Onboarding-iOS-client.md) to on-board the MDK client.

    After you accept app update, tap on the **Products** to navigate to the Products List page.

    ![MDK](img-6.6.png)

4. In following pages, you can create a new record, modify an existing record, open the product image and even delete the record.

    ![MDK](img-6.7.png)
    ![MDK](img-6.8.png)

[OPTION END]

[OPTION BEGIN [Web]]

1. Click the highlighted button to open the MDK Web application in a browser. Enter your SAP BTP credentials if asked.

    <!-- border -->![MDK](img-6.9.png)

    >You can also open the MDK web application by accessing its URL from `.project.json` file.
    <!-- border -->![MDK](img-6.10.png)

2.  After you accept app update, click on the **Products** to navigate to the Products List page.

    <!-- border -->![MDK](img-6.11.png)

3.  In following pages, you can create a new record, modify an existing record, open the product image and even delete the record.

    <!-- border -->![MDK](img-6.12.png)
    <!-- border -->![MDK](img-6.13.png)

[OPTION END]


---
