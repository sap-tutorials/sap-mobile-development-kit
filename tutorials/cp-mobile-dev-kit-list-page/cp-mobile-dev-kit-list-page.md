---
parser: v2
auto_validation: true
primary_tag: software-product>mobile-development-kit-client
tags: [ tutorial>beginner, operating-system>ios, operating-system>android, topic>mobile, software-product>sap-business-technology-platform, software-product>mobile-development-kit-client, software-product>sap-mobile-services, software-product>sap-build-code, software-product>sap-business-application-studio ]
time: 10
author_name: Jitendra Kansal
author_profile: https://github.com/jitendrakansal
---

# Create a Customer List Page in an MDK App
<!-- description --> Use the mobile development kit page editor to create a new page for displaying a customer list.

## You will learn
  - How to create a new page and add some controls to display customer information
  - How to navigate from one page to another page

---

## Intro
To enhance your MDK app with customer list information, you need to carry out the following tasks:

*  Create a new customer list page
*  Add Contact Table control to the page
*  Create a new navigation action to the customer list page
*  Add a new button on main page and navigate to customer list page when user clicks it
*  Deploy the app metadata to SAP Mobile Services & Cloud Foundry
*  Update the app with new changes

![MDK](img-1.0.gif)

### Create a new page for displaying customer list

This page is a searchable list that displays all customers.

To create the Customer List page, you will create a **Section page** and drag the Customer **Contact Table** control onto the page. In the property palette, you will link the control to the Customer Collection and then map data and actions to different areas of the cell object in the property palette. One nice feature about the **Contact Table** control is that it has icons that can activate device functionality such as phone, email, video and more.

1. In the project, right-click the **Pages** | **MDK: New Page**.

    <!-- border -->![MDK](img-1.1.png)

2. Select | **Section** and click **Next**.

    <!-- border -->![MDK](img-1.2.png)

    >You can find more details about the Section Page in this [documentation](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/features/fiori-ui/mdk/section-page.html).

3. Enter the page **Name** as `Customers_List`, and click **Finish**. 

    <!-- border -->![MDK](img-1.3.png)

4. Once the page opens in the page editor, click on the white area to select it's Action Bar, and set the **Caption** to **Customers**.

    <!-- border -->![MDK](img-1.4.gif)

5. In the Layout Editor, expand the **Controls** | **Data Bound Container** group, drag and drop the **Contact Table** control onto the Page area.

    <!-- border -->![MDK](img-1.5.gif)

6. In the Target section, provide the below information:

    | Field | Value |
    |----|----|
    | `Service`| Select `com_sap_edm_sampleservice_v4.service` from the dropdown |
    | `EntitySet` | Select `Customers` from the dropdown |

    <!-- border -->![MDK](img-1.6.png)

7. In the **Properties** pane, click the **link icon** to open the Object Browser for the **Description** property. Double click the `City` property of the Customer entity to set it as the binding expression and click **OK**.

    <!-- border -->![MDK](img-1.7.gif)

    >Be careful **not** to select `City` from `Address (ESPM.Address)`.
    
    ><!-- border -->![MDK](img-1.8.png)

8. Remove the default value for the `DetailImage` property. Repeat the above steps for `Headline` and `Subheadline` properties binding to `LastName` and `FirstName` properties of the Customer entity respectively.

    You should have final results as below.

    <!-- border -->![MDK](img-1.9.png)

9. In the **Search** section of the Properties pane, change both the `SearchEnabled` and `BarcodeScanner` properties to **true**.

    <!-- border -->![MDK](img-1.10.png)

10. In the **Data** section of the **Properties** pane, click **Add** to add a new activity item.

    <!-- border -->![MDK](img-1.11.png)

11. Expand the added item, click the dotted icon to open the Object browser to bind the `ActivityValue` to the `PhoneNumber` property of the Customer entity.

    <!-- border -->![MDK](img-1.12.gif)

12. Similarly, add one more activity item, select **Email** from the dropdown and bind it to `EmailAddress` property of the Customer entity.

    <!-- border -->![MDK](img-1.13.png)


### Navigate to the Customer List page

Now, you will add a button on the Main page and from there, you will navigate to the `Customers_List.page`.

1. In the `Main.page`, expand the **Controls** | **Static Container** group, drag and drop the **Button Table** control onto the Page area.

    <!-- border -->![MDK](img-2.1.gif)

    >**Container** includes controls that act as containers for other controls, such as container items. A container is constant across all pages. The size of a container depends on the controls and contents it contains.

2. Expand the **Static Items** section of the Controls palette.

    Drag and drop a **Button** onto the Button Table container on the page.

    <!-- border -->![MDK](img-2.2.gif)

    >Each static container type in a Section Page can contain specific controls (static items).

    >You can find more details about Container and Item it may contain in this [documentation](https://help.sap.com/doc/69c2ce3e50454264acf9cafe6c6e442c/Latest/en-US/docs-en/reference/schemadoc/Page/SectionedTable/SectionedTable.schema.html).

3. In the Properties pane, provide the below information:

    | Field | Value |
    |----|----|
    | `Image` | Click link icon to open the browser and bind it to `customer` SAP icon |
    | `Title` | `Customers` |

    <!-- border -->![MDK](img-2.3.png)
    
    >`Image` property is referencing to the [SAP font icon](https://openui5.hana.ondemand.com/test-resources/sap/m/demokit/iconExplorer/webapp/index.html#/overview/SAP-icons).

4. Under **Events** tab, click the dotted icon for the `OnPress Handler` property and select the `Create a rule/action`.

    <!-- border -->![MDK](img-2.4.png)

5. Keep the default selection for the *Object Type* as Action and *Folders* path.

    <!-- border -->![MDK](img-2.5.png)

6. Choose **UI** in **Category** | click **Navigation** | **Next**.

    <!-- border -->![MDK](img-2.6.png)

7. In the **Base Information** step, provide the below information and click **Finish**.

    | Field | Value |
    |----|----|
    | `Name` | `NavToCustomers_List` |
    | `PageToOpen` | Select `Customers_List.page` from the dropdown |

    <!-- border -->![MDK](img-2.7.png)

### Deploy the Project

You will now deploy the updated project to your MDK client.

Click the **Deploy** option in the editor's header area, and then choose the deployment target as **Mobile Services** 

<!-- border -->![MDK](img-3.png)

### Run the Project

>Make sure you are choosing the right platform tab above.

[OPTION BEGIN [Android]]

1. Tap **Check for Updates** in the user menu on the Main page, you will see a _New Version Available_ pop-up, tap **Now**.

    ![MDK](img-4.1.png)

2. You will notice the newly added section button on the Main page. Tap **Customers**.

    ![MDK](img-4.2.png)

    Here, you will see list of all the Customers. You can search a record by by First Name or Last Name or City. Controls are rendered natively on device, you can email to the customer, make a phone call etc.

    ![MDK](img-4.3.png)

    >Here, you may notice that City is not showing up on screen, this is by design. In portrait mode, the device width is considered compact, if it was a tablet device (where both portrait and landscape are considered regular instead of compact) you would see City on either orientation.

[OPTION END]

[OPTION BEGIN [iOS]]

1. Tap **Check for Updates** in the user menu on the Main page, you will see a _New Version Available_ pop-up, tap **Now**.

    ![MDK](img-4.4.png)

2. You will notice the newly added section button on the Main page. Tap **Customers**.

    ![MDK](img-4.5.png)

    Here, you will see list of all the Customers. You can search a record by by First Name or Last Name or City. Controls are rendered natively on device, you can email to the customer, make a phone call etc.

    ![MDK](img-4.6.png)

    >Here, you may notice that **City** is not showing up on screen, this is by design. In portrait mode, the device width is considered _compact_ , if you change device orientation to landscape mode, you will see **City**.

    >![MDK](img-4.7.png)

    >If it was an iPad (where both portrait and landscape are considered *regular* instead of *compact*) you would see **City** on either orientation.

[OPTION END]

---
