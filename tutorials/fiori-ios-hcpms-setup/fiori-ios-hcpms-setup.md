---
parser: v2
auto_validation: true
primary_tag: software-product>sap-mobile-services
tags: [  tutorial>beginner, topic>mobile, operating-system>ios, software-product>sap-business-technology-platform, software-product>sap-mobile-services, software-product>sap-btp-sdk-for-ios, software-product>sap-btp-sdk-for-android, software-product>mobile-development-kit-client]
time: 5
author_name: Jitendra Kansal
author_profile: https://github.com/jitendrakansal
---
# Access SAP Mobile Services
<!-- description --> Access SAP Mobile Services within a SAP Business Technology Platform account and open the Mobile Services cockpit.

## Prerequisites  
- You have [Set Up a BTP Account for Tutorials](https://developers.sap.com/group.btp-setup.html). Follow the instructions to get an account, and then to set up entitlements and service instances for the following BTP services.
    - **SAP Mobile Services**

## You will learn
  - How to access SAP Mobile Services in your BTP account

## Intro
Once SAP Mobile Services is available, you can use its features in your Mobile development kit, SAP BTP SDK for iOS & Android apps.

---

### Go to your global account on SAP BTP


> Make sure you are choosing the right BTP account type ( **Free Tier** or **Free Trial** ) in the tabs under Step 1.


[OPTION BEGIN [Free Tier]]

1. In your web browser, open the [SAP BTP cockpit](https://account.hana.ondemand.com/cockpit).

2. Provide the login details to log on to your account.

3. Enter your Global Account.

    ![SAP BTP Global Account](img-1.1.png)

[OPTION END]

[OPTION BEGIN [Free Trial]]

1. In your web browser, open the [SAP BTP Trial cockpit](https://account.hanatrial.ondemand.com/cockpit).

2. Provide the login details to log on to your account.

3. Navigate to the trial global account by clicking **Go To Your Trial Account**.

    ![Trial global account](img-1.2.png)

[OPTION END]


### Launch SAP Mobile Services cockpit


1. Click subaccount available in your global account.

    ![enter subaccount](img-2.1.png)

2. In the left pane, choose **Services** **&rarr;** **Service Marketplace**.

    >The **Service Marketplace** is where you can find services to attach to any of your applications. These services are provided by SAP BTP to create, and produce applications quickly and easily. Once a service has been created, it is known as a `service instance`.

    ![service marketplace](img-2.2.png)

3. Search for **Mobile**, and click **Mobile Services** tile.  

    ![mobile service tile](img-2.3.png)

4. Choose **Support** to open **SAP Mobile Services Cockpit**.

    ![support button click](img-2.4.png)

5. The SAP mobile service cockpit dialog, **Select API Endpoint**, appears. Before you click on **Login**, make sure to provide the correct API endpoint belongs to your BTP subaccount. 

    ![support button click](img-2.4.1.png)

    >In order to find the API endpoint, navigate to your BTP subaccount > Overview > Cloud Foundry Environment. Copy the URL and paste it in the Mobile service cockpit dialog. This is mainly required if you are using Mobile Services in an Extended Landscape. 
    >![support button click](img-2.4.2.png)

    Once you provide the correct API Endpoint, click on **Login** to continue.

6. You may asked to enter your Email or Username to continue and click **Next**.

7. After you log into the endpoint, the **Organization** and **Space** prompt appears. The current API Endpoint value appears. Choose the relevant **Organization** and **Space** from the dropdown list, and then choose **Open**.

    ![BTP](img-2.5.png)

    >If the organization that you want to access is not listed, you might have used the wrong API Endpoint. In this case, you can log out and log in again, in order to input the correct API Endpoint value.
    You have now logged in to the SAP Mobile Services cockpit.

    ![BTP](img-2.6.png)

    Bookmark the **Mobile Services cockpit URL** for quick access.


