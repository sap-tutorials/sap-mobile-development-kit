---
parser: v2
auto_validation: true
time: 5
tags: [ tutorial>beginner, topic>mobile, operating-system>ios, operating-system>android, products>sap-business-technology-platform, products>sap-btp--cloud-foundry-environment, products>sap-mobile-cards, products>sap-mobile-services, products>sap-business-application-studio, products>mobile-development-kit-client ]
primary_tag: products>sap-business-technology-platform
author_name: Jitendra Kansal
author_profile: https://github.com/jitendrakansal
---

# Set Up SAP Business Application Studio for Multi-Channel Development
<!-- description --> Set up your SAP Business Application Studio to start developing mobile and web applications.

## Prerequisites
 - [Set Up SAP Business Application Studio for Development](appstudio-onboarding)

## You will learn
  - How to create a development space in SAP Business Application Studio

## Intro
The SAP Business Application Studio (BAS) is an IDE hosted on SAP Business Technology Platform (BTP). There are different Dev Space setup flavours available depending on your development project requirements. In this tutorial, you will set up your SAP Business Application Studio for developing mobile apps.

---

### Open SAP Business Application Studio and create your Dev Space

Before you can start using SAP Business Application Studio, you need to create your developer space, where your project will run. Depending on the application you want to develop, you can create different types of dev spaces.

For this tutorial, you will create a dev space personalized for Mobile development.

1. Launch SAP Business Application Studio in any one of the [supported browsers](https://help.sap.com/docs/SAP%20Business%20Application%20Studio/9d1db9835307451daa8c930fbd9ab264/8f46c6e6f86641cc900871c903761fd4.html#availability) and click **Create Dev Space**.

2. Enter `Tutorial` as the name for your dev space and choose **SAP Mobile Application** as the application type. Continue with **Create Dev Space**.

    ![BAS New Space](img-1.1.png)

    By selecting **SAP Mobile Application**, your space comes with several extensions out of the box that you will need to develop Mobile applications. The creation of the dev space takes a few seconds.

3. When it's ready, open your dev space by clicking on the name.

    ![BAS Enter Space](img-1.2.png)

    >Please note that you're using the trial version of SAP Business Application Studio. See section [Restrictions](https://help.sap.com/products/SAP%20Business%20Application%20Studio/9d1db9835307451daa8c930fbd9ab264/a45742a719704bdea179b4c4f9afa07f.html) in the SAP Business Application Studio documentation for more details on how your development environment can be affected.


  ### Change the workspace to the projects folder

  You will now switch your workspace to the `projects` folder.

1. Click on the Explorer icon and click  **Open Folder**.

    ![BAS New Space](img-2.1.png)
    
2. Select the `projects` folder if not already selected and click **OK**.

    ![BAS New Space](img-2.2.png)

   The projects folder is now open as the workspace.

   ![BAS New Space](img-2.3.png)

---

Congratulations, you have successfully configured SAP Business Application Studio to build multi-channel applications.

You can now build [**Mobile Development Kit apps**](mission.mobile-dev-kit-get-started) using Business Application studio.

---
