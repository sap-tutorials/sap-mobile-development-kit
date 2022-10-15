---
title: Modify a Customer Record in an MDK App
description: Allow editing of customer details in an MDK app.
auto_validation: true
primary_tag: software-product>mobile-development-kit-client
tags: [ tutorial>beginner, operating-system>ios, operating-system>android, topic>mobile, software-product>sap-business-technology-platform, software-product>mobile-development-kit-client, software-product>sap-mobile-services, software-product>sap-business-application-studio ]
time: 25
author_name: Jitendra Kansal
author_profile: https://github.com/jitendrakansal
---

## Details
### You will learn
  - How to create a new page for modifying customer details such as name, email and phone number
  - How to store changes locally on Mobile app and sync these changes with backend
  - How to update a record in web application

---

![MDK](img-1.0.gif)

[ACCORDION-BEGIN [Step 1: ](Create a new page for modifying customer data)]

Regardless of whether your application is online or offline, you can allow users to modify data in the application.

For online applications, the changes are saved to the backend immediately.

For offline applications, the changes are stored locally until they are synced using an Upload action.

In this step, you will create the _Edit Customer Detail_ page as a **Form Cell Page**. This type of page allows for form input style changes. The page will provide only a subset of items available on the Customer Detail page. You will add the fields that will be editable by the end-user.

1. Right-click the **Pages** folder | **MDK: New Page** | **Form Cell Page** | **Next**.

    !![MDK](img-1.1.png)

    >A Form Cell Page is suitable for pages that generate new objects or modify existing objects. It includes a form cell container by default. You can add multiple containers or action controls to this page. Under each container section, you can add various controls.

    >You can find more details about [Form Cell page](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/features/fiori-ui/mdk/formcell-page.html).

2. Enter the Page Name `Customers_Edit` and click **Next** and the **Finish** on the Confirmation step.

    !![MDK](img-1.2.png)

3. In the **Properties** pane set the Caption to **Update Customer**.

    !![MDK](img-1.3.png)

4. Now, you will add the fields (like first name, last name, phone & email address) that will be editable by the end-user.

    In the Layout Editor, expand the **Controls** group.

    Drag and drop a **Simple Property** onto the Page area.

    !![MDK](img-1.4.gif)

5. Drag and drop three additional Simple Property controls onto the page so you have four total controls.

    !![MDK](img-1.5.png)

6. Select the first **Simple Property control** and provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `FCFirstName` |
    | `Caption` | `First Name` |
    | `Value`| click the link icon and bind it to `FirstName` property of the Customer entity |

    !![MDK](img-1.6.png)

7. Select the second Simple Property control and provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `FCLastName` |
    | `Caption` | `Last Name` |
    | `Value`| click the link icon and bind it to `LastName` property of the Customer entity |

    !![MDK](img-1.7.png)

8. Select the third Simple Property control and provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `FCPhone` |
    | `Caption` | `Phone` |
    | `KeyboardType` | `Phone` |
    | `Value`| click the link icon and bind it to `PhoneNumber` property of the Customer entity |

    !![MDK](img-1.8.png)

    >To streamline data entry, the keyboard displayed when editing a `SimplePropertyFormCell` should be appropriate for the type of content in the field. If your app asks for number, for example, it should display the phone keyboard.

9. Select the last Simple Property control and provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `FCEmail` |
    | `Caption` | `Email` |
    | `KeyboardType` | `Email` |
    | `Value`| click the link icon and bind it to `EmailAddress` property of the Customer entity |

    !![MDK](img-1.9.png)

    >To streamline data entry, the keyboard displayed when editing a `SimplePropertyFormCell` should be appropriate for the type of content in the field. If your app asks for an email address, for example, it should display the email address keyboard.

[VALIDATE_2]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 2: ](Add a cancel button on the Edit Customer page)]

While updating the customer details, you may want to close the current page and cancels or interrupts any execution in process.

1. In `Customers_Edit.page`, drag and drop an **Action Bar Item** control to the upper left corner of the action bar.

    >Action Bar Item is a button that users can use to fire actions when pressed. You can add an Action Bar Item only to the Action Bar (at the top of the page).

    !![MDK](img-2.1.gif)

2. In the Properties pane, click the **link icon** to open the object browser for the **System Item** property.

    Double-click the **Cancel** type and click **OK**.

    !![MDK](img-2.2.gif)

    >System Item are predefined system-supplied icon or text. Overwrites _Text_ and _Icon_ if specified.

3. Navigate to the **Events** tab. Click the 3 dots icon for the `OnPress` property and select the `Create a rule/action`.

    !![MDK](img-2.3.png)

4. Keep the default selection for the *Object Type* as Action and *Folders* path.

    !![MDK](img-2.4.png)   

5. Choose **MDK UI Actions** in **Category** | click **Close Page Action** | **Next**.

    !![MDK](img-2.5.png)

6. Provide the below information:

    | Property | Value |
    |----|----|
    | `Action Name`| `CloseModalPage_Cancel` |
    | `DismissModal` | Select `Canceled` from the dropdown |
    | `CancelPendingActions`| Select `true` from the dropdown |

    !![MDK](img-2.6.png)

    >You can close pages with the option to terminate ongoing events or wait until they are complete. Visit [documentation](https://help.sap.com/doc/69c2ce3e50454264acf9cafe6c6e442c/Latest/en-US/docs-en/reference/schemadoc/Action/ClosePage.schema.html) for more details about Close Page Action.

7. Click **Next** and then **Finish** on the confirmation step.

[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 3: ](Store the updated data locally)]

The next step is to store the updated record locally for an offline application or send the updated record directly back to the backed for online applications.

* You will now add an Action Bar item on the `Customers_Edit.page` that will call an OData Update Entity action to save the record
* You may want to close the page when the update action is successful
* You may want to show a failure message if the update action fails to save the changes

First, add an action bar item on the `Customers_Edit.page`

1.  In `Customers_Edit.page`, **drag and drop** an **Action Bar Item** to the upper right corner of the action bar.

    !![MDK](img-3.1.png)

2. Click the **link** icon to open the object browser for the **System Item** property. Double-click the **Save** type and click **OK**.

    !![MDK](img-3.2.png)

3. Navigate to the **Events** tab. Click the 3 dots icon for the `OnPress` property and select the `Create a rule/action`.

    !![MDK](img-3.3.png)

4. Keep the default selection for the *Object Type* as Action and *Folders* path.

    !![MDK](img-3.4.png)   

5. Choose **MDK Data Actions** in **Category** | click **OData Action** | **Next**.

    !![MDK](img-3.5.png)

6. In the **Operation and Service Selection** step, provide the below information:

    | Property | Value |
    |----|----|
    | `Action Name`| `Customers_UpdateEntity` |
    | `Type` | Select `UpdateEntity` from the dropdown |
    | `Service`| Select `SampleServiceV2.service` from the dropdown |
    | `EntitySet`| Select `Customers` from the dropdown |
    | `ReadLink`| click link icon and Double-click `readLink` |

    !![MDK](img-3.6.png)

    >This action will map the changes to the correct entities in the OData service and save the changes.

    >The `readLink` is a direct reference to an individual entity set entry.

7. Click **Next**.

8.  In **Property and Update Links** step, uncheck **City**.

9.  Since in `Customers_Detail.page`, you defined four properties (First Name, Last Name, Phone & Email) to be edited, now, in this step, you will bind them to respective UI Controls.

    Check the `EmailAddress` property and click the **link icon** to open the object browser.

    Change the drop down in the object browser to `Controls & ClientData`, click the **Current Page** radio button.

    In the search box start typing the control name `FCEmail`. The list will filter down to show the matching values. Double-click the **Value (Value)** entry under the `FCEmail` field and click **OK** to set binding.

    !![MDK](img-3.7.gif)

10.  Repeat the above step for remaining properties: `FirstName`, `LastName` and `PhoneNumber`.

    !![MDK](img-3.8.png)

    Click **Next** and **Finish** on the confirmation screen. The action editor will open with the `Customers_UpdateEntity.action` loaded.

    >You can find more details about [Update Entity Action](https://help.sap.com/doc/69c2ce3e50454264acf9cafe6c6e442c/Latest/en-US/docs-en/reference/schemadoc/Action/ODataService/UpdateEntity.schema.html).

11. When the above OData action is executed, you may want to display messages on its success and failure behavior. For example, on its success, you may want to close the page and allow any execution to continue. On its failure, you may want to display an error.

    In the `Customers_UpdateEntity.action`, scroll down and expand the *Common Action Properties* section. Click the `Create a rule/action` icon for the *Success Action*.

    !![MDK](img-3.9.png)

12. Keep the default selection for the *Object Type* as Action and *Folders* path.

    !![MDK](img-3.4.png)   

13. Choose **MDK UI Actions** in **Category** | click **Close Page Action** | **Next**.

    !![MDK](img-3.10.png)

14. Provide the below information:

    | Property | Value |
    |----|----|
    | `Action Name` | `CloseModalPage_Complete` |
    | `DismissModal` | Select `Completed` from the dropdown |
    | `CancelPendingActions` | Select `false` from the dropdown |

    !![MDK](img-3.11.png)

    Click **Next** and then **Finish** on the confirmation step.

15. Similar, create a message action displaying error in case of the update failure. In the `Customers_UpdateEntity.action`, provide value as **update** for the *Action Result* and click the `Create a rule/action` icon for the *Failure Action*.

    !![MDK](img-3.12.png)

16. Keep the default selection for the *Object Type* as Action and *Folders* path.

    !![MDK](img-3.4.png)   

17. Choose **MDK Message Actions** in **Category** | click **Message Action** | **Next**.

    !![MDK](img-3.13.png)

18. Provide the below information:

    | Property | Value |
    |----|----|
    | `Action Name`| `UpdateCustomerEntityFailureMessage` |
    | `Type` | Select `Message` from the dropdown |
    | `Message` | `Failed to Save Customer Updates - {#ActionResults:update/error}` |
    | `Title` | `Update Customer` |
    | `OKCaption` | `OK` |
    | `OnOK` | `--None--` |
    | `CancelCaption` | leave it blank |
    | `OnCancel` | `--None--` |

    !![MDK](img-3.14.png)

    >`update` is the Action Result value of the `Customers_UpdateEntity.action`. This reference is used to pass the results to subsequent actions in the chain. These actions can reference the action result as needed. In this case if there is a failure, you access the error property of the action result to display the OData failure message.

    >This is the standard Binding Target Path (also called Dynamic Target Path) syntax used when you need to include a binding with other bindings or within a string as used in the message here.

    >You could exclude above expression and can just display a generic message.
    
19. Click **Next** and then **Finish** on the confirmation step.

When `Customers_UpdateEntity.action` gets executed successfully then `CloseModalPage_Complete.action` will be triggered or if `Customers_UpdateEntity.action` fails then `UpdateCustomerEntityFailureMessage.action` will be triggered.

[VALIDATE_4]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 4: ](Navigate to the Customer Edit page)]

You will navigate from the Customer Detail page to a new page modifying customer information. For this, you will add an action bar item on the Details page and will link it to a navigation action. When the action bar item is pressed by the end-user that will open the `Customers_Edit.page`.

1. In `Customers_Detail.page`, drag and drop an **Action Bar Item** to the upper right of the action bar.

    !![MDK](img-4.1.png)

2. Click the **link icon** to open the object browser for the **System Item** property.

    Double-click the **Edit** type and click **OK**.

    !![MDK](img-4.2.png)

3. Navigate to the **Events** tab. Click the 3 dots icon for the `OnPress` property and select the `Create a rule/action`.

   !![MDK](img-4.3.png)

4. Keep the default selection for the *Object Type* as Action and *Folders* path.

    !![MDK](img-3.4.png)    

5. Choose **MDK UI Actions** in **Category** | click **Navigation Action** | **Next**.

    !![MDK](img-4.4.png)

6. Provide the below information:

    | Property | Value |
    |----|----|
    | `Action Name`| `NavToCustomers_Edit` |
    | `PageToOpen` | Select `Customers_Edit.page` from the dropdown |
    | `ModalPage`| Select `true` from the dropdown |

    !![MDK](img-4.6.png)

7. Click **Next** and then **Finish** on the confirmation step.

[DONE]
[ACCORDION-END]


[ACCORDION-BEGIN [Step 5: ](Deploy the application)]

Deploy the updated application to your MDK client.

1. Right-click `Application.app` and select **MDK: Deploy**.

    !![MDK](img-5.1.png)

2. Select deploy target as **Mobile & Cloud**.

    !![MDK](img-5.2.png)

    You should see success message for both deployments.

    !![MDK](img-5.3.png)

    >Alternatively, you can select *MDK: Redeploy* in the command palette (View menu>Find Command OR press Command+Shift+p on Mac OR press Ctrl+Shift+P on Windows machine), it will perform the last deployment.

    >!![MDK](img-5.4.png)

[VALIDATE_3]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 6: ](Run the app)]

>Make sure you are choosing the right device platform tab above.

[OPTION BEGIN [Android]]

1. Tap **Update** on the Main page, you will see a _New Version Available_ pop-up, tap **Now**.

    ![MDK](img-6.1.png)

2. Tap **Customers**, tap one of the available customer record, you will then navigate to Customer detail page. Tap `edit` icon. Update existing values for the given customer. You will notice the Phone keyboard appears when updating Phone value. Tap save icon and record gets updated locally.

    ![MDK](img-6.2.gif)

4. You can cross verify if the record has been updated in the backend.

    >Backend endpoint can be found in [Mobile Services Cockpit](cp-mobile-dev-kit-ms-setup).

    >**Mobile Applications** | **Native/MDK** | click the MDK App **com.sap.mdk.demo** | **Mobile Connectivity** | click **Launch in Browser** icon

    >!![MDK](img-6.3.png)

    >It will open the URL in a new tab, remove `?auth=uaa` and add `/Customers` at the end of the URL.

    But here result is pointing to old values.

    ![MDK](img-6.4.png)

    Since this is an Offline application, new entry is added to the request queue of the local store which needs to be sent or uploaded to the backend explicitly.  

    >MDK Base template has added a **Sync** button on main page of the app to upload local changes from device to the backend and to download the latest changes from backend to the device. Actions | Service | `UploadOffline.action` & `DownloadOffline.action`.

5. On Main page, tap **Sync**, a successful message will be shown.

    ![MDK](img-6.5.png)

Now, refresh the URL to check if record has been updated in the backend. As Sync is pressed, `UploadOffline.action` gets triggered to upload local changes from device to the backend and on success of this call, `DownloadOffline.action` is being called.

![MDK](img-6.6.png)

[OPTION END]

[OPTION BEGIN [iOS]]

1. Tap **Update** on the Main page, you will see a _New Version Available_ pop-up, tap **Now**.

    ![MDK](img-6.7.png)

2. 2. Tap **Customers**, tap one of the available customer record, you will then navigate to Customer detail page. Tap `edit` icon. Update existing values for the given customer. You will notice the Phone keyboard appears when updating Phone value. Tap save icon and record gets updated locally.

    ![MDK](img-6.8.gif)

3. You can cross verify if the record has been updated in the backend.

    >Backend endpoint can be found in [Mobile Services Cockpit](cp-mobile-dev-kit-ms-setup).

    >**Mobile Applications** | **Native/MDK** | click the MDK App **com.sap.mdk.demo** | **Mobile Connectivity** | click **Launch in Browser** icon

    >!![MDK](img-6.3.png)

    >It will open the URL in a new tab, remove `?auth=uaa` and add `/Customers` at the end of the URL.

    But here result is pointing to old values.

    ![MDK](img-6.4.png)

    Since this is an Offline application, new entry is added to the request queue of the local store which needs to be sent or uploaded to the backend explicitly.  

    >MDK Base template has added a **Sync** button on main page of the app to upload local changes from device to the backend and to download the latest changes from backend to the device. Actions | Service | `UploadOffline.action` & `DownloadOffline.action`.

4. On Main page, tap **Sync**, a successful message will be shown.

    ![MDK](img-6.9.png)

Now, refresh the URL to check if record has been updated in the backend. As Sync is pressed, `UploadOffline.action` gets triggered to upload local changes from device to the backend and on success of this call, `DownloadOffline.action` is being called.

![MDK](img-6.6.png)

[OPTION END]

[OPTION BEGIN [Web]]

1. Either click the highlighted button or refresh the web page to load the changes.

    !![MDK](img-6.10.png)

    >If you see the error `404 Not Found: Requested route ('xxxxx-dev-nsdemosampleapp-approuter.cfapps.xxxx.hana.ondemand.com') does not exist.` while accessing the web application, make sure that in your space cockpit, highlight applications are in started state.

    >!![MDK](img-6.11.png)

2. Click **Customers**, click one of the available customer record,  you will then navigate to Customer detail page.

    !![MDK](img-6.12.png)

3. Click **Edit**.

    !![MDK](img-6.13.png)

4. For example, updating First Name from `Isabelle` to `Carolina`. Click **Save**.

    !![MDK](img-6.14.png)

    Record gets updated accordingly.

    !![MDK](img-6.15.png)

5. You can cross verify if the record has been updated in the backend.

    >Backend endpoint can be found in [Mobile Services Cockpit](cp-mobile-dev-kit-ms-setup).

    >**Mobile Applications** | **Native/MDK** | click the MDK App **com.sap.mdk.demo** | **Mobile Connectivity** | click **Launch in Browser** icon

    >!![MDK](img-6.3.png)

    >It will open the URL in a new tab, remove `?auth=uaa` and add `/Customers` at the end of the URL.

[OPTION END]

[DONE]
[ACCORDION-END]

---
