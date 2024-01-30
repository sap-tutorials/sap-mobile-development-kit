---
parser: v2
auto_validation: true
primary_tag: software-product>mobile-development-kit-client
tags: [ tutorial>intermediate, operating-system>ios, operating-system>android, topic>mobile, software-product>sap-business-technology-platform, software-product>mobile-development-kit-client, software-product>sap-mobile-services, software-product>sap-business-application-studio ]
time: 25
author_name: Jitendra Kansal
author_profile: https://github.com/jitendrakansal
---

# Create a Customer Record in an MDK App
<!-- description --> Allow the user to create a customer record in an MDK app.

## Prerequisites
- You have created an MDK app as described in [Create Your First Mobile App with the Mobile Development Kit (MDK)](mission.mobile-dev-kit-get-started).

## You will learn
  - How to create a customer record
  - How to store changes locally on Mobile app and sync these changes with backend
  - How to update a record in web application

## Intro
If you didn't follow the prerequisite then You may clone an existing metadata project from the [MDK Tutorial GitHub repository](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/tree/main/2-Create-Your-First-Mobile-App-with-the-Mobile-Development-Kit) to start with this tutorial.

---

![MDK](img-1.0.gif)

### Create new page for new customer record



In this step, you will create a Section page with a Form Cell Section to contain the Form Cell controls. You will add the fields that will be editable by the end-user.

1. Right-click the **Pages** folder | **MDK: New Page** | **Section** | **Next**.

    <!-- border -->![MDK](img-1.1.png)
    
2. In the **Base Information** step, enter the page **Name** as `Customers_Create` and click **Finish** to complete the page creation wizard.

    <!-- border -->![MDK](img-1.2.png)

3. In the **Properties** pane, set the **Caption** to **Create Customer**.

    <!-- border -->![MDK](img-1.3.png)

4. Now, you will add the fields (like first name, last name, phone, email address & date of birth) for creating a new customer record by the end-user. In the Layout Editor, expand the **Static Container** group. Drag and drop **Form Cell** section onto the Page area.

    <!-- border -->![MDK](img-1.3.1.gif)

    >Form Cell section is used to contain Form Cell controls in a section page.
    
5. You will now add Form Cell controls in the Form Cell section. Expand the **Form Cell Controls** group, drag and drop a **Simple Property** onto the Page area.

    <!-- border -->![MDK](img-1.4.gif)

6. Drag and drop three additional Simple Property controls onto the page so you have four total controls.

    <!-- border -->![MDK](img-1.5.png)

7. Select the first **Simple Property control** and provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `FCCreateFirstName` |
    | `Caption` | `First Name` |
    | `PlaceHolder`| `Enter Value` |

    <!-- border -->![MDK](img-1.6.png)

8. Select the second **Simple Property control** and provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `FCCreateLastName` |
    | `Caption` | `Last Name` |
    | `PlaceHolder`| `Enter Value` |

    <!-- border -->![MDK](img-1.7.png)

9. Select the third **Simple Property control** and provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `FCCreatePhone` |
    | `Caption` | `Phone` |
    | `KeyboardType` | `Phone` |
    | `PlaceHolder`| `Enter Value` |

    <!-- border -->![MDK](img-1.8.png)

    >To streamline data entry, the keyboard displayed when editing a `SimplePropertyFormCell` should be appropriate for the type of content in the field. If your app asks for number, for example, it should display the phone keyboard.

10. Select the last **Simple Property control** and provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `FCCreateEmail` |
    | `Caption` | `Email` |
    | `KeyboardType` | `Email` |
    | `PlaceHolder`| `Enter Value` |

    <!-- border -->![MDK](img-1.9.png)

    >[`KeyboardType`](https://help.sap.com/doc/69c2ce3e50454264acf9cafe6c6e442c/Latest/en-US/docs-en/reference/schemadoc/Page/FormCell/SimpleProperty.schema.html#keyboardtype) streamlines the data entry. When entering an email address, it would display the email address keyboard in mobile clients.

11. Drag and drop a **Date Picker** control onto the page area for date of birth parameter.

    <!-- border -->![MDK](img-1.10.gif)

    Provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `FCCreateDOB` |
    | `Caption` | `DOB` |


### Add a cancel button on the Create Customer page


While creating a customer record, you may want to close the current page and cancels or interrupts any execution in process.

Now, you will add a button on the Create Customer page and set it's `onPress` to `CloseModalPage_Cancel.action`.

1. Drag and drop an **Action Bar Item** to the upper left corner of the action bar.

    >Action Bar Item is a button that users can use to fire actions when pressed. You can add an Action Bar Item only to the Action Bar (at the top of the page).

    <!-- border -->![MDK](img-2.1.png)

2. In the **Properties** pane, click the **link icon** to open the **Object Browser** for the **System Item** property. Double click the **Cancel** type and click **OK**.

    <!-- border -->![MDK](img-2.2.png)

    >System Item are predefined system-supplied icon or text. Overwrites _Text_ and _Icon_ if specified.

3. Now, you will set the `onPress` event to `CloseModalPage_Cancel.action`.

    In **Events** tab, click the 3 dots icon for the `OnPress` property to open the **Object Browser**. Double click the `CloseModalPage_Cancel.action` and click **OK** to set it as the `OnPress` Action.

    <!-- border -->![MDK](img-2.3.png)


### Store the created data locally


The next step is to store newly created record locally for an offline application or send the new record directly back to the backed for online applications. 

* You will add an Action Bar item on the `Customers_Create.page` that will call an OData Create Entity action to save the record
* You may want to close the page when the OData Create Entity action is successful
* You may want to show a failure message if the OData Create Entity action fails to save the changes

First, add an action bar item on the `Customers_Create.page`.

1.  In `Customers_Create.page`, **drag and drop** an **Action Bar Item** to the upper right corner of the action bar.

    <!-- border -->![MDK](img-3.1.png)

2. Click the **link** icon to open the object browser for the **System Item** property. Double-click the **Save** type and click **OK**.

3. Navigate to the **Events** tab. Click the 3 dots icon for the `OnPress` property and select the `Create a rule/action`.

    <!-- border -->![MDK](img-3.2.png)

4. Keep the default selection for the *Object Type* as Action and *Folders* path.

    <!-- border -->![MDK](img-3.3.png)  

5. In the **Template Selection**, choose **Data** in **Category** | click **OData** Action | **Next**.

    <!-- border -->![MDK](img-3.4.png)  

6. In the **Base Information** step, provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `Customers_CreateEntity` |
    | `Type` | Select `CreateEntity` from the dropdown |
    | `Service`| Select `SampleServiceV4.service` from the dropdown |
    | `EntitySet` | Select `Customers` from the dropdown |

    <!-- border -->![MDK](img-3.5.png)

7. Click **Next**.

8. In **Property and Create Links** step, uncheck **City**.

    Since in `Customer_Create.page`, you defined five properties (First Name, Last Name, Phone, Email & date of birth) to be added, now in this step, you will bind them to respective UI Controls.

    Check the `DateOfBirth` property and click the **link icon** to open the object browser.

    Change the drop down in the object browser to `Controls & ClientData`, click the **Current Page** radio button.

    In the search box start typing the control name `FCCreateDOB`. The list will filter down to show the matching values. Double click the **Value (Value)** entry under the `FCCreateDOB` field and click **OK** to set binding.

    <!-- border -->![MDK](img-3.6.gif)

9. Repeat the above step for remaining properties: `EmailAddress`, `FirstName`, `LastName` and `PhoneNumber`.

    <!-- border -->![MDK](img-3.7.png)

 10. Click **Finish** to complete the action creation process. The action editor will open with the `Customers_CreateEntity.action` loaded.

    >You can find more details about [Create Entity Action](https://help.sap.com/doc/69c2ce3e50454264acf9cafe6c6e442c/Latest/en-US/docs-en/reference/schemadoc/Action/ODataService/CreateEntity.schema.html).     

 11. When the above OData action is executed, you may want to display messages on its success and failure behavior. For example, on its success, you may want to close the page and allow any execution to continue. On its failure, you may want to display an error.  

    In the `Customers_CreateEntity.action`, scroll down and expand the *Common Action Properties* section. Click the link icon to open the object browser for the *Success Action* and bind it to `CloseModalPage_Complete.action`.

    <!-- border -->![MDK](img-3.9.png)

 12. Create a message action displaying error in case of the update failure. In the `Customers_CreateEntity.action`, click the `Create a rule/action` icon for the *Failure Action*.

    <!-- border -->![MDK](img-3.10.png)

 13. Keep the default selection for the *Object Type* as Action and *Folders* path.

    <!-- border -->![MDK](img-3.3.png)     

14. In the **Template Selection** step, choose **Message** in **Category** | click **Message** Action | **Next**.

    <!-- border -->![MDK](img-3.11.png)

15. In the **Base Information** step, provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `CreateCustomerEntityFailureMessage` |
    | `Type` | Select `Message` from the dropdown |
    | `Message` | `Failed to Create Customer record - {#ActionResults:Customers_CreateEntity/error}` |
    | `Title` | `Create Customer` |
    | `OKCaption` | `OK` |
    | `OnOK` | `--None--` |
    | `CancelCaption` | leave it blank |
    | `OnCancel` | `--None--`|

    <!-- border -->![MDK](img-3.12.png)

    >`Customers_CreateEntity` is the Action Result value of the `Customers_CreateEntity.action`. This reference is used to pass the results to subsequent actions in the chain. These actions can reference the action result as needed. In this case if there is a failure, you access the error property of the action result to display the OData failure message.

    >This is the standard Binding Target Path (also called Dynamic Target Path) syntax used when you need to include a binding with other bindings or within a string as used in the message here.

    >You could exclude above expression and can just display a generic message.

16. Click **Finish** to complete the action creation process.

    When `Customers_CreateEntity.action` gets executed successfully then `CloseModalPage_Complete.action` will be triggered or if `Customers_CreateEntity.action` fails then `CreateCustomerEntityFailureMessage.action` will be triggered.



### Navigate to the Customer Create page


You will open the `Customers_Create.page` from the Customer List page. For this, you will add an action bar item on the List page and will link it to a navigation action. When the action bar item is pressed by the end-user that will open the `Customers_Create.page`.

1. In `Customers_List.page`, drag and drop an **Action Bar Item** to the upper right of the action bar.

    <!-- border -->![MDK](img-4.1.png)

2. Click the **link icon** to open the object browser for the `SystemItem` property. Double click the **Add** type and click **OK**.

    <!-- border -->![MDK](img-4.2.png)

3. Navigate to the **Events** tab. Click the 3 dots icon for the `OnPress` property and select the `Create a rule/action`.    

    <!-- border -->![MDK](img-4.3.png)

4. Keep the default selection for the *Object Type* as Action and *Folders* path.

    <!-- border -->![MDK](img-3.3.png)       

5. In the **Template Selection** step, choose **UI** in **Category** | click **Navigation** | **Next**.

    <!-- border -->![MDK](img-4.4.png)

6. In the **Base Information** step, provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `NavToCustomers_Create` |
    | `PageToOpen` | Select `Customers_Create.page` from the dropdown |
    | `ModalPage`| Select `true` from the dropdown |

    <!-- border -->![MDK](img-4.5.png)

7. Click **Next** and then **Finish** on the confirmation step.


### Deploy the application


Deploy the updated application to your MDK client.

1. Right-click `Application.app` and select **MDK: Deploy**.

    <!-- border -->![MDK](img-6.1.png)

2. Select deploy target as **Mobile & Cloud**.

    <!-- border -->![MDK](img-6.2.png)

    You should see success message for both deployments.

    <!-- border -->![MDK](img-6.3.png)

    >Alternatively, you can select *MDK: Redeploy* in the command palette (View menu>Find Command OR press Command+Shift+p on Mac OR press Ctrl+Shift+P on Windows machine), it will perform the last deployment.

    ><!-- border -->![MDK](img-6.4.png)


### Run the app


>Make sure you are choosing the right device platform tab above.

[OPTION BEGIN [Android]]

1. Tap **Check for Updates** in the user menu on the Main page, you will see a _New Version Available_ pop-up, tap **Now**.

    ![MDK](img-7.1.png)

2. Tap **Customers**, click **+** icon to create a new record.

    ![MDK](img-7.2.png)

3. Enter the values, and tap save icon. Local record gets created accordingly.

    ![MDK](img-1.0.gif)

    Since this is an Offline application, new entry is added to the request queue of the local store which needs to be sent or uploaded to the backend explicitly.  

    >MDK template has added a **Sync Changes** option in the user menu on main page of the app to upload local changes from device to the backend and to download the latest changes from backend to the device. Actions | Service | `UploadOffline.action` & `DownloadOffline.action`.

4. Tap on **Sync Changes** in the user menu On Main page to push the local changes to the backend, a successful message will be shown once data is submitted.

    ![MDK](img-7.3.png)

[OPTION END]

[OPTION BEGIN [iOS]]

1. Tap **Check for Updates** in the user menu on the Main page, you will see a _New Version Available_ pop-up, tap **Now**.

    ![MDK](img-7.4.png)

2. Tap **Customers**, click **+** icon to create a new record.

    ![MDK](img-7.5.png)

3. Enter the values, and tap **Save**.

    ![MDK](img-7.6.png)

    Local record gets created accordingly.

    ![MDK](img-7.7.png)

    Since this is an Offline application, new entry is added to the request queue of the local store which needs to be sent or uploaded to the backend explicitly.  

    >MDK template has added a **Sync Changes** option in the user menu on main page of the app to upload local changes from device to the backend and to download the latest changes from backend to the device. Actions | Service | `UploadOffline.action` & `DownloadOffline.action`.

4. Tap on **Sync Changes** in the user menu On Main page to push the local changes to the backend, a successful message will be shown once data is submitted. 

    ![MDK](img-7.8.png)

[OPTION END]

[OPTION BEGIN [Web]]

1. Either click the highlighted button or refresh the web page to load the changes.

    <!-- border -->![MDK](img-7.9.png)

    >If you see the error `404 Not Found: Requested route ('xxxxx-dev-nsdemosampleapp-approuter.cfapps.xxxx.hana.ondemand.com') does not exist.` while accessing the web application, make sure that in your space cockpit, highlight applications are in started state.

    ><!-- border -->![MDK](img-7.10.png)   

2. Click **Customers**, click **+ Add** icon to create a new record.

    <!-- border -->![MDK](img-7.11.png)

3. Enter the values, and click **Save**.

    <!-- border -->![MDK](img-7.12.png)

    Record gets created accordingly.

    <!-- border -->![MDK](img-7.13.png)

[OPTION END]

You can cross verify if a record has been updated in the backend.

>Backend URL can be found in [Mobile Services Cockpit](cp-mobile-dev-kit-ms-setup).

>**Mobile Applications** **&rarr;** **Native/MDK** **&rarr;** click the MDK App **com.sap.mdk.demo** **&rarr;** **Mobile Connectivity** **&rarr;** click **Launch in Browser** icon

><!-- border -->![MDK](img-7.14.png)

>It will open the URL in a new tab, remove `?auth=uaa` and add `/Customers` at the end of the URL.
![MDK](img-7.15.png)

---
