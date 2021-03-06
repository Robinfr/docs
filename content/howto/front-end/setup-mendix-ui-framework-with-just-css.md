---
title: "Set Up the Mendix UI Framework with Just CSS"
parent: "setup-mendix-ui-framework"
tags: ["UI Framework", "Styling", "CSS"]
---

## 1 Introduction

In this how-to we will go through how to setup the [Mendix UI Framework](https://ux.mendix.com/) and do our first styling changes by just using CSS.

{{% alert type="warning" %}}
These instructions are for apps which are based on the [Mendix UI Framework](https://ux.mendix.com/). They do not apply to apps which use the [Mendix Atlas UI](https://atlas.mendix.com/).
{{% /alert %}}

**After completing this how-to you will know:**

*   How to create a new App
*   How to create your own theme with just CSS
*   How to make your first styling changes

## 2 Preparation

Before you can start with this how-to, make sure you have completed the following prerequisites.

* Download the latest version of the [Mendix Studio Pro](https://appstore.mendix.com)
* Download a text editor [Sublime Text](http://www.sublimetext.com/)

## 3 Creating a New App in Mendix Studio Pro

In this chapter we will create a new app and select a theme from the New App selector.

1.  Open **Mendix Studio Pro**.
2. Click the App Store icon in the top-right menu bar, then search for "materialism":

	![](attachments/18448709/materialism.png)

3. Click **Download** to start using this theme.
4. Select **Run Locally** to deploy your app.


## 4 Opening Your Project in the Text Editor

1.  Open your **App Project Folder** in your text editor by choice.
2.  Navigate to the folder **theme** and you will find the css files in the folder **styles\css**.

	![](attachments/18448709/18581430.png) 
	
3.  There are two subfolders in our **styles\css** folder.<br>
    a. **Lib**, this folder contains our library file that contains all the default styling from the Mendix UI Framework. It's best practise to not alter this file.<br>
    b. **Custom**, the custom file contains a preset of CSS elements and a copy of the library file but empty. Here we want to make our custom changes
4.  Let's open up the **custom.css** file. We always want to work from this file so we can always look back at the library.css file to see how the original theme used to be.  
5.  Let's change the color of our topbar. Find the CSS element .**region-topbar **as you can see in the screenshot below.

	![](attachments/18448709/18581428.png) 
	
6.  Let's do a simple change by making the topbar red. Insert the following line as in the example below:

    background-color: #FF0000; 

	![](attachments/18448709/18581427.png) 
    
7.  Let's redeploy our application and look at the result!

	![](attachments/18448709/18581426.png)
	
8.  Congratulations, you made your first theme change.

## 5 Read More

*   [Create Your First Two Overview & Detail Pages](create-your-first-two-overview-and-detail-pages)
*   [Find the Root Cause of Runtime Errors](../monitoring-troubleshooting/finding-the-root-cause-of-runtime-errors)
*   [Use Layouts and Snippets](layouts-and-snippets)
*   [Set Up the Navigation Structure](../general/setting-up-the-navigation-structure)
*   [Set Up the Mendix UI Framework with Koala](setup-mendix-ui-framework-with-koala)
*   [Set Up the Mendix UI Framework with Scout](setup-mendix-ui-framework-with-scout)
