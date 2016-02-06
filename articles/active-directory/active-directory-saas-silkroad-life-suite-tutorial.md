<properties
	pageTitle="Tutorial: Azure Active Directory integration with SilkRoad Life Suite | Microsoft Azure"
	description="Learn how to configure single sign-on between Azure Active Directory and SilkRoad Life Suite."
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/14/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory integration with SilkRoad Life Suite

The objective of this tutorial is to show you how to integrate SilkRoad Life Suite with Azure Active Directory (Azure AD).<br>Integrating SilkRoad Life Suite with Azure AD provides you with the following benefits: 

- You can control in Azure AD who has access to SilkRoad Life Suite 
- You can enable your users to automatically get signed-on to SilkRoad Life Suite (Single Sign-On) with their Azure AD accounts

If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).

## Prerequisites 

To configure Azure AD integration with SilkRoad Life Suite, you need the following items:

- An Azure AD subscription
- A SilkRoad Life Suite single-sign on enabled subscription


> [AZURE.NOTE] To test the steps in this tutorial, we do not recommend using a production environment.


To test the steps in this tutorial, you should follow these recommendations:

- You should not use your production environment, unless this is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/). 

 
## Scenario Description
The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment. <br>
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding SilkRoad Life Suite from the gallery 
2. Configuring and testing Azure AD single sign-on


## Adding SilkRoad Life Suite from the gallery
To configure the integration of SilkRoad Life Suite into Azure AD, you need to add SilkRoad Life Suite from the gallery to your list of managed SaaS apps.

**To add SilkRoad Life Suite from the gallery, perform the following steps:**

1. In the **Azure Management Portal**, on the left navigation pane, click **Active Directory**. <br><br>
![Active Directory][1]<br>

2. From the **Directory** list, select the directory for which you want to enable directory integration.

3. To open the applications view, in the directory view, click **Applications** in the top menu.<br><br>
![Applications][2]<br>
4. Click **Add** at the bottom of the page.<br><br>
![Applications][3]<br>
5. On the **What do you want to do** dialog, click **Add an application from the gallery**.<br><br>
![Applications][4]<br>
6. In the search box, type **SilkRoad Life Suite**.<br><br>
![Applications][5]<br>
7. In the results pane, select **SilkRoad Life Suite**, and then click **Complete** to add the application.
<br><br>![Applications][50]<br>


##  Configuring and testing Azure AD single sign-on
The objective of this section is to show you how to configure and test Azure AD single sign-on with SilkRoad Life Suite based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in SilkRoad Life Suite to an user in Azure AD is. In other words, a link relationship between an Azure AD user and the related user in SilkRoad Life Suite needs to be established.<br>
This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SilkRoad Life Suite.
 
To configure and test Azure AD single sign-on with SilkRoad Life Suite, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
4. **[Creating a SilkRoad Life Suite test user](#creating-a-silkroad-life-suite-test-user)** - to have a counterpart of Britta Simon in SilkRoad Life Suite that is linked to the Azure AD representation of her.
5. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD Single Sign-On

The objective of this section is to enable Azure AD single sign-on in the Azure AD portal and to configure single sign-on in your SilkRoad Life Suite application.<br>

**To configure Azure AD single sign-on with SilkRoad Life Suite, perform the following steps:**

5. Sign-on to your SilkRoad company site as administrator. 


    > [AZURE.NOTE] To obtain access to the SilkRoad Life Suite Authentication application for configuring federation with Microsoft Azure AD, please contact SilkRoad Support or your SilkRoad Services representative.


6. Go to **Service Provider**, and then clcik **Federation Details**. 
<br><br>![Azure AD Single Sign-On][10] <br>


1. Click **Download Federation Metadata**, and then save the metadata file on your computer.
<br><br>![Azure AD Single Sign-On][11] <br>

3. In the Azure AD portal, on the **SilkRoad Life Suite** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.
<br><br> ![Configure Single Sign-On][6] <br>

2. On the **How would you like users to sign on to SilkRoad Life Suite** page, select **Azure AD Single Sign-On**, and then click **Next**.
<br><br> ![Azure AD Single Sign-On][7] <br>

3. On the **Configure App Settings** dialog page, perform the following steps:
<br><br>![Azure AD Single Sign-On][8] <br>
 
    a. In the **Sign On URL** textbox, type the URL used by your users to sign-on to your SilkRoad Life Suite site (e.g.: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).

    b. Open the downloaded **Silkroad** metadata file.

    c. Locate the **AssertionConsumerService** tag, and then copy the **Location** attribute.         
    <br><br>![Azure AD Single Sign-On][21] <br>
   
    d. Paste the value into the **Reply URL** textbox.
 
    e. Click **Next**.
 
4. On the **Configure single sign-on at SilkRoad Life Suite** page, perform the following steps:
<br><br>![Azure AD Single Sign-On][9] <br>

    a. Click Download certificate, and then save the file on your computer.

    b. Click **Next**.




1. In your **SilkRoad** application, click **Authentication Sources**.
<br><br>![Azure AD Single Sign-On][12] <br>



1. Click **Add Authentication Source**. 
<br><br>![Azure AD Single Sign-On][13] <br>



1. In the **Add Authentication Source** section, perform the following steps: 
<br><br>![Azure AD Single Sign-On][14] <br>

    a. Under **Option 2 - Metadata File**, click **Browse** to upload the downloaded metadata file.

    b. Click **Create Identity Provider using File Data**.



1. In the **Authentication Sources** section, click **Edit**. 
<br><br>![Azure AD Single Sign-On][15] <br>


1. On the **Edit Authentication Source** dialog, perform the following steps: 
<br><br>![Azure AD Single Sign-On][16] <br>

    a. As **Enabled**, select **Yes**.

    b. In the **IdP Description** textbox, type a description for your configuration (e.g.: *Azure AD SSO*).

    c. In the **IdP Name** textbox, type a name that is specific to your configuration (e.g.: *Azure SP*).

    d. Click **Save**.


6. Disable all other authentication sources. 
<br><br>![Azure AD Single Sign-On][17]<br>

7. In the Azure AD portal, on the **Single sign-on confirmation** page, click **Next**.  
  <br><br>![Azure AD Single Sign-On][18]

1. On the **Single sign-on confirmation** page, click **Complete**.
  <br><br>![Azure AD Single Sign-On][19]


### Creating an Azure AD test user
The objective of this section is to create a test user in the Azure portal called Britta Simon.<br>
In the Users list, select **Britta Simon**.<br><br>![Create Azure AD User][20]<br>

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure Management Portal**, on the left navigation pane, click **Active Directory**.
<br><br>![Creating an Azure AD test user](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png) <br> 

2. From the **Directory** list, select the directory for which you want to enable directory integration.

3. To display the list of users, in the menu on the top, click **Users**.
<br><br> ![Creating an Azure AD test user](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) <br>
 
4. To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**. 
<br><br> ![Creating an Azure AD test user](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) <br>

5. On the **Tell us about this user** dialog page, perform the following steps: 
<br><br> ![Creating an Azure AD test user](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png) <br> 

    a. As Type Of User, select New user in your organization.

    b. In the User Name **textbox**, type **BrittaSimon**.

    c. Click **Next**.

6.  On the **User Profile** dialog page, perform the following steps: 
<br><br>![Creating an Azure AD test user](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png) <br>
 
    a. In the **First Name** textbox, type **Britta**.  

    b. In the **Last Name** textbox, type, **Simon**.

    c. In the **Display Name** textbox, type **Britta Simon**.

    d. In the **Role** list, select **User**.
    e. Click **Next**.

7. On the **Get temporary password** dialog page, click **create**.
<br><br> ![Creating an Azure AD test user](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) <br>
 
8. On the **Get temporary password** dialog page, perform the following steps:
<br><br>![Creating an Azure AD test user](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png) <br>
  
    a. Write down the value of the **New Password**.

    b. Click **Complete**.   

  
 
### Creating a SilkRoad Life Suite test user

The objective of this section is to create a user called Britta Simon in SilkRoad Life Suite. Britta's must have an SSO ID (sometimes referred to as an *AuthParam*) that matches Britta's **emailaddress** in Azure AD.

**To create a user called Britta Simon in SilkRoad Life Suite, perform the following steps:**

1. Ask your SilkRoad Life Suite support team to create a user that has as **SSO ID** attribute the same value as the **emailaddress** of Britta Simon in Azure AD.



### Assigning the Azure AD test user

The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to SilkRoad Life Suite.
<br><br>![Assign User][200] <br>

**To assign Britta Simon to SilkRoad Life Suite, perform the following steps:**

1. On the Azure portal, to open the applications view, in the directory view, click **Applications** in the top menu.
<br><br>![Assign User][201] <br>
2. In the applications list, select **SilkRoad Life Suite**.
<br><br>![Assign User][202] <br>
1. In the menu on the top, click **Users**.
<br><br>![Assign User][203] <br>
1. In the Users list, select **Britta Simon**.

2. In the toolbar on the bottom, click **Assign**.
<br><br>![Assign User][205]



### Testing Single Sign-On

The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.<br>
When you click the SilkRoad Life Suite tile in the Access Panel, you should get automatically signed-on to your SilkRoad Life Suite application.


## Additional Resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](active-directory-saas-tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png





