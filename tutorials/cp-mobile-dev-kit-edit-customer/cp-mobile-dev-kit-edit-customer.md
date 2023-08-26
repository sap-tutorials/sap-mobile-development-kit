---
parser: v2
auto_validation: true
primary_tag: software-product>mobile-development-kit-client
tags: [ tutorial>beginner, operating-system>ios, operating-system>android, topic>mobile, software-product>sap-business-technology-platform, software-product>mobile-development-kit-client, software-product>sap-mobile-services, software-product>sap-business-application-studio ]
time: 25
author_name: Jitendra Kansal
author_profile: https://github.com/jitendrakansal
---

# Modify a Customer Record in an MDK App
<!-- description --> Allow editing of customer details in an MDK app.

## You will learn
  - How to create a new page for modifying customer details such as name, email and phone number
  - How to store changes locally on Mobile app and sync these changes with backend
  - How to update a record in web application

---

## Intro
![MDK](img-1.0.gif)

### Create a new page for modifying customer data


Regardless of whether your application is online or offline, you can allow users to modify data in the application.

For online applications, the changes are saved to the backend immediately.

For offline applications, the changes are stored locally until they are synced using an Upload action.

In this step, you will create a Section page with a Form Cell Section to contain the Form Cell controls. The page will provide only a subset of items available on the Customer Detail page. You will add the fields that will be editable by the end-user.

1. Right-click the **Pages** folder | **MDK: New Page** | **Section** | **Next**.

    <!-- border -->![MDK](img-1.1.png)

2. In the **Base Information** step, enter the Page Name `Customers_Edit` and click **Finish** to complete the page creation process.

    <!-- border -->![MDK](img-1.2.png)

3. In the **Properties** pane set the Caption to **Update Customer**.

    <!-- border -->![MDK](img-1.3.png)

4. Now, you will add the fields (like first name, last name, phone & email address) that will be editable by the end-user. In the Layout Editor, expand the **Static Container** group. Drag and drop **Form Cell Section** onto the Page area.

    <!-- border -->![MDK](img-1.3.1.gif)

    >Form Cell Section is used to contain Form Cell controls in a section page.

5. You will now add Form Cell controls in the Form Cell Section. Expand the **Form Cell Controls** group, drag and drop a **Simple Property** onto the Page area.

    <!-- border -->![MDK](img-1.4.gif)

6. Drag and drop three additional Simple Property controls onto the page so you have four total controls.

    <!-- border -->![MDK](img-1.5.png)

7. Select the first **Simple Property control** and provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `FCFirstName` |
    | `Caption` | `First Name` |
    | `Value`| click the link icon and bind it to `FirstName` property of the Customer entity |

    <!-- border -->![MDK](img-1.6.png)

8. Select the second Simple Property control and provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `FCLastName` |
    | `Caption` | `Last Name` |
    | `Value`| click the link icon and bind it to `LastName` property of the Customer entity |

    <!-- border -->![MDK](img-1.7.png)

9. Select the third Simple Property control and provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `FCPhone` |
    | `Caption` | `Phone` |
    | `KeyboardType` | `Phone` |
    | `Value`| click the link icon and bind it to `PhoneNumber` property of the Customer entity |

    <!-- border -->![MDK](img-1.8.png)

    >To streamline data entry, the keyboard displayed when editing a `SimplePropertyFormCell` should be appropriate for the type of content in the field. If your app asks for number, for example, it should display the phone keyboard.

10. Select the last Simple Property control and provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `FCEmail` |
    | `Caption` | `Email` |
    | `KeyboardType` | `Email` |
    | `Value`| click the link icon and bind it to `EmailAddress` property of the Customer entity |

    <!-- border -->![MDK](img-1.9.png)

    >To streamline data entry, the keyboard displayed when editing a `SimplePropertyFormCell` should be appropriate for the type of content in the field. If your app asks for an email address, for example, it should display the email address keyboard.


### Add a cancel button on the Edit Customer page


While updating the customer details, you may want to close the current page and cancels or interrupts any execution in process.

1. In `Customers_Edit.page`, drag and drop an **Action Bar Item** control to the upper left corner of the action bar.

    >Action Bar Item is a button that users can use to fire actions when pressed. You can add an Action Bar Item only to the Action Bar (at the top of the page).

    <!-- border -->![MDK](img-2.1.gif)

2. In the Properties pane, click the **link icon** to open the object browser for the **System Item** property.

    Double-click the **Cancel** type and click **OK**.

    <!-- border -->![MDK](img-2.2.gif)

    >System Item are predefined system-supplied icon or text. Overwrites _Text_ and _Icon_ if specified.

3. Navigate to the **Events** tab. Click the 3 dots icon for the `OnPress` property and select the `Create a rule/action`.

    <!-- border -->![MDK](img-2.3.png)

4. Keep the default selection for the *Object Type* as Action and *Folders* path.

    <!-- border -->![MDK](img-2.4.png)   

5. Choose **UI** in **Category** | click **Close Page** | **Next**.

    <!-- border -->![MDK](img-2.5.png)

6. In the **Base Information**, provide the below information and then click **Finish** to complete the action creation process.

    | Property | Value |
    |----|----|
    | `Name`| `CloseModalPage_Cancel` |
    | `DismissModal` | Select `Canceled` from the dropdown |
    | `CancelPendingActions`| Select `true` from the dropdown |

    <!-- border -->![MDK](img-2.6.png)

    >You can close pages with the option to terminate ongoing events or wait until they are complete. Visit [documentation](https://help.sap.com/doc/69c2ce3e50454264acf9cafe6c6e442c/Latest/en-US/docs-en/reference/schemadoc/Action/ClosePage.schema.html) for more details about Close Page Action.

### Store the updated data locally


The next step is to store the updated record locally for an offline application or send the updated record directly back to the backed for online applications.

* You will now add an Action Bar item on the `Customers_Edit.page` that will call an OData Update Entity action to save the record
* You may want to close the page when the update action is successful
* You may want to show a failure message if the update action fails to save the changes

First, add an action bar item on the `Customers_Edit.page`

1.  In `Customers_Edit.page`, **drag and drop** an **Action Bar Item** to the upper right corner of the action bar.

    <!-- border -->![MDK](img-3.1.png)

2. Click the **link** icon to open the object browser for the **System Item** property. Double-click the **Save** type and click **OK**.

    <!-- border -->![MDK](img-3.2.png)

3. Navigate to the **Events** tab. Click the 3 dots icon for the `OnPress` property and select the `Create a rule/action`.

    <!-- border -->![MDK](img-3.3.png)

4. Keep the default selection for the *Object Type* as Action and *Folders* path.

    <!-- border -->![MDK](img-2.4.png)   

5. Choose **Data** in **Category** | click **OData** | **Next**.

    <!-- border -->![MDK](img-3.5.png)

6. In the **Base Information** step, provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `Customers_UpdateEntity` |
    | `Type` | Select `UpdateEntity` from the dropdown |
    | `Service`| Select `SampleServiceV2.service` from the dropdown |
    | `EntitySet`| Select `Customers` from the dropdown |
    | `ReadLink`| click link icon and double-click `readLink` |

    <!-- border -->![MDK](img-3.6.png)

    >This action will map the changes to the correct entities in the OData service and save the changes.

    >The `readLink` is a direct reference to an individual entity set entry.

7. Click **Next**.

8.  In **Property and Update Links** step, uncheck **City**.

9.  Since in `Customers_Detail.page`, you defined four properties (First Name, Last Name, Phone & Email) to be edited, now, in this step, you will bind them to respective UI Controls.

    Check the `EmailAddress` property and click the **link icon** to open the object browser.

    Change the drop down in the object browser to `Controls & ClientData`, click the **Current Page** radio button.

    In the search box start typing the control name `FCEmail`. The list will filter down to show the matching values. Double-click the **Value (Value)** entry under the `FCEmail` field and click **OK** to set binding.

    <!-- border -->![MDK](img-3.7.gif)

10.  Repeat the above step for remaining properties: `FirstName`, `LastName` and `PhoneNumber`.

    <!-- border -->![MDK](img-3.8.png)

    Click **Finish**. The action editor will open with the `Customers_UpdateEntity.action` loaded.

    >You can find more details about [Update Entity Action](https://help.sap.com/doc/69c2ce3e50454264acf9cafe6c6e442c/Latest/en-US/docs-en/reference/schemadoc/Action/ODataService/UpdateEntity.schema.html).

11. When the above OData action is executed, you may want to display messages on its success and failure behavior. For example, on its success, you may want to close the page and allow any execution to continue. On its failure, you may want to display an error.

    In the `Customers_UpdateEntity.action`, scroll down and expand the *Common Action Properties* section. Click the `Create a rule/action` icon for the *Success Action*.

    <!-- border -->![MDK](img-3.9.png)

12. Keep the default selection for the *Object Type* as Action and *Folders* path.

    <!-- border -->![MDK](img-2.4.png)   

13. Choose **UI** in **Category** | click **Close Page** | **Next**.

    <!-- border -->![MDK](img-3.10.png)

14. In the **Base Information**, provide the below information and then click **Finish** to complete the action creation process.

    | Property | Value |
    |----|----|
    | `Name` | `CloseModalPage_Complete` |
    | `DismissModal` | Select `Completed` from the dropdown |
    | `CancelPendingActions` | Select `false` from the dropdown |

    <!-- border -->![MDK](img-3.11.png)


15. Similar, create a message action displaying error in case of the update failure. In the `Customers_UpdateEntity.action`, click the `Create a rule/action` icon for the *Failure Action*.

    <!-- border -->![MDK](img-3.12.png)

16. Keep the default selection for the *Object Type* as Action and *Folders* path.

    <!-- border -->![MDK](img-3.4.png)   

17. Choose **Message** in **Category** | click **Message** | **Next**.

    <!-- border -->![MDK](img-3.13.png)

18. In the **Base Information**, provide the below information and then click **Finish**.

    | Property | Value |
    |----|----|
    | `Name`| `UpdateCustomerEntityFailureMessage` |
    | `Type` | Select `Message` from the dropdown |
    | `Message` | `Failed to Save Customer Updates - {#ActionResults:Customers_UpdateEntity/error}` |
    | `Title` | `Update Customer` |
    | `OKCaption` | `OK` |
    | `OnOK` | `--None--` |
    | `CancelCaption` | leave it blank |
    | `OnCancel` | `--None--` |

    <!-- border -->![MDK](img-3.14.png)

    >`Customers_UpdateEntity` is the Action Result value of the `Customers_UpdateEntity.action`. This reference is used to pass the results to subsequent actions in the chain. These actions can reference the action result as needed. In this case if there is a failure, you access the error property of the action result to display the OData failure message.

    >This is the standard Binding Target Path (also called Dynamic Target Path) syntax used when you need to include a binding with other bindings or within a string as used in the message here.

    >You could exclude above expression and can just display a generic message.

When `Customers_UpdateEntity.action` gets executed successfully then `CloseModalPage_Complete.action` will be triggered or if `Customers_UpdateEntity.action` fails then `UpdateCustomerEntityFailureMessage.action` will be triggered.



### Navigate to the Customer Edit page


You will navigate from the Customer Detail page to a new page modifying customer information. For this, you will add an action bar item on the Details page and will link it to a navigation action. When the action bar item is pressed by the end-user that will open the `Customers_Edit.page`.


1. In `Customers_Detail.page`, drag and drop an **Action Bar Item** to the upper right of the action bar.

    <!-- border -->![MDK](img-4.1.png)    

2. Click the **link icon** to open the object browser for the **System Item** property.

    Double-click the **Edit** type and click **OK**.

    <!-- border -->![MDK](img-4.2.png)

3. Navigate to the **Events** tab. Click the 3 dots icon for the `OnPress` property and select the `Create a rule/action`.

    <!-- border -->![MDK](img-4.3.png)
    
4. Keep the default selection for the *Object Type* as Action and *Folders* path.

    <!-- border -->![MDK](img-2.4.png)    

5. Choose **UI** in **Category** | click **Navigation** | **Next**.

    <!-- border -->![MDK](img-4.4.png)

6. Provide the below information:

    | Property | Value |
    |----|----|
    | `Action Name`| `NavToCustomers_Edit` |
    | `PageToOpen` | Select `Customers_Edit.page` from the dropdown |
    | `ModalPage`| Select `true` from the dropdown |

    <!-- border -->![MDK](img-4.6.png)

7. Click **Finish** to complete the action creation process. 


### Deploy the application


Deploy the updated application to your MDK client.

1. Right-click `Application.app` and select **MDK: Deploy**.

    <!-- border -->![MDK](img-5.1.png)

2. Select deploy target as **Mobile & Cloud**.

    <!-- border -->![MDK](img-5.2.png)

    You should see success message for both deployments.

    <!-- border -->![MDK](img-5.3.png)

    >Alternatively, you can select *MDK: Redeploy* in the command palette (View menu>Find Command OR press Command+Shift+p on Mac OR press Ctrl+Shift+P on Windows machine), it will perform the last deployment.

    ><!-- border -->![MDK](img-5.4.png)



### Run the app


>Make sure you are choosing the right device platform tab above.

[OPTION BEGIN [Android]]

1. Tap **Update** on the Main page, you will see a _New Version Available_ pop-up, tap **Now**.

    ![MDK](img-6.1.png)

2. Tap **Customers**, tap one of the available customer record, you will then navigate to Customer detail page. Tap `edit` icon. Update existing values for the given customer. You will notice the Phone keyboard appears when updating Phone value. Tap save icon and record gets updated locally.

    ![MDK](img-6.2.gif)

4. You can cross verify if the record has been updated in the backend.

    >Backend endpoint can be found in [Mobile Services Cockpit](cp-mobile-dev-kit-ms-setup).

    >**Mobile Applications** | **Native/MDK** | click the MDK App **com.sap.mdk.demo** | **Mobile Connectivity** | click **Launch in Browser** icon

    ><!-- border -->![MDK](img-6.3.png)

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

2. Tap **Customers**, tap one of the available customer record, you will then navigate to Customer detail page. Tap `edit` icon. Update existing values for the given customer. You will notice the Phone keyboard appears when updating Phone value. Tap save icon and record gets updated locally.

    ![MDK](img-6.8.gif)

3. You can cross verify if the record has been updated in the backend.

    >Backend endpoint can be found in [Mobile Services Cockpit](cp-mobile-dev-kit-ms-setup).

    >**Mobile Applications** | **Native/MDK** | click the MDK App **com.sap.mdk.demo** | **Mobile Connectivity** | click **Launch in Browser** icon

    ><!-- border -->![MDK](img-6.3.png)

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

    <!-- border -->![MDK](img-6.10.png)

    >If you see the error `404 Not Found: Requested route ('xxxxx-dev-nsdemosampleapp-approuter.cfapps.xxxx.hana.ondemand.com') does not exist.` while accessing the web application, make sure that in your space cockpit, highlight applications are in started state.

    ><!-- border -->![MDK](img-6.11.png)

2. Click **Customers**, click one of the available customer record,  you will then navigate to Customer detail page.

    <!-- border -->![MDK](img-6.12.png)

3. Click **Edit**.

    <!-- border -->![MDK](img-6.13.png)

4. For example, updating First Name from `Isabelle` to `Carolina`. Click **Save**.

    <!-- border -->![MDK](img-6.14.png)

    Record gets updated accordingly.

    <!-- border -->![MDK](img-6.15.png)

5. You can cross verify if the record has been updated in the backend.

    >Backend endpoint can be found in [Mobile Services Cockpit](cp-mobile-dev-kit-ms-setup).

    >**Mobile Applications** | **Native/MDK** | click the MDK App **com.sap.mdk.demo** | **Mobile Connectivity** | click **Launch in Browser** icon

    ><!-- border -->![MDK](img-6.3.png)

    >It will open the URL in a new tab, remove `?auth=uaa` and add `/Customers` at the end of the URL.

[OPTION END]


---
