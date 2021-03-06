---
# required metadata

title: Decommissioning and deactivating the Azure Rights Management service| Azure Information Protection
description: Information and instructions if you decide you no longer want to use this information protection service from Azure Information Protection.
author: cabailey
ms.author: cabailey
manager: mbaldwin
ms.date: 09/25/2016
ms.topic: article
ms.prod:
ms.service: information-protection
ms.technology: techgroup-identity
ms.assetid: 0b1c2064-0d01-45ae-a541-cebd7fd762ad

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: esaggese
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Decommissioning and deactivating Azure Rights Management

>*Applies to: Azure Information Protection, Office 365*

You are always in control of whether your organization protects content by using the Azure Rights Management service from Azure Information Protection, and if you decide you no longer want to use this information protection service, you have the assurance that you won’t be locked out of content that was previously protected. If you don’t need continued access to previously protected content, you simply deactivate the service and you can let your subscription for Azure Information Protection expire. For example, this would be appropriate for when you have completed testing Azure Information Protection before you deploy it in a production environment.

However, if you have deployed Azure Information Protection in production and protected documents and emails, make sure that you have a copy of your Azure Information Protection tenant key before you deactivate the Azure Rights Management service and do this before your subscription expires, because this will ensure that you can retain access to content that was protected by Azure Rights Management after the service is deactivated. If you used the bring your own key solution (BYOK) where you generate and manage your own key in an HSM, you will already have your Azure Information Protection tenant key. But if it was managed by Microsoft (the default), see the instructions for exporting your tenant key in [Operations for your Azure Rights Management tenant key](operations-tenant-key.md) article.

> [!TIP]
> Even after your subscription expires, your Azure Information Protection tenant remains available for consuming content for an extended period. However, you will no longer be able to export your tenant key.

When you have your Azure Information Protection tenant key, you can deploy Rights Management on premises (AD RMS) and import your tenant key as a trusted publishing domain (TPD). You then have the following options for decommissioning your Azure Information Protection deployment:

|If this applies to you …|… do this:|
|----------------------------|--------------|
|You want all users to continue using Rights Management, but use an on-premises solution rather than using Azure Information Protection    →|Use the [Set-AadrmMigrationUrl](https://msdn.microsoft.com/library/azure/dn629429.aspx) cmdlet to direct existing users to your on-premises deployment when they consume content protected after this change. Users will automatically use the AD RMS installation to consume the protected content.<br /><br />For users to consume content that was protected before this change, redirect your clients to the on-premises deployment by using the **LicensingRedirection** registry key for Office 2016 or Office 2013, as described in the [service discovery section](../rms-client/client-deployment-notes.md) in the RMS client deployment notes, and the **LicenseServerRedirection** registry key for Office 2010, as described in [Office Registry Settings](https://technet.microsoft.com/library/dd772637%28v=ws.10%29.aspx).|
|You want to stop using Rights Management technologies completely    →|Grant a designated administrator [super user rights](../deploy-use/configure-super-users.md) and give this person the [RMS Protection Tool](http://www.microsoft.com/en-us/download/details.aspx?id=47256).<br /><br />This administrator can then use the tool to bulk-decrypt files in folders that were protected by the Azure Rights Management service so that the files revert to being unprotected and can therefore be read without a Rights Management technology such as Azure Information Protection or AD RMS. This tool can be used with both the Azure Rights Management service from Azure Information Protection and AD RMS, so you have the choice of decrypting files before or after you deactivate the Azure Rights Management service, or a combination.|
|You are not able to identify all the files that were protected by the Azure Rights Management service from Azure Information Protection, or you want all users to be able to automatically read any protected files that were missed    →|Deploy a registry setting on all client computers by using the **LicensingRedirection** registry key for Office 2016 and Office 2013, as described in the [service discovery section](../rms-client/client-deployment-notes.md) in the RMS client deployment notes, and the **LicenseServerRedirection** registry key for Office 2010, as described in [Office Registry Settings](https://technet.microsoft.com/library/dd772637%28v=ws.10%29.aspx).<br /><br />Also deploy another registry setting to prevent users from protecting new files by setting **DisableCreation** to **1**, as described in [Office Registry Settings](https://technet.microsoft.com/library/dd772637%28v=ws.10%29.aspx).|
|You want a controlled, manual recovery service for any files that were missed    →|Grant designated users in a data recovery group [super user rights](../deploy-use/configure-super-users.md) and give them the [RMS Protection Tool](http://www.microsoft.com/en-us/download/details.aspx?id=47256) so that they can unprotect files when requested by standard users.<br /><br />On all computers, deploy the registry setting to prevent users from protecting new files by setting **DisableCreation** to **1**, as described in [Office Registry Settings](https://technet.microsoft.com/library/dd772637%28v=ws.10%29.aspx).|
For more information about the procedures in this table, see the following resources:

-   For information about AD RMS and deployment references, see [Active Directory Rights Management Services Overview](https://technet.microsoft.com/library/hh831364.aspx).

-   For instructions to import your Azure Information Protection tenant key as a TPD file, see [Add a Trusted Publishing Domain](https://technet.microsoft.com/library/cc771460.aspx).

-   To install the Windows PowerShell module for Azure Rights Management, to set the migration URL, see [Installing Windows PowerShell for Azure Rights Management](install-powershell.md).

When you are ready to deactivate the Azure Rights Management service for your organization, use the following instructions.

## Deactivating Rights Management
Use one of the following procedures to deactivate [!INCLUDE[aad_rightsmanagement_1](../includes/aad_rightsmanagement_1_md.md)].

> [!TIP]
> You can also use the Windows PowerShell cmdlet, [Disable-Aadrm](http://msdn.microsoft.com/library/windowsazure/dn629422.aspx), to deactivate [!INCLUDE[aad_rightsmanagement_2](../includes/aad_rightsmanagement_2_md.md)].

#### To deactivate Rights Management from the Office 365 admin center

1.  [Sign in to Office 365 with your work or school account](https://portal.office.com/) that is an administrator for your Office 365 deployment.

2.  If the Office 365 admin center does not automatically display, select the app launcher icon in the upper-left and choose **Admin**. The **Admin** tile appears only to Office 365 administrators.

    > [!TIP]
    > For admin center help, see [About the Office 365 admin center - Admin Help](https://support.office.com/article/About-the-Office-365-admin-center-Admin-Help-58537702-d421-4d02-8141-e128e3703547).

3.  In the left pane, expand **SERVICE SETTINGS**.

4.  Click **Rights Management**.

5.  On the **RIGHTS MANAGEMENT** page, click **Manage**.

6.  On the **rights management** page, click **deactivate**.

7.  When prompted **Do you want to deactivate Rights Management?**, click **deactivate**.

You should now see **Rights Management is not activated** and the option to activate.

#### To deactivate Rights Management from the Azure classic portal

1.  Sign in to the [Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=275081).

2.  In the left pane, click **ACTIVE DIRECTORY**.

3.  From the **active directory** page, click **RIGHTS MANAGEMENT**.

4.  Select the directory to manage for [!INCLUDE[aad_rightsmanagement_2](../includes/aad_rightsmanagement_2_md.md)], click **DEACTIVATE**, and then confirm your action.

The **RIGHTS MANAGEMENT STATUS** should now display **Inactive** and the **DEACTIVATE** option is replaced with **ACTIVATE**.



