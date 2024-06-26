---
parser: v2
auto_validation: true
primary_tag: software-product>mobile-development-kit-client
tags: [ tutorial>intermediate, operating-system>ios, operating-system>android, topic>mobile, software-product>sap-business-technology-platform, software-product>mobile-development-kit-client, software-product>sap-mobile-services, software-product>sap-business-application-studio ]
time: 30
author_name: Jitendra Kansal
author_profile: https://github.com/jitendrakansal
---

# Consume a REST API in an MDK App
<!-- description --> Create a fully functional multi-channel application consuming Petstore REST API.

## Prerequisites
- **Tutorial:** [Set Up Business Application Studio for Mobile Technologies](https://developers.sap.com/tutorials/cp-mobile-bas-setup.html)
- **Install SAP Mobile Services Client** on your [Android](https://play.google.com/store/apps/details?id=com.sap.mobileservices.client) device or [iOS](https://apps.apple.com/us/app/sap-mobile-services-client/id1413653544)
<table><tr><td align="center"><!-- border -->![Play Store QR Code](img-1.1.1.png)<br>Android</td><td align="center">![App Store QR Code](img-1.1.2.png)<br>iOS</td></tr></table>
(If you are connecting to `AliCloud` accounts, you will need to brand your [custom MDK client](https://developers.sap.com/tutorials/cp-mobile-dev-kit-build-client.html) by allowing custom domains.)

## You will learn
  - How to configure an application in Mobile Services
  - How to define a REST endpoint as a destination in Mobile Services
  - How to define a REST endpoint as a destination in Cloud Foundry
  - How to create MDK applications in SAP Business Application Studio
  - How to create a MDK Service file pointing to REST endpoint destination
  - How to use `RestService SendRequest` Action to make directly call to `Petstore` API

## Intro
You may clone an existing metadata project from the [MDK Tutorial GitHub repository](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/tree/main/4-Level-Up-with-the-Mobile-Development-Kit/7-Consume-rest-api-in-mdk-app) and start directly with step 15 in this tutorial but make sure you complete step 2&3.

---

Mobile Development Kit allows you to consume REST APIs. You need to first define REST endpoint as a destination and then easily bind a `RestServiceTarget` to an MDK control e.g., `ObjectTable`, `ContactCell`, `ObjectCollection` etc. This assumes the REST service returns JSON similar to how OData requests are returned.

A publicly available `Petstore` API from [swagger.io](https://petstore.swagger.io) is used as an example in this tutorial.

![MDK](img-1.0.png)

### Understand the Petstore API to retrieve data

1. Open *[`Swagger Petstore`](https://petstore.swagger.io/)*, find all pets with status as `available`.

    <!-- border -->![MDK](img-1.1.png)

2. Click **Execute** to get the response.

    <!-- border -->![MDK](img-1.2.png)

    By looking at results, you now have understood

    -	what is the Request URL to retrieve pet information
    -	what is header parameter to be passed in GET call
    -	what is the response code
    -	how the response body looks like



### Configure new MDK app in Mobile Services cockpit

You will now configure an app in Mobile Services, add root of `Petstore` URL as a destination and then consume it in MDK.

1. Navigate to [SAP Mobile Services cockpit on Cloud Foundry environment](https://developers.sap.com/tutorials/fiori-ios-hcpms-setup.html).

2. On the home screen, select **Create new app** or navigate to **Mobile Applications** **&rarr;** **Native/MDK** **&rarr;** **New**.

    <!-- border -->![MDK](img-2.1.png)

3. In the **Basic Info** step, provide the required information and click **Next**.

    | Field | Value |
    |----|----|
    | `ID` | `com.sap.mdk.restapi` |
    | `Name` | `SAP MDK REST API` |

    <!-- border -->![MDK](img-2.2.png)

    > If you are configuring this app in a trial account, make sure to select **License Type** as *lite*.

    >Other fields are optional. For more information about these fields, see [Creating Applications](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/getting-started/admin/manage.html#creating-applications) in the help documentation.

4. In the **XSUAA Settings** step, continue with the default settings and click **Next** to navigate to further steps.

    <!-- border -->![MDK](img-2.3.png)

5. In the **Role Settings** step, leave the settings as it is and click **Next** to navigate to further steps.

    <!-- border -->![MDK](img-2.3.1.png)        

6. In the **Assign Features** step, choose **Mobile Development Kit Application** from the dropdown if not there already, and Click **Finish**.

    <!-- border -->![MDK](img-2.4.png)

    >If you see a _Application without Role Settings_ warning message, click **OK**. You may assign roles after the app has been configured, if needed.    

7. Click **Mobile Connectivity** to add `petstore` root API as a destination.

    <!-- border -->![MDK](img-2.5.png)

8. Click **+** icon to add a new destination.  

    <!-- border -->![MDK](img-2.6.png)

9. Provide the required information and click **Next**.

    | Field | Value |
    |----|----|
    | `Destination Name` | `swagger_petstore` |
    | `URL` | `https://petstore.swagger.io/v2` |

    <!-- border -->![MDK](img-2.7.png)

10. For this tutorial, there is no Custom Headers, Annotations, Authentication required, click **Next** and Finish the form.

### Create a new destination to your MDK Web application


1. Download the zip file from [here](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/raw/main/4-Level-Up-with-the-Mobile-Development-Kit/7-Consume-rest-api-in-mdk-app/swagger_petstore.zip) and unzip it on your machine.

2. Navigate to **Connectivity** **&rarr;** **Destinations** to create a BTP destination, click **Import Destination** to import the extracted file and click **Save**.

    <!-- border -->![MDK](img-3.2.png)


### Create a new MDK project in SAP Business Application Studio


1. Launch the [Dev space](https://developers.sap.com/tutorials/cp-mobile-bas-setup.html) in SAP Business Application Studio.


2. Click **New Project from Template** on the `Get Started` page.

    <!-- border -->![MDK](img-4.1.png)

    >If you do not see the `Get Started` page, you can access it by typing `>get started` in the center search bar.

    <!-- border -->![MDK](img-1.2.gif)

3. Select **MDK Project** and click **Start**. If you do not see the **MDK Project** option check if your Dev Space has finished loading or reload the page in your browser and try again.

    <!-- border -->![MDK](img-4.2.png)  

    >This screen will only show up when your CF login session has expired. Use either `Credentials` OR  `SSO Passcode` option for authentication. After successful signed in to Cloud Foundry, select your Cloud Foundry Organization and Space where you have set up the initial configuration for your MDK app and click Apply.

    ><!-- border -->![MDK](img-1.4.png)

4. In *Basic Information* step, provide the below information and click **Finish**:

    | Field | Value |
    |----|----|
    | `MDK Template Type`| Select `Empty` from the dropdown |
    | `Your Project Name` | Provide a name of your choice. `MDK_Petstore` is used for this tutorial |
    | `Your Application Name` | <default name is same as project name, you can provide any name of your choice> |
    | `Target MDK Client Version` | Leave the default selection as `MDK 23.4+ (For use with MDK 23.4 or later clients)` |
    | `Choose a target folder` | By default, the target folder uses project root path. However, you can choose a different folder path |

    <!-- border -->![MDK](img-4.3.png)

    >More details on _MDK template_ is available in [help documentation](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/getting-started/mdk/bas.html#creating-a-new-project-cloud-foundry).

5. After clicking **Finish**, the wizard will generate your MDK Application based on your selections. You should now see the `MDK_Petstore` project in the project explorer.


### Create a new MDK Service file


1. Right-click the **Services** folder | **MDK: New Service**.

    <!-- border -->![MDK](img-5.1.png)

2. Provide the below information and click **Finish**.

    | Field | Value |
    |----|----|
    | `Name`| `petstore` |
    | `Data Source` | Select `Mobile Services` from the dropdown. You will be asked to select the Mobile services landscape where you have configured the MDK app as per step 2 and then select the application `com.sap.mdk.restapi` |
    | `Destination` | Select `swagger_petstore` from the dropdown |
    | `Path Suffix` | Leave it as it is |
    | `Language URL Param` | Leave it as it is |
    | `REST Service` | choose this option |

    <!-- border -->![MDK](img-5.4.png)

    `.service` and `.xml` (empty file) have been created under the **Services** folder.

    <!-- border -->![MDK](img-5.5.png)


### Display Pets list in MDK page


You will add an **Object Table** control  item on `Main.page` to display the list of Pets.

1. In the Layout Editor, expand the **Controls** | **Data Bound Container** group, drag and drop the **Object Table** control onto the `Main.page` area.

    <!-- border -->![MDK](img-6.1.gif)

2. Provide the required information for **Target** section:

    | Field | Value |
    |----|----|
    | `Target` | Select `REST Service Target` from the dropdown |
    | `Service` | Select `petstore.service` from the dropdown |
    | `Path` | Enter `/pet/findByStatus?status=available` |

    <!-- border -->![MDK](img-6.2.png)

    >You can find more details on **Target** in [documentation](https://help.sap.com/doc/69c2ce3e50454264acf9cafe6c6e442c/Latest/en-US/docs-en/reference/schemadoc/definitions/Target.schema.html).

    >Method GET is pre-selected for binding the `ObjectTable` control to a `RestServiceTarget`.

3. Under **Appearance**, provide below information:

    | Property | Value |
    |----|----|
    | `Description`| leave it empty |
    | `Footnote`| leave it empty |
    | `PreserveIconStackSpacing` | Select `false` from the dropdown |
    | `ProgessIndicator`| leave it empty |
    | `Status` | `{status}` |
    | `Subhead` | `Pet Name: {name}` |
    | `Substatus` | leave it empty |
    | `Tags` | Click the `item0` and click the trash icon to delete the default item |
    | `Title` | `Pet ID: {id}` |

    <!-- border -->![MDK](img-6.4.png)

4. In the **Avatar Grid** section of the **Properties** pane, remove the default Avatar.  First, click on the `item0`, a trash icon appears. Click on the trash icon to delete the default item.

    <!-- border -->![MDK](img-6.5.png)    

5. In the **Avatar Stack** section of the **Property** pane, remove the default Avatar.  First, click on the `item0`, a trash icon appears. Click on the trash icon to delete the default item.

    <!-- border -->![MDK](img-6.6.png)  

    >If you see any error in Main.page (code editor), ignore it as MDK editor currently can't validate such REST Service endpoint properties.


### Deploy the application


So far, you have learned how to build an MDK application in the SAP Business Application Studio editor. Now, you will deploy the application definitions to Mobile Services and Cloud Foundry to use it in the Mobile client and Web application respectively.

1. Right-click `Application.app` and select **MDK: Deploy**.

    <!-- border -->![MDK](img-7.1.png)

2. Select deploy target as **Mobile & Cloud**.

    MDK editor will deploy the metadata to Mobile Services (for Mobile application) followed by to Cloud Foundry (for Web application).

    <!-- border -->![MDK](img-7.2.png)

    Ensure that you see successful messages for both deployments.

    <!-- border -->![MDK](img-7.3.png)


### Display the QR code for onboarding the Mobile app

SAP Business Application Studio has a feature to display the QR code for onboarding in the Mobile client. Click on `Application.app` to open it in MDK Application Editor, and then click the **Application QR Code** icon.

.

<!-- border -->![MDK](img-8.1.png)

<!-- border -->![MDK](img-8.2.png)

### Run the app


[OPTION BEGIN [Android]]

>Ensure that you choose the correct device platform tab above. Once you have scanned and onboarded using the onboarding URL, it will be remembered. If you log out and onboard again, you will be prompted to either continue using the current application or scan a new QR code.

Follow [these steps](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/blob/main/Onboarding-Android-client/Onboarding-Android-client.md) to successfully on-board the MDK client on your Android device.

After accepting the app update, you will see the Pets list on the **Main** page.

![MDK](img-8.3.png)

[OPTION END]

[OPTION BEGIN [iOS]]

>Ensure that you choose the correct device platform tab above. Once you have scanned and onboarded using the onboarding URL, it will be remembered. If you log out and onboard again, you will be prompted to either continue using the current application or scan a new QR code.

Follow [these steps](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/blob/main/Onboarding-iOS-client/Onboarding-iOS-client.md) to successfully on-board the MDK client on your iOS device.

After accepting the app update, you will see the Pets list on the **Main** page.

![MDK](img-8.4.png)

[OPTION END]

[OPTION BEGIN [Web]]

Click the highlighted button to open the MDK Web application in a browser. Enter your SAP BTP credentials if asked.

<!-- border -->![MDK](img-8.5.png)

>You can also open the MDK web application by accessing its URL from `.project.json` file.
<!-- border -->![MDK](img-8.6.png)

You will see the Pets list on the **Main** page.

<!-- border -->![MDK](img-8.7.png)


[OPTION END]

Congratulations, you have learned how to consume a REST API in MDK app to display Pets list.

Next, you will learn how to create a new pet record.


### Understand the Petstore API to create new record


1. In [`Swagger Petstore`](https://petstore.swagger.io/), add a new pet to the store.

    <!-- border -->![MDK](img-9.1.png)

    There is payload example to be passed for adding a new pet.

2. For testing, use below payload.

    ```JSON
    {
      "name": "pet-test",
      "status": "available"
    }
    ```

3. Click **Execute** to get the response.

    <!-- border -->![MDK](img-9.2.png)

    By looking at results, you now have understood

    -	what is the Request URL & body to create a new pet record
    -	what are header parameters to be passed in POST call
    -	what is the response code
    -	how the response body looks like

With above details, you will now create a new MDK rule to create a new Pet record.


### Create new page for new pet record

In this step, you will create a Section page with a Form Cell Section to contain the Form Cell controls. You will add the fields that will be editable by the end-user.

1. Right-click the **Pages** folder | **MDK: New Page** | **Section** | **Next**.

    <!-- border -->![MDK](img-10.1.png)

2. In the **Base Information** step, enter the Page **Name** as `Pet_Create` and click **Finish**.

    <!-- border -->![MDK](img-10.2.png)

3. In the **Properties** pane, set the **Caption** to **Create Pet**.

    <!-- border -->![MDK](img-10.3.png)

4. Now, you will add the fields (Pet name and Status) for creating a new pet record by the end-user. In the Layout Editor, expand the **Static Container** group. Drag and drop **Form Cell** section onto the Page area. 

    <!-- border -->![MDK](img-10.3.1.gif)

    >Form Cell section is used to contain Form Cell controls in a section page.

5. You will now add Form Cell controls in the Form Cell Section. Expand the **Form Cell Controls** group, drag and drop a **Simple Property** onto the Page area.

    <!-- border -->![MDK](img-10.4.gif)

6. Drag and drop one more Simple Property control onto the page so you have two total controls.

    <!-- border -->![MDK](img-10.5.png)

7. Select the first **Simple Property control** and provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `FCCreatePet` |
    | `Caption` | `Pet Name` |
    | `PlaceHolder`| `Enter Value` |

    <!-- border -->![MDK](img-10.6.png)

8. Select the second **Simple Property control** and provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `FCCreateStatus` |
    | `Caption` | `Status` |
    | `PlaceHolder`| `Enter Value` |

    <!-- border -->![MDK](img-10.7.png)    


### Send data to the Backend


After filling-up the details for creating a new pet record, you will send these data to the backend.   

1. In `Pet_Create.page`, drag & drop an action bar item to the right corner of the action bar.

    <!-- border -->![MDK](img-11.1.png)

2. In the **Properties** pane, click the **link icon** to open the object browser for the **System Item** property. Double click the **Save** type and click **OK**.

    <!-- border -->![MDK](img-11.2.png)

3. Navigate to **Events** tab, click three dots icon and click `create a new rule/action`.

    <!-- border -->![MDK](img-11.3.png)

4. Keep the default selection for *Object Type* (as Action) and *Folder* path.   

    <!-- border -->![MDK](img-11.4.png)

5. Choose **`REST Service`** in **Category** | select **`REST Service Send Request` Action** | **Next**.

    <!-- border -->![MDK](img-11.5.png)        

6. In the **Base Information**, provide the below information:

    | Field | Value |
    |----|----|
    | `Name`| `CreatePet` |
    | `Service` | choose `petstore.service` from the dropdown |
    | `Path` | `/pet` |    

    <!-- border -->![MDK](img-11.6.png)

8. For `RequestProperties` object, choose `POST` method from the dropdown. Under `Body`, switch to `object type` by clicking the icon, once it's color has changed, click on `Body[0]` to add array items, this should now display a create icon in front of `Body[0]`. Click Create icon to create an array item(0).

    Provide the below information:

    | Field | Value |
    |----|----|
    | `Key`| `name` |
    | `Value`| Bind it to input control `#Control:FCCreatePet/#Value` |

    <!-- border -->![MDK](img-11.7.gif)

9. Click create icon to add another array item(1) and click **Finish**. 

    | Field | Value |
    |----|----|
    | `Key`| `status` |
    | `Value`| Bind it to input control `#Control:FCCreateStatus/#Value` |

    <!-- border -->![MDK](img-11.8.png)

    >You can find more details about `SendRequest` action in [help documentation](https://help.sap.com/doc/69c2ce3e50454264acf9cafe6c6e442c/Latest/en-US/docs-en/reference/schemadoc/Action/RestService/SendRequest.schema.html).

10. When the `CreatePet.action` is successful, you may want to close the page. Expand the **Common Action Properties** and bind the **Success Action** to the `CloseModalPage_Complete.action`.

    <!-- border -->![MDK](img-14.1.png)

### Add cancel button on create pet page

Now, you will add a button on the `Pet_Create.page` and set it's `onPress` to `ClosePage.action`.

1. Drag and drop an **Action Bar Item** to the left corner of the action bar.

    <!-- border -->![MDK](img-12.1.png)

2. In the **Properties** pane, click the **link icon** to open the object browser for the **System Item** property. Double click the **Cancel** type and click **OK**.

    <!-- border -->![MDK](img-12.2.png)

    >System Item are predefined system-supplied icon or text. Overwrites _Text_ and _Icon_ if specified.

3. Now, you will set the `onPress` event to `ClosePage.action`.

    In **Events** tab, click the 3 dots icon for the `OnPress` property to open the **Object Browser**.

    Double click the `ClosePage.action` and click **OK** to set it as the `OnPress` Action.

    <!-- border -->![MDK](img-12.3.png)


### Navigate to create a new Pet Record


You will add a button to the `Main.page` called **Add**. When you click on this button, you want to navigate to the create pet page.

1. In `Main.page`, drag and drop an **Action Bar Item** to the upper right of the action bar.

2. Click the **link icon** to open the object browser for the `SystemItem` property.

    Double click the **Add** type and click **OK**.

    <!-- border -->![MDK](img-13.1.png)

3. Navigate to the **Events** tab, click the 3 dots icon for the `OnPress` property to open the **Object Browser**.

    <!-- border -->![MDK](img-13.2.png)

 4. Keep the default selection for *Object Type* (as Action) and *Folder* path.   

    <!-- border -->![MDK](img-11.4.png)

5. Choose **UI** in **Category** | click **Navigation** | **Next**.

6. In the **Base Information**, provide the below information and click **Finish**.

    | Property | Value |
    |----|----|
    | `Name`| `NavToPet_Create` |
    | `PageToOpen` | Select `Pet_Create.page` from the dropdown |
    | `ModalPage`| Select `true` from the dropdown |

    <!-- border -->![MDK](img-13.3.png)

### Redeploy the application

Right-click the `Application.app` file in the project explorer pane,  select **MDK: Deploy** and then select deploy target as **Mobile & Cloud**.

>Alternatively, you can select *MDK: Redeploy* in the command palette (View menu>Find Command OR press Command+Shift+p on Mac OR press Ctrl+Shift+P on Windows machine), it will perform the last deployment.

><!-- border -->![MDK](img-15.1.png)


### Update the app


[OPTION BEGIN [Android]]

1. Tap **Check for Updates** in the user menu on the Main page, you will see a _New Version Available_ pop-up, tap **Now**.

    ![MDK](img-16.1.png)

2. Tap **+** icon on `Main.page` to navigate to Create Pet page.

    ![MDK](img-1.0.png)

3.  Fill out the details to create a new Pet record.

    ![MDK](img-16.3.png)

[OPTION END]

[OPTION BEGIN [iOS]]

1. Tap **Check for Updates** in the user menu on the Main page, you will see a _New Version Available_ pop-up, tap **Now**.

    ![MDK](img-16.4.png)

2. Tap **+** icon on `Main.page` to navigate to Create Pet page.

    ![MDK](img-16.5.png)

3.  Fill out the details to create a new Pet record.

    ![MDK](img-16.6.png)

[OPTION END]

[OPTION BEGIN [Web]]

1. Either click the highlighted button or refresh the web page to load the changes.

    <!-- border -->![MDK](img-16.7.png)

2. Tap Add button on `Main.page` to navigate to Create Pet page.

    <!-- border -->![MDK](img-16.8.png)

3.  Fill out the details to create a new Pet record.

    <!-- border -->![MDK](img-16.9.png)

[OPTION END]

You have created a new record consuming REST API. Similarly, you can also modify and delete an existing record.



---
