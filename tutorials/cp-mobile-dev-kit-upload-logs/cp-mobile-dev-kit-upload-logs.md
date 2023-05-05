---
parser: v2
auto_validation: true
primary_tag: software-product>mobile-development-kit-client
tags: [ tutorial>intermediate, operating-system>ios, operating-system>android, topic>mobile, software-product>sap-business-technology-platform, software-product>mobile-development-kit-client, software-product>sap-mobile-services, software-product>sap-business-application-studio ]
time: 20
author_name: Jitendra Kansal
author_profile: https://github.com/jitendrakansal
---

# Upload Logs from an MDK App
<!-- description --> Allow users to upload logs from an MDK app to SAP Mobile Services.

## You will learn
  - How to enable log upload feature in Mobile Services
  - How to upload logs from the app
  - How to examine log details for troubleshooting


## Intro
You may clone an existing project from [GitHub repository](https://github.com/SAP-samples/cloud-mdk-tutorial-samples/tree/main/3-Enhance-Your-First-MDK-App-with-Additional-Functionalities/2-cp-mobile-dev-kit-delete-customer) to start with this tutorial.

---

![MDK](img-1.0.gif)

### Define client log policy in Mobile Services cockpit


SAP Mobile Services provides administrators, developers and support staff with extensive logs and traces functionality to troubleshoot application issues. You can control the amount of information that is captured by setting the log level for individual logging components.

In this step, you will enable client log upload policy in **SAP Mobile Services Cockpit** for a given MDK app.

1. Login to [Mobile Services Cockpit](fiori-ios-hcpms-setup), click `com.sap.mdk.demo` | **Mobile Client Log Upload**.

    <!-- border -->![MDK](img-1.1.png)

2. Check **Log Upload** option and click **Save**.

    <!-- border -->![MDK](img-1.2.png)

    >Other policy parameters like Log Level, Delete Uploaded Log After, Maximum Number of Logs and Maximum Log Size currently have no effect.


    >You can find more details about [Client Log Upload feature](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/features/logging/admin/policies.html).



### Create log actions


Logs help you trace events that occur while your application is running. You can create logging actions, set their priority levels and upload stored log entries.

You will create 3 actions:

* Log action of Type `SetState`: to turn the logger On, Off or Toggle

* Log action of Type `SetLevel`: to set log level (Debug, Info, Warn or Error)

* Log action of Type `Upload`: to upload logs from app to Mobile Services

>You can find more details about [MDK Log Actions](https://help.sap.com/doc/69c2ce3e50454264acf9cafe6c6e442c/Latest/en-US/docs-en/reference/schemadoc/Action/Logger/LogMessage.schema.html).

<!-- border -->![MDK](img-2.1.png)

1. Create a log action of type `SetState`:

    Right-click the **Actions** folder | **MDK: New Action** | choose **Log** in **Category** | click **Log** | **Next**.

    <!-- border -->![MDK](img-2.2.png)

    In the **Base Information** step, provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `LogSetState` |
    | `Type` | Select `SetState` from the dropdown |
    | `LoggerState`| Select `On` from the dropdown |

    <!-- border -->![MDK](img-2.3.png)

    Click **Finish** to create the action creation process.

2. Create a log action of type `SetLevel`:

    Right-click the **Actions** folder | **MDK: New Action** | choose **Log** in **Category** | click **Log** | **Next**.

    In the **Base Information** step, provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `LogSetLevel` |
    | `Type` | Select `SetLevel` from the dropdown |
    | `Level`| Select `Trace` from the dropdown |

    <!-- border -->![MDK](img-2.4.png)

    Click **Finish** to create the action creation process.

3. Create a log action of type `Upload`:

    Right-click the **Actions** folder | **MDK: New Action** | choose **Log** in **Category** | click **Log** | **Next**.

    In the **Base Information** step, provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `LogUpload` |
    | `Type` | Select `Upload` from the dropdown |

    <!-- border -->![MDK](img-2.5.png)

    Click **Finish** to create the action creation process.


### Bind Set Level log action to the success of Set State action


In the Project Explorer, click the `LogSetState.action` file | expand **Common Action Properties** | click the link icon for **Success Action** and bind it to  `LogSetLevel.action`

<!-- border -->![MDK](img-3.1.png)


### Bind Set State log action to success of Initialize Offline action


When the metadata is downloaded from App Update, `OnDidUpdate` event is being called which eventually triggers `InitializeOffline.action`. You can look at `Application.app` file for the details.

You will bind `LogSetState.action` at success of  `InitializeOffline.action`.

Navigate to `DemoSampleApp` | `Actions` | `Service` | `InitializeOffline.action` | scroll-down and expand **Common Action Properties** section | click the link icon for **Success Action** and bind it to  `LogSetState.action`

<!-- border -->![MDK](img-4.1.gif)


### Define success/failure messages for Log Upload action


You will define two message actions for displaying success or failure when _Log upload action_ is triggered.

1. Expand the *Common Action Properties* section in the `LogUpload.action` and select the `Create a rule/action` for its Success Action.

    <!-- border -->![MDK](img-5.1.png)

2. Keep the default selection for the *Object Type* as Action and *Folders* path.

    <!-- border -->![MDK](img-5.2.png)   

3. In the **Template Selection** step, choose **Message** in **Category** | click **Message** | **Next**.

4. In the **Base Information** step, provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `LogUploadSuccessful` |
    | `Type` | Select `ToastMessage` from the dropdown |
    | `Message`| `Log File Uploaded` |
    | `NumberOfLines`| 1 |
    | `Duration` | 3 |
    | `IsIconHidden`| Select `true` from the dropdown |
    | `Animated` | Select `true` from the dropdown |

    <!-- border -->![MDK](img-5.3.png)

5. Click **Finish** to create the action creation process.

6. Switch back to the `LogUpload.action` tab and create a message action displaying error in case of the Log Upload failure. In the `LogUpload.action`, click the `Create a rule/action` icon for the *Failure Action*.

    <!-- border -->![MDK](img-5.4.png)

7. Keep the default selection for the *Object Type* as Action and *Folders* path.

    <!-- border -->![MDK](img-5.2.png)      

8. In the **Template Selection** step, choose **Message** in **Category** | click **Message** | **Next**.

9. In the **Base Information** step, provide the below information:

    | Property | Value |
    |----|----|
    | `Name`| `LogUploadFailure` |
    | `Type` | Select `Message` from the dropdown |
    | `Message`| `Failed to upload client logs - {#ActionResults:LogUpload/error}` |
    | `Title`| `Upload Client Logs` |
    | `OKCaption` | `OK` |
    | `OnOK` | `--None--` |
    | `CancelCaption` | leave it blank |
    | `OnCancel` | `--None--` |

    <!-- border -->![MDK](img-5.5.png)

10. Click **Finish** to create the action creation process.

    >`LogUpload` is the Action Result value of the `LogUpload.action`. This reference is used to pass the results to subsequent actions in the chain. These actions can reference the action result as needed. In this case if there is a failure, you access the error property of the action result to display the failure message.

11. Let's show an Activity Indicator in the MDK client when logs are being uploaded to the SAP Mobile Services. Switch back to the `LogUpload.action` tab and provide below information.

    | Property | Value |
    |----|----|
    | `Show Activity Indicator` | Select `true` from the dropdown ||
    | `Activity Indicator Text`|  `Uploading Logs...` |

    <!-- border -->![MDK](img-5.6.png)


### Add Upload Log button to main page

You will add a toolbar item to the _main page_ called **Upload Logs**. You will link toolbar item to `LogUpload.action` you just created in step 2.

1. In the `Main.page`, drag and drop an **Toolbar Item** to the bottom right of the page.

    <!-- border -->![MDK](img-6.1.png)

2. Provide the below information:

    | Property | Value |
    |----|----|
    | `Caption`| `Upload Logs`  |
    | `Visible`| `$(PLT,true,true,false)`  |

    <!-- border -->![MDK](img-6.2.png)

    >Platform (PLT) function `$(PLT, <iOS>, <Android>, <Web>)` lets you to define a platform specific value for metadata property as Upload Logs functionality is not applicable in MDK Web environment, this option will not be visible in Web application. You can find more details on PLT function in [help documentation](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/getting-started/mdk/development/property-binding/platform-specific-binding.html#plt-formatter-for-events-supported-for-both-mobile-and-web).

3. In the Properties pane, click the **Events** tab, click the 3 dots icon for the `OnPress` property to open the **Object Browser**.

    Double click the `LogUpload.action` action and click **OK** to set it as the `OnPress` Action.

    <!-- border -->![MDK](img-6.3.png)



### Deploy the application


Deploy the updated application to your MDK client.

1. Right-click `Application.app` and select **MDK: Deploy**.

    <!-- border -->![MDK](img-7.1.png)

2. Select deploy target as **Mobile Services**.

    <!-- border -->![MDK](img-7.2.png)

    You should see **Deploy to Mobile Services successfully!** message.

    <!-- border -->![MDK](img-7.3.png)

    >Alternatively, you can select *MDK: Redeploy* in the command palette (View menu>Find Command OR press Command+Shift+p on Mac OR press Ctrl+Shift+P on Windows machine), it will perform the last deployment.

    ><!-- border -->![MDK](img-7.4.png)


### Run the app


[OPTION BEGIN [Android]]

1. Tap **Update** on the Main page, you will see a _New Version Available_ pop-up, tap **Now**.

    ![MDK](img-8.1.png)

    Once the re-initialization is completed, app starts gathering the client logs.

2. Tap **Upload Logs** to upload client logs from app to SAP Mobile Services.

    ![MDK](img-8.2.png)

    ![MDK](img-8.3.png)

[OPTION END]

[OPTION BEGIN [iOS]]

1. Tap **Update** on the Main page, you will see a _New Version Available_ pop-up, tap **Now**.

    ![MDK](img-8.4.png)

    Once the re-initialization is completed, app starts gathering the client logs.

2. Tap **Upload Logs** to upload client logs from app to SAP Mobile Services.

    ![MDK](img-8.5.png)

    ![MDK](img-8.6.png)

[OPTION END]


### Examine uploaded logs in Mobile Services cockpit


Open SAP Mobile Services Cockpit, click **Mobile Applications** | **Native/MDK** | click `com.sap.mdk.demo` app | **Mobile Client Log Upload** | **Log Files**.

<!-- border -->![MDK](img-9.1.png)

A log file is created for each upload. All the uploaded files, including all levels, not just error or fatal will be listed here.

>Check [documentation](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/features/logging/admin/config.html) for more details about Uploading and Viewing Client Logs.



---
