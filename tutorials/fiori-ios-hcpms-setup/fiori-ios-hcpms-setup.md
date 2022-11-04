---
parser: v2
auto_validation: true
primary_tag: software-product>sap-mobile-services
tags: [  tutorial>beginner, topic>mobile, operating-system>ios, software-product>sap-business-technology-platform, software-product>sap-mobile-services, software-product>sap-btp-sdk-for-ios, software-product>sap-btp-sdk-for-android, software-product>sap-mobile-cards, software-product>mobile-development-kit-client]
time: 5
author_name: Jitendra Kansal
author_profile: https://github.com/jitendrakansal
---
# Access SAP Mobile Services
<!-- description --> Access SAP Mobile Services within a SAP Business Technology Platform account and open the Mobile Services cockpit.

## Prerequisites  
- You have [Set Up a BTP Account for Tutorials](group.btp-setup). Follow the instructions to get an account, and then to set up entitlements and service instances for the following BTP services.
    - **SAP Mobile Services**

## You will learn
  - How to access SAP Mobile Services in your BTP account

## Intro
Once SAP Mobile Services is available, you can use its features in your Mobile development kit, Mobile Cards, SAP BTP SDK for iOS & Android apps.

---

### Go to your global account on SAP BTP


> Make sure you are choosing the right BTP account type ( **Free Tier** or **Free Trial** ) in the tabs under Step 1.


[OPTION BEGIN [Free Tier]]

1. In your web browser, open the [SAP BTP cockpit](https://account.hana.ondemand.com/cockpit).

2. Provide the login details and click **Log On**.

    <!-- border -->![SAP BTP Log On Screen](img-1.1.png)

3. Enter your Global Account.

    <!-- border -->![SAP BTP Global Account](img-1.2.png)

[OPTION END]

[OPTION BEGIN [Free Trial]]

1. In your web browser, open the [SAP BTP Trial cockpit](https://account.hanatrial.ondemand.com/cockpit).

2. Provide the login details and click **Log On**.

    <!-- border -->![SAP BTP Log On Screen](img-1.1.png)

3. Navigate to the trial global account by clicking **Go To Your Trial Account**.

    <!-- border -->![Trial global account](img-1.3.png)

[OPTION END]


### Launch SAP Mobile Services cockpit


1. Click subaccount available in your global account.

    <!-- border -->![enter subaccount](img-2.1.png)

2. In the left pane, choose **Services** **&rarr;** **Service Marketplace**.

    >The **Service Marketplace** is where you can find services to attach to any of your applications. These services are provided by SAP BTP to create, and produce applications quickly and easily. Once a service has been created, it is known as a `service instance`.

    <!-- border -->![service marketplace](img-2.2.png)

3. Search for **Mobile**, and click **Mobile Services** tile.  

    <!-- border -->![mobile service tile](img-2.3.png)

4. Choose **Support** to open **SAP Mobile Services Cockpit**.

    <!-- border -->![support button click](img-2.4.png)

5. If you are asked to sign in then enter your Email or Username to continue and click **Next**.

6. Choose the relevant **Organization** and **Space** from the dropdown list, and then select **Open**.

    >**Organization:** Organizations in CF enable collaboration among users and enable grouping of resources.

    >**Space:** Cloud Foundry has a standard working environment for individual applications: it is called a space. Spaces are individual working areas, which normally contain a single application.

    <!-- border -->![BTP](img-2.5.png)

    You have now logged in to the SAP Mobile Services cockpit.

    <!-- border -->![BTP](img-2.6.png)

    Bookmark the **Mobile Services cockpit URL** for quick access.


