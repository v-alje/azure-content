
<properties
	pageTitle="Using attributes to create advanced rules| Microsoft Azure"
	description="How-to's to create advanced rules for a group including supported expression rule operators and parameters."
	services="active-directory"
	documentationCenter=""
	authors="curtand"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="02/07/2016"
	ms.author="curtand"/>


# Using attributes to create advanced rules
The Azure portal provides you with the flexibility to set up advanced rules in Azure Active Directory (Azure AD) to enable more complex dynamic memberships for Azure AD groups.

**To create the advanced rule**
In the Azure portal, under the group’s **Configure** tab, select the **Advanced rule** option and then type in your advanced rule in the provided text box. You can create your advanced rule using the following information.

## Constructing the body of an advanced rule
The advanced rule that you can create for the dynamic memberships for groups is essentially a binary expression that consists of three parts and results in a true or false outcome. The three parts are:

- Left parameter
- Binary operator
- Right constant

A complete advanced rule looks similar to this: (leftParameter binaryOperator "RightConstant"), where the opening and closing parenthesis are required for the entire binary expression, double quotes are required for the right constant, and the syntax for the left parameter is user.property. An advanced rule can consist of more than one binary expressions separated by the -and, -or, and -not logical operators.
The following are examples of a properly constructed advanced rule:

- (user.department -eq "Sales") -or (user.department -eq "Marketing")
- (user.department -eq "Sales") -and -not (user.jobTitle -contains "SDE")

For the complete list of supported parameters and expression rule operators, see sections below.

The total length of the body of your advanced rule cannot exceed 2048 characters.
> [AZURE.NOTE]
>String and regex operations are case insensitive. You can also perform Null checks, using $null as a constant, for example, user.department -eq $null.
Strings containing quotes " should be escaped using 'character, for example, user.department -eq "Sa`"les".

##Supported expression rule operators
The following table lists all the supported expression rule operators and their syntax to be used in the body of the advanced rule:

| Operator        | Syntax         |
|-----------------|----------------|
| Not Equals      | -ne            |
| Equals          | -eq            |
| Not Starts With | -notStartsWith |
| Starts With     | -startsWith    |
| Not Contains    | -notContains   |
| Contains        | -contains      |
| Not Match       | -notMatch      |
| Match           | -match         |


| Query Parse Error                                                    | Error Usage                                                                                                        | Corrected Usage                                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Error: Attribute not supported.                                      | (user.invalidProperty -eq "Value")                                                                                 | (user.department -eq "value")Property should match one from the supported properties list above.                                                                                                                                                                           |
| Error: Operator is not supported on attribute.                       | (user.accountEnabled -contains true)                                                                               | (user.accountEnabled -eq true)Property is of type boolean. Use the supported operators (-eq or -ne) on boolean type from the above list.                                                                                                                                   |
| Error: Query compilation error.                                      | (user.department -eq "Sales") -and (user.department -eq "Marketing")(user.userPrincipalName -match "*@domain.ext") | (user.department -eq "Sales") -and (user.department -eq "Marketing")Logical operator should match one from the supported properties list above.(user.userPrincipalName -match ".*@domain.ext")or(user.userPrincipalName -match "@domain.ext$")Error in regular expression. |
| Error: Binary expression is not in right format.                     | (user.department –eq “Sales”) (user.department -eq "Sales")(user.department-eq"Sales")                             | (user.accountEnabled -eq true) -and (user.userPrincipalName -contains "alias@domain")Query has multiple errors. Parenthesis not in right place.                                                                                                                            |
| Error: Unknown error occurred during setting up dynamic memberships. | (user.accountEnabled -eq "True" AND user.userPrincipalName -contains "alias@domain")                               | (user.accountEnabled -eq true) -and (user.userPrincipalName -contains "alias@domain")Query has multiple errors. Parenthesis not in right place.                                                                                                                            |

##Supported Parameters
The following are all the user properties that you can use in your advanced rule:

**Properties of type boolean**

Allowed operators

* -eq


* -ne


| Properties     | Allowed values  | Usage                          |
|----------------|-----------------|--------------------------------|
| accountEnabled | true false      | user.accountEnabled -eq true)  |
| dirSyncEnabled | true false null | (user.dirSyncEnabled -eq true) |

**Properties of type string**

Allowed operators

* -eq


* -ne


* -notStartsWith


* -StartsWith


* -contains


* -notContains


* -match


* -notMatch

| Properties                 | Allowed values                                                                                        | Usage                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| city                       | Any string value or $null.                                                                            | (user.city -eq "value")                                   |
| country                    | Any string value or $null.                                                                            | (user.country -eq "value")                                |
| department                 | Any string value or $null.                                                                            | (user.department -eq "value")                             |
| displayName                | Any string value.                                                                                     | (user.displayName -eq "value")                            |
| facsimileTelephoneNumber   | Any string value or $null.                                                                            | (user.facsimileTelephoneNumber -eq "value")               |
| givenName                  | Any string value or $null.                                                                            | (user.givenName -eq "value")                              |
| jobTitle                   | Any string value or $null.                                                                            | (user.jobTitle -eq "value")                               |
| mail                       | Any string value or $null. SMTP address of the user.                                                  | (user.mail -eq "value")                                   |
| mailNickName               | Any string value. Mail alias of the user.                                                             | (user.mailNickName -eq "value")                           |
| mobile                     | Any string value or $null.                                                                            | (user.mobile -eq "value")                                 |
| objectId                   | GUID of the user object                                                                               | (user.objectId -eq "1111111-1111-1111-1111-111111111111") |
| passwordPolicies           | None DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |   (user.passwordPolicies -eq "DisableStrongPassword")                                                      |
| physicalDeliveryOfficeName | Any string value or $null.                                                                            | (user.physicalDeliveryOfficeName -eq "value")             |
| postalCode                 | Any string value or $null.                                                                            | (user.postalCode -eq "value")                             |
| preferredLanguage          | ISO 639-1 code                                                                                        | (user.preferredLanguage -eq "en-US")                      |
| sipProxyAddress            | Any string value or $null.                                                                            | (user.sipProxyAddress -eq "value")                        |
| state                      | Any string value or $null.                                                                            | (user.state -eq "value")                                  |
| streetAddress              | Any string value or $null.                                                                            | (user.streetAddress -eq "value")                          |
| surname                    | Any string value or $null.                                                                            | (user.surname -eq "value")                                |
| telephoneNumber            | Any string value or $null.                                                                            | (user.telephoneNumber -eq "value")                        |
| usageLocation              | Two lettered country code                                                                             | (user.usageLocation -eq "US")                             |
| userPrincipalName          | Any string value.                                                                                     | (user.userPrincipalName -eq "alias@domain")               |
| userType                   | member guest $null                                                                                    | (user.userType -eq "Member")                              |

**Properties of type string collection**

Allowed operators

* -contains


* -notContains

| Poperties      | Allowed values                        | Usage                                                |
|----------------|---------------------------------------|------------------------------------------------------|
| otherMails     | Any string value                      | (user.otherMails -contains "alias@domain")           |
| proxyAddresses | SMTP: alias@domain smtp: alias@domain | (user.proxyAddresses -contains "SMTP: alias@domain") |

## Extension attributes and custom attributes
Extension attributes and custom attributes are supported in dynamic membership rules.

Extension attributes are synced from on premise Window Server AD and take the format of "ExtensionAttributeX", where X equals 1 - 15.
An example of a rule that uses an extension attribute would be

(user.extensionAttribute15 -eq "Marketing")

Custom Attributes are synced from on premise Windows Server AD or from a connected SaaS application and the the format of "user.extension_[GUID]__[Attribute]", where [GUID] is the unique identifier in AAD for the application that created the attribute in AAD and [Attribute] is the name of the attribute as it was created.
An example of a rule that uses a custom attribute is

user.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

The custom attribute name can be found in the directory by querying a user's attribute using Graph Explorer and searching for the attribute name.

## Direct Reports Rule
You can now populate members in a group based on the manager attribute of a user.
To configure a group as a “Manager” group
--------------------------------------------------------------------------------
1. On the Administrator portal, click the **Configure** tab, and then select **ADVANCED RULE**.
2. Type the rule with the following syntax:
Direct Reports for *Direct Reports for {UserID_of_manager}*. An example of a valid rule for Direct Reports is

Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

where “62e19b97-8b3d-4d4a-a106-4ce66896a863” is the objectID of the manager. The object ID can be found in the AAD Admin Portal on the profile tab of the user page of the user that is the manager.

3. When saving this rule, all users that satisfy the rule will be joined as members of the group. Note that it can take some minutes for the group to initially populate.


## Additional information
These articles provide additional information on Azure Active Directory.

* [Troubleshooting dynamic memberships for groups](active-directory-accessmanagement-troubleshooting.md)

* [Managing access to resources with Azure Active Directory groups](active-directory-manage-groups.md)

* [What is Azure Active Directory?](active-directory-whatis.md)

* [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md)
