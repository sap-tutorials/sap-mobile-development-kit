---
parser: v2
auto_validation: true
primary_tag: software-product>mobile-development-kit-client
tags: [ tutorial>intermediate, operating-system>ios, operating-system>android, topic>mobile, software-product>sap-business-technology-platform, software-product>mobile-development-kit-client, software-product>sap-mobile-services ]
time: 25
author_name: Jitendra Kansal
author_profile: https://github.com/jitendrakansal
---

# Build Your Mobile Development Kit Client Using MDK SDK
<!-- description --> Set up your development environment that enable MDK SDK so that you can begin building your branded Mobile Development Kit client.

## Prerequisites
- **Tutorial**: [Set Up Initial Configuration for an MDK App](cp-mobile-dev-kit-ms-setup)
- **Download the latest version of Mobile Development Kit SDK** either from the SAP community [trial download](https://developers.sap.com/trials-downloads.html?search=Mobile+Development+Kit) or [SAP Software Center](https://me.sap.com/softwarecenter) if you are a SAP Mobile Services customer

## You will learn
  - How to install a Mobile development kit client SDK on Mac and Windows OS
  - How to build a branded Mobile development kit client for iOS and Android
  - How to connect to SAP Mobile app

  There are 3 options for mobile development kit client:

  1. Install the SAP Mobile Services client from the public store which is meant for demo and development or learning purposes
  2. [Use the Cloud Build feature in SAP Mobile Services to generate a MDK Client](cp-mobile-dev-kit-cbs-client) 
  3. Build a client on your local machine in your organization development environment using MDK SDK

## Intro
In this tutorial, you will learn creating a MDK client using option 3.  

For distribution to your users, you need to build a custom client. This enables you to:

-	Provide customer app icon
-	Use your signing profiles
-	Distribute custom extensions
-	Provide app-specific settings (Custom EULA texts, Add app assets)
-	Add demo mode
-	Customize languages (for onboarding screens)
-	Use your own distribution channels, like the mobile device management of your choice

---

### Run MDK Dependencies Installer

>Make sure you are choosing the right development platform tab above.

Make sure that you have download latest version of MDK SDK as described in Prerequisites.

[OPTION BEGIN [Mac]]

1. Extract the downloaded zip file on your Mac. You will see following Files and Folders in the extracted folder.

    <!-- border -->![MDK](img-1.1.png)

2. If you are running an Intel Mac, use the `MDK Dependencies Installer-x86`. If you are running a Mac with an M1,M2, or M3 chip, use the `MDK Dependencies Installer-arm64`. Both installers are available at the root of the extracted MDK SDK folder to start the installation.

    <!-- border -->![MDK](img-1.2.png)

    >If you find some issues (for example: app cant be opened because the identity of the developer cannot be confirmed) while opening this file, go to System Preferences > Security & Privacy and click **Open Anyway**.

3. Enter Admin user password and click **OK**.

    <!-- border -->![MDK](img-1.3.png)

    The installer will list all required components for iOS and Android platform and automatically check if they are already installed in the machine. Follow the installer UI to install the components you selected.

    <!-- border -->![MDK](img-1.4.gif)

    >You might see different software versions depending on MDK SDK version you are using.

    >You can look into console by clicking **Show Log** for execution of each dependencies.

[OPTION END]

[OPTION BEGIN [Windows]]

1. Extract the downloaded zip file on your Windows machine. You will see following Files and Folders in the extracted folder.

    <!-- border -->![MDK](img-1.5.png)

2. At the root of the extracted MDK SDK folder, extract `MDKDependenciesInstallerWindows.zip` file, by default a new folder `MDK Dependencies Installer-win32-ia32` will be created. Enter the new folder and double click `MDK_Dependencies_Installer.exe` to start installer.

    <!-- border -->![MDK](img-1.6.png)

    The installer will list all required components for Windows platform and automatically check if they are already installed in the machine. Follow the installer UI to install the components you selected.

    <!-- border -->![MDK](img-1.7.png)

    >If you encounter any issue while running the MDK Dependencies Installer then have a look at [this](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/troubleshooting/mdk/troubleshoot.html#loading-message-displays-while-running-mobile-development-kit-dependency-installer-on-windows-machine) troubleshooting guide.

    >You might see different software versions depending on MDK SDK version you are using.

    >You can look into console by clicking **Show Log** for execution of each dependencies.

[OPTION END]

Once you've installed these prerequisites, your machine is ready to generate and build an MDK project.



### Installing the SDK dependencies

>Make sure you are choosing the right development platform tab above.

[OPTION BEGIN [Mac]]

To use the SDK to generate a mobile development kit client, the first step is to install some dependencies.

1. Unzip `MDKClient_SDK.zip` if it is not already extracted.

    <!-- border -->![MDK](img-2.1.png)

2. Invoke `install.command` either from Terminal or from Finder. From a terminal window, navigate to the `[path] -> MDKClient_SDK` folder and execute `./install.command`. A terminal window will appear installs the SDK dependencies using NPM.

    <!-- border -->![MDK](img-2.2.png)

    Once this completes, you will a success message in the console followed by next steps.

    <!-- border -->![MDK](img-2.3.png)

    >If you select the command from Finder, you may see the following message:
    "install.command" can't be opened because it is from an unidentified developer. Your security preferences allow installation of only apps from the App Store and identified developers. To get around this, navigate to the `Security & Privacy` section of `System Preferences`. This can be found using Spotlight. Near the bottom of this dialog, click the *Open Anyway* button on the right side. Then select *Open* from the next prompt.

    Notice that the `create-client.command` file has appeared in the SDK directory.

    <!-- border -->![MDK](img-2.4.png)

[OPTION END]

[OPTION BEGIN [Windows]]

To use the SDK to generate a mobile development kit client, the first step is to install some dependencies.

1. Unzip `MDKClient_SDK.zip` if it is not already extracted.

    <!-- border -->![MDK](img-2.5.png)

2. Invoke `install.cmd`  from a command line window, navigate to the `[path] -> MDKClient_SDK` folder and execute `install.cmd`. A terminal window will appear installs the SDK dependencies using NPM.

    <!-- border -->![MDK](img-2.6.png)

    <!-- border -->![MDK](img-2.7.png)

    Once this completes, you will a success message in the console followed by next steps.

    <!-- border -->![MDK](img-2.8.png)    

    Notice that the `create-client.cmd` file has appeared in the SDK directory.

    <!-- border -->![MDK](img-2.9.png)    

[OPTION END]


### Create your .mdkproject folder


>Make sure you are choosing the right development platform tab above.

[OPTION BEGIN [Mac]]

1. In the `MDKClient_SDK` folder, you will find the `template.mdkproject` folder.

    <!-- border -->![MDK](img-3.1.png)

    It is recommended that you copy this folder to another location so that you can to use it for future builds. Copy and paste it anywhere, and then rename template to `DemoSampleApp.mdkproject`.

    <!-- border -->![MDK](img-3.2.png)

2. Next, you will need to update the `BrandedSettings.json` and `MDKProject.json` files as needed for your client. Go into the `DemoSampleApp.mdkproject` folder.

    <!-- border -->![MDK](img-3.3.png)

3. Open the `MDKProject.json` file and update it as needed. This file has some build-time configurations such as the application name, version and bundle ID.

    <!-- border -->![MDK](img-3.4.png)

    >`AppDisplayName`: This is the name of the application on the home screen of the device.

    >`AppName`: This is the name of the folder where the client is created.

    >`AndroidVersionCode`: The Android application version code. It accepts value 'Auto' or any positive integer value. If it is not defined, the default version code will be 1. For more details, [Android Application Version Code](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/getting-started/mdk/custom-client/branding-custom-client.html#android-application-version-code).

    >`BaseProject`: The sub-directory of metadata where the project is found. For this tutorial, you don't have any subfolder under metadata, you can either keep it as it is or remove it completely.

    >`BundleID`: It should be a unique identifier for your application. This controls if the client can be installed side by side with other applications on the device. Two applications with the same Bundle ID cannot be installed at the same time on a device. For iOS this is the Identifier `(AppID)` that is registered in Apple Developer account since that determines if the application can be installed alongside other applications. If the `XCode` project is set up to use _Automatically manage signing_ then when building, `XCode` will automatically generate a signing profile for the specified bundle id. Without matching them, trying to run the custom client in iOS device will result in failure. In Android, it is known as [application ID](https://developer.android.com/build/configure-app-module).

    >`UrlScheme`: Allows you to specify a custom URL scheme which opens the client. The default is `mdkclient`. This value also needs to be unique across applications on your device. If the value is not unique the wrong application may be referenced when redirecting.

    >You can find more details about configuration of `MDKProject.json` file in [this](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/getting-started/mdk/custom-client/branding-custom-client.html#configuration-of-mdkprojectjson-file) help documentation.        

4. Now, open the `BrandedSettings.json` and provide `ApplicationDisplayName` name. This name will appear on the Welcome screen in your branded MDK client.

    <!-- border -->![MDK](img-3.5.png)

5. To update the `AppId`, `ClientId`, `ServerUrl`, `AuthorizationEndPointUrl`, `RedirectUrl` and `TokenUrl` in the `ConnectionSettings` block, navigate to the [Mobile Services cockpit](cp-mobile-dev-kit-ms-setup), click `com.sap.mdk.demo` > **Mobile Settings Exchange** > **Info** tab, copy the highlighted block and paste it in `BrandedSettings.json`.

    <!-- border -->![MDK](img-3.6.png)

    <!-- border -->![MDK](img-3.7.png)

    Regarding other properties:

    **Debug settings**: The settings in the `DebugSettings` property are for development use and should not be enabled in a production setting.

    **Log Settings**: Set this to the log level to be used when the client is launched.

    **Demo**: If you want to access the app in the demo mode, you can configure required settings.        

    >If you are connecting to `AliCloud` accounts, you will also need to add your custom domains under `URLWhitelist` property in the same file. You can find more details in [documentation](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/getting-started/mdk/custom-client/branding-custom-client.html#connection-settings-allowlist).

5. In the last section of `BrandedSettings.json` file, make these changes and save the file:

    | Field | Value |
    |----|----|
    | `DetailLabelViewText` | `My first branded client using MDK SDK` |
    | `SigninButtonText` | `Start` |

    <!-- border -->![MDK](img-3.8.png)        

[OPTION END]

[OPTION BEGIN [Windows]]

1. In the `MDKClient_SDK` folder, you will find the `template.mdkproject` folder.

    <!-- border -->![MDK](img-3.9.png)

    It is recommended that you copy this folder to another location so that you can to use it for future builds. Copy and paste it anywhere, and then rename the template to `DemoSampleApp.mdkproject`.

    <!-- border -->![MDK](img-3.10.png)

2. Next, you will need to update the `MDKProject.json` and `BrandedSettings.json` files as needed for your client. Go into the `DemoSampleApp.mdkproject` folder.

    <!-- border -->![MDK](img-3.11.png)

3. Open the `MDKProject.json` file and update it as needed. This file has some build-time configurations such as the application name, version, bundle ID and few others.

    <!-- border -->![MDK](img-3.4.png)

    >`AppDisplayName`: This is the name of the application on the home screen of the device.

    >`AppName`: This is the name of the folder where the client is created.

    >`BundleID`: It should be a unique identifier for your application. This controls if the client can be installed side by side with other applications on the device. Two applications with the same Bundle ID cannot be installed at the same time on a device.

    >`URLScheme`: Allows you to specify a custom URL scheme which opens the client. The default is `mdkclient`. This value also needs to be unique across applications on your device.  If the value is not unique the wrong application may be referenced when redirecting.    

    >You can find more details about configuration of `MDKProject.json` file in [this](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/getting-started/mdk/custom-client/branding-custom-client.html#configuration-of-mdkprojectjson-file) help documentation.                   

4. Now, open the `BrandedSettings.json` and provide `ApplicationDisplayName` name. This name will appear on the Welcome screen in your branded MDK client.

    <!-- border -->![MDK](img-3.5.png)

5. To update the `AppId`, `ClientId`, `ServerUrl`, `AuthorizationEndPointUrl`, `RedirectUrl` and `TokenUrl` in the `ConnectionSettings` block, navigate to the [Mobile Services cockpit](cp-mobile-dev-kit-ms-setup), click `com.sap.mdk.demo` > **Mobile Settings Exchange** > **Info** tab, copy the highlighted block and paste it in `BrandedSettings.json`.

    <!-- border -->![MDK](img-3.6.png)

    <!-- border -->![MDK](img-3.7.png)

    Regarding other properties:

    **Debug settings**: The settings in the `DebugSettings` property are for development use and should not be enabled in a production setting.

    **Log Settings**: Set this to the log level to be used when the client is launched.

    **Demo**: If you want to access the app in the demo mode, you can configure required settings.        

    >If you are connecting to `AliCloud` accounts, you will also need to add your custom domains under `URLWhitelist` property in the same file. You can find more details in [documentation](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/getting-started/mdk/custom-client/branding-custom-client.html#connection-settings-allowlist).

5. In the last section of `BrandedSettings.json` file, make these changes and save the file:

    | Field | Value |
    |----|----|
    | `DetailLabelViewText` | `My first branded client using MDK SDK` |
    | `SigninButtonText` | `Start` |

    <!-- border -->![MDK](img-3.8.png)        

[OPTION END]


### Create the MDK Client


>Make sure you are choosing the right development platform tab above.

[OPTION BEGIN [Mac]]

1. Run `./create-client.command` to create the client. You will be asked to provide the path to the `.mdkproject` file. You can also provide optional arguments such as output directory and log verbosity. You can run `create-client.command --help` to find out how to specify these options as arguments.

    <!-- border -->![MDK](img-4.1.png)

    >You can run the `create-client command` from any directory.  The resulting MDK client will be created in the directory where the `create-client command` is run from.

2. You will be asked whether you would like to build for iOS or Android or All?

    <!-- border -->![MDK](img-4.2.png)

    >**All** option was chosen in this tutorial as you will learn how to create the MDK client for iOS and Android.

    Once the `create-client.command` script executed successfully, you will see **Application ready** message in terminal console.

    <!-- border -->![MDK](img-4.3.png)

    You will also find your MDK Client app created under the `MDKClient_SDK` folder.

    <!-- border -->![MDK](img-4.4.png)

[OPTION END]

[OPTION BEGIN [Windows]]

1. Run `./create-client.cmd` to create the client. You will be asked to provide the path to the `.mdkproject` file. You can also provide optional arguments such as output directory and log verbosity. You can run `create-client.cmd --help` to find out how to specify these options as arguments.

    <!-- border -->![MDK](img-4.5.png)

    >You can run the `create-client cmd` from any directory. The resulting MDK client will be created in the directory where the `create-client command` is run from.

2. Once the `create-client.cmd` script executed successfully, you will see **Application ready** message in terminal console.

    <!-- border -->![MDK](img-4.6.png)

    <!-- border -->![MDK](img-4.7.png)

     You will also find your app created under the `MDKClient_SDK` folder.

    <!-- border -->![MDK](img-4.8.png)

[OPTION END]

>This name of this folder is based on the `<App Name>` provided in the `MDKProject.json file` and this is the MDK client application generated project.



### Run the MDK Client


>Make sure you are choosing the right device platform tab above.

[OPTION BEGIN [Android]]

1. In this step, you will run your branded MDK client on an Android emulator. Before trying to launch the client on Android emulator, make sure that you have already configured a virtual device (Android Studio>Virtual Device Manager). Otherwise, you may get an error like No emulator image available for device identifier. In terminal or command line window, navigate to the app name folder **`DemoSampleApp`** (in `MDClient_SDK` path) and use `tns run android --emulator` command to run the MDK client on the Android emulator.

    <!-- border -->![MDK](img-5.1.png)

    >To run the MDK client on Android device, first attach your device to your machine. Then run `tns device android` command to print a list of attached devices. Copy the **Device Identifier** value for your device.
    <!-- border -->![MDK](img-5.2.png)
    Make sure **Developer option** and **USB debugging** option is enabled in android device. Then run `tns run android --device <device identifier>` command to launch the MDK client on your Android device.
    <!-- border -->![MDK](img-5.3.png)

    Once, above command gets successfully executed, you will see new MDK client up and running in Android simulator.

4. Tap **Agree** on `End User License Agreement`.

    ![MDK](img-5.4.png)

5. In Welcome screen, you will notice that **app name**, **detailed label text** and **signing button text** have been updated as per changes done in step 3. Tap **Start** to connect MDK client to SAP Business Technology Platform (BTP).

    ![MDK](img-5.5.png)


6. Enter your BTP E-Mail, ID or Login Name to continue. 

    ![MDK](img-5.6.png)

7. Enter your Universal ID password to login to SAP Business Technology Platform (BTP).

    ![MDK](img-5.7.1.png)

8. Choose a passcode with at least 8 characters for unlocking the app and tap **Next**.

    ![MDK](img-5.8.png)

9. Confirm the passcode and tap **Done**.

    ![MDK](img-5.9.png)
    

10. Optionally, you can enable biometric authentication to get faster access to the app data. **Please note that if your MDK client is multi-user enabled, you won't see the biometric screen**.

    ![MDK](img-5.9.2.png)

11. If you want your MDK client to send you notification, click **Next**. 

    ![MDK](img-5.9.1.png)

12. If there is any app metadata already deployed to Mobile Services, you will see `New Version Available!` dialog box otherwise you will see a screen with next steps.

    ![MDK](img-5.10.png)

    >You can always interrupt running process in terminal window by pressing `control + C`.

    >To build an **`APK` for an Android device**, use `tns build android --release`. More information about archiving can be found in `NativeScript` documentation [here](https://v7.docs.nativescript.org/tooling/docs-cli/project/testing/build-android).

[OPTION END]

[OPTION BEGIN [iOS]]

1. In this step, you will run your branded MDK client on an iOS simulator. In terminal window, navigate to the app name folder **`DemoSampleApp`** (in `MDClient_SDK` path) and use `tns run ios --emulator` command to run the MDK client on the iOS simulator.

    <!-- border -->![MDK](img-5.11.png)

    >To run the MDK client on iOS device, first attach the device to your Mac. Then run `tns device ios` command to print a list of attached devices. Copy the **Device Identifier** value for your device.
    <!-- border -->![MDK](img-5.12.png)
    Then run `tns run ios --device <device identifier>` command to launch the MDK client on your iOS device.
    <!-- border -->![MDK](img-5.13.png)

    >You can also run the app in Xcode. Open the project in Xcode with the command `open platforms/ios/<app name>.xcworkspace`, or open the workspace using the `File -> Open...` dialog in Xcode. Configure the application's code signing settings, then run the application for the target device.

    Once, above command gets successfully executed, you will see new MDK client up and running in iOS simulator.

4. Tap **Agree** on `End User License Agreement`.

    ![MDK](img-5.13.1.png)

5. In Welcome screen, you will notice that **app name**, **detailed label text** and **signing button text** have been updated as per changes done in step 3. Tap **Start** to connect MDK client to SAP Business Technology Platform (BTP).

    ![MDK](img-5.14.1.png)

6. Enter your BTP E-Mail, ID or Login Name to continue. 

    ![MDK](img-5.15.png)

7. Enter your Universal ID password to login to SAP Business Technology Platform (BTP).

    ![MDK](img-5.16.png)

8. Choose a passcode with at least 8 characters for unlocking the app and tap **Next**.

    ![MDK](img-5.17.png)


9. Confirm the passcode and tap **Done**.

    ![MDK](img-5.18.png)

10. Optionally, you can enable biometric authentication to get faster access to the app data. **Please note that if your MDK client is multi-user enabled, you won't see the biometric screen**.

    ![MDK](img-5.19.png)

11. If there is any app metadata already deployed to Mobile Services, you will see `New Version Available!` dialog box otherwise you will see a screen with next steps.

    ![MDK](img-5.20.png)

    >You can always interrupt running process in terminal window by pressing `control + C`.

    >To build an **IPA for an iOS device**, use `tns build ios --for-device --release`. This can also be accomplished in Xcode by opening the workspace and selecting the Archive option. More information about archiving can be found in Apple's documentation [here](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/UploadingYourApptoiTunesConnect/UploadingYourApptoiTunesConnect.html).

[OPTION END]



---
