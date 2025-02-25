---
# required metadata

title: Set up the Enrollment Status Page
titleSuffix: Microsoft Intune
description: Set up a greeting page for users enrolling Windows 10 or Windows 11 devices.
keywords:
author: Lenewsad
ms.author: lanewsad
manager: dougeby
ms.date: 10/04/2021
ms.topic: how-to
ms.service: microsoft-intune
ms.subservice: enrollment
ms.localizationpriority: high
ms.technology:
ms.assetid: 8518d8fa-a0de-449d-89b6-8a33fad7b3eb
 
# optional metadata
 
#ROBOTS:
#audience:

ms.reviewer: ericor
ms.suite: ems
search.appverid: MET150
#ms.tgt_pltfrm:
ms.custom: intune-azure;seodec18
ms.collection:
  - M365-identity-device-management
  - highpri
---


 
# Set up the Enrollment Status Page  

**Applies to**
- Windows 10  
- Windows 11  
 
[!INCLUDE [azure_portal](../includes/azure_portal.md)]
 
The Enrollment Status Page (ESP) displays provisioning progress after a new device is enrolled, as well as when new users sign into the device.  This enables IT administrators to optionally prevent (block) access to the device until it has been fully provisioned, while at the same time giving users information about the tasks remaining in the provisioning process.

The ESP can be used as part of any [Windows Autopilot](../../autopilot/index.yml) provisioning scenario, and can also be used separately from Windows Autopilot as part of the default out-of-box experience (OOBE) for Azure AD Join, as well as for any new users signing into the device for the first time.

You can create multiple Enrollment Status Page profiles with different configurations that specify:

- Showing installation progress
- Blocking access until the provisioning process is completed
- Time limits
- Allowed troubleshooting operations

These profiles are specified in a priority order; the highest priority that is applicable will be used.  Each ESP profile can be targeted to groups containing devices or users.  When determining which profile to use, the following criteria will be followed:

- The highest-priority profile targeted to the device will be used first.
- If there are no profiles targeted to the device, the highest priority profile targeted to the current user will be used.  (This only applies in scenarios where there is a user. In white glove and self-deploying scenarios, only device targeting can be used. Only profiles targeted to the device are used during the device preparation and device setup phases.)
- If there are no profiles targeted to specific groups, then the default ESP profile will be used.

## Available settings

The following settings can be configured to customize behavior of the Enrollment Status page:

<table>
<th align="left">Setting<th align="left">Yes<th align="left">No
<tr><td>Show app and profile configuration progress<td>The enrollment status page is displayed.<td>The enrollment status page isn't displayed.
<tr><td>Show an error when installation takes longer than specified number of minutes<td colspan="2">Specify the number of minutes to wait for installation to complete. A default value of 60 minutes is entered.
<tr><td>Show custom message when time limit or error occurs<td>A text box is provided where you can specify a custom message to display if an installation error occurs.<td>The default message is displayed: <br><b>Setup could not be completed. Please try again or contact your support person for help.<b>
<tr><td>Turn on log collection and diagnostics page for end users<td>If there's an installation error, a <b>Collect logs</b> button is displayed. <br>If the user clicks this button, they're asked to choose a location to save the log file <b>MDMDiagReport.cab</b>.<br>In Windows 11, the <b>Windows Autopilot diagnostics</b> page is also displayed.<td>The <b>Collect logs</b> button isn't displayed if there's an installation error.<br>The <b>Windows Autopilot diagnostics</b> page is not displayed in Windows 11.
<tr><td>Only show page to devices provisioned by out-of-box experience (OOBE)<td>The enrollment status page is only shown to devices that go through the out-of-box experience (OOBE).<td>The enrollment status page is shown to devices that go through the out-of-box experience (OOBE) and the first logged on user, but not to subsequent users who logon to the device.
<tr><td>Block device use until all apps and profiles are installed<td>The settings in this table are made available to customize behavior of the enrollment status page, so that the user can address potential installation issues.
<tr><td>Allow users to reset device if installation error occurs<td>A <b>Reset device</b> button is displayed if there's an installation failure.<td>The <b>Reset device</b> button isn't displayed if there's an installation failure.
<tr><td>Allow users to use device if installation error occurs<td>A <b>Continue anyway</b> button is displayed if there's an installation failure.<td>The <b>Continue anyway</b> button isn't displayed if there's an installation failure.
<tr><td>Block device use until these required apps are installed if they are assigned to the user/device<td colspan="2">Choose <b>All</b> or <b>Selected</b>. <br><br>If <b>Selected</b> is chosen, a <b>Select apps</b> button appears that lets you choose which apps must be installed before enabling the device.
</table>

> [!NOTE]
> Be aware that the **Only show page to devices provisioned by out-of-box experience (OOBE)** setting is not *only* applicable to Windows Autopilot devices. If set to **No**, subsequent users will see the user ESP splash screen on any MDM-enrolled device, including co-managed ones. If you only want the ESP screen to appear on Autopilot devices during the initial provisioning, leave the default policy set to **No**. Then create a new ESP profile, set it to **Yes**, and target it to an Autopilot device group. For more information about troubleshooting the ESP page, including how to disable it once it's been configured, see [Understand and troubleshoot the Enrollment Status Page](/troubleshoot/mem/intune/understand-troubleshoot-esp#common-questions-for-troubleshooting-esp-related-issues).  

## Turn on default Enrollment Status Page for all users

To turn on the Enrollment Status Page, follow the steps below.
 
1. In the [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431), choose **Devices** > **Windows** > **Windows enrollment** > **Enrollment Status Page**.
2. In the **Enrollment Status Page** blade, choose **Default** > **Settings**.
3. For **Show app and profile installation progress**, choose **Yes**.
4. Choose the other settings that you want to turn on and then choose **Save**.

## Create Enrollment Status Page profile and assign to a group

1. In the [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431), choose **Devices** > **Windows** > **Windows enrollment** > **Enrollment Status Page** > **Create profile**.
2. Provide a **Name** and **Description**.
3. Choose **Create**.
4. Choose the new profile in the **Enrollment Status Page** list.
5. Optionally, on the **Scope tags** page, you can add the scope tags that you want to apply to this profile. For more information about scope tags, see [Use role-based access control and scope tags for distributed IT](../fundamentals/scope-tags.md). When using scope tags with Enrollment Status Page profiles, users can only re-order profiles for which they have scope. Also, they can only reorder for the profile positions for which they have scope. Users see the true profile priority number on each policy. A scoped user can tell the relative priority of their profile even if they can't see all the other profiles.
6. Choose **Assignments** > **Select groups** > choose the groups that you want to adopt this profile > **Select** > **Save**.
7. Optionally, select **Edit filter** to restrict the policy assignment further.
8. Choose **Settings** > *choose the settings you want to apply to this profile* > **Save**.

> [!NOTE]
> Due to OS restrictions, only a subset of filters are available to use when assigning **Enrollment Status Page** policies. The filter picker will only show filters that have rules defined for `osVersion`, `operatingSystemSKU`, and `enrollmentProfileName` properties. Filters that contain other properties are not shown.

## Set the enrollment status page priority

A device or user can be in many groups and have multiple Enrollment Status Page profiles targeted. To control which profiles are considered first, you can set the priorities for each profile; those with higher priorities are considered first.

1. In the [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431), choose **Devices** > **Windows** > **Windows enrollment** > **Enrollment Status Page**.
2. Hover over the profile in the list.
3. Using the three vertical dots, drag the profile to the desired position on the list.

## Block access to a device until a specific application is installed

You can specify which apps must be installed before the Enrollment Status Page (ESP) completes.

1. In the [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431), choose **Devices** > **Windows** > **Windows enrollment** > **Enrollment Status Page**.
2. Choose a profile > **Settings**.
3. Choose **Yes** for **Show app and profile installation progress**.
4. Choose **Yes** for **Block device use until all apps and profiles are installed**.
5. Choose **Selected** for **Block device use until these required apps are installed if they're assigned to the user/device**.
6. Choose **Select apps** > choose the apps > **Select** > **Save**.

The apps that are included in this list are used by Intune to filter the list that should be considered blocking.  It does not specify what apps should be installed.  For example, if you configure this list to include "App 1," "App 2," and "App 3" and "App 3" and "App 4" are targeted to the device or user, the Enrollment Status Page will track only "App 3."  "App 4" will still be installed, but the Enrollment Status Page will not wait for it to complete.

A maximum of 100 apps can be specified.

## Enrollment Status Page tracking information

There are three phases where the Enrollment Status Page tracks information for; device preparation, device setup, and account setup.

### Device preparation

For device preparation, the enrollment status page tracks:

- Trusted Platform Module (TPM) key attestation (when applicable)
- Azure Active Directory join process
- Intune (MDM) enrollment
- Installation of the Intune Management Extensions (used to install Win32 apps)

### Device setup

The Enrollment Status Page tracks the following device setup items:

- Security policies
  - Microsoft Edge, Assigned Access, and Kiosk Browser policies are presently tracked.
  - Other policies are not tracked.
- Applications
  - Per machine Line-of-business (LoB) MSI apps.
  - LoB store apps with installation context = Device.
  - Offline store and LoB store apps with installation context = Device.
  - Win32 applications (Windows 11 and Windows 10 version 1903 and later only) 

  > [!NOTE]
  > It's preferable to deploy the offline-licensed Microsoft Store for Business apps. Don't mix LOB and Win32 apps. Both LOB (MSI) and Win32 installers use TrustedInstaller, which doesn't allow simultaneous installations. If the OMA DM agent starts an MSI installation, the Intune Management Extension plugin starts a Win32 app installation by using the same TrustedInstaller. In this situation, Win32 app installation fails and returns an **Another installation is in progress, please try again later** error message. In this situation, ESP fails. Therefore, don't mix LOB and Win32 apps in any type of Autopilot enrollment.
  > 

- Connectivity profiles
  - VPN or Wi-Fi profiles that are assigned to **All Devices** or a device group in which the enrolling device is a member, but only for Autopilot devices
- Certificate profiles that are assigned to **All Devices** or a device group in which the enrolling device is a member, but only for Autopilot devices

### Account setup

For account setup, the Enrollment Status Page tracks the following items if they're assigned to the current logged in user:

- Security policies
  - Microsoft Edge, Assigned Access, and Kiosk Browser policies are presently tracked.
  - Other policies are not tracked.
- Applications
  - Per user LoB MSI apps that are assigned to All Devices, All Users, or a user group in which the user enrolling the device is a member.
  - Per machine LoB MSI apps that are assigned to All Users or a user group in which the user enrolling device is a member.
  - LoB store apps, online store apps, and offline store apps that are assigned to any of the following objects:
    - All Devices
    - All Users
    - A user group in which the user enrolling the device is a member with installation context set to User.
  - Win32 applications (Windows 10 version 1903 and newer only) 
- Connectivity profiles
  - VPN or Wi-Fi profiles that are assigned to All Users or a user group in which the user enrolling the device is a member.
- Certificates
  - Certificate profiles that are assigned to All Users or a user group in which the user enrolling the device is a member.

### Known issues

The following are known issues related to the Enrollment Status Page.

- Disabling the ESP profile doesn't remove ESP policy from devices and users still get ESP when they log in to device for first time. The policy isn't removed when the ESP profile is disabled. 
- A reboot during Device setup will force the user to enter their credentials before transitioning to Account setup phase. User credentials aren't preserved during reboot. Have the user enter their credentials then the Enrollment Status Page can continue. 
- Enrollment Status Page will always time out during an Add work and school account enrollment on Windows 10 versions earlier than 1903. The Enrollment Status Page waits for Azure AD registration to complete. The issue is fixed on Windows 10 version 1903 and newer.  
- Hybrid Azure AD Autopilot deployment with ESP takes longer than the timeout duration entered in the ESP profile. On Hybrid Azure AD Autopilot deployments, the ESP will take 40 minutes longer than the value set in the ESP profile. For example, you set the timeout duration to 30 minutes in the profile. The ESP can take 30 minutes + 40 minutes.

  This delay gives time for the on-prem AD connector to create the new device record to Azure AD.
  
- Windows logon page isn't pre-populated with the username in Autopilot User Driven Mode. If there's a reboot during the Device Setup phase of ESP:
  - the user credentials aren't preserved
  - the user must enter the credentials again before proceeding from Device Setup phase to the Account setup phase
- ESP is stuck for a long time or never completes the "Identifying" phase. Intune computes the ESP policies during the identifying phase. A device may never complete computing ESP policies if the current user doesn't have an Intune licensed assigned.  
- Configuring Microsoft Defender Application Control causes a prompt to reboot during Autopilot. Configuring Microsoft Defender Application (AppLocker CSP) requires a reboot. When this policy is configured, it may cause a device to reboot during Autopilot. Currently, there's no way to suppress or postpone the reboot.
- When the [DeviceLock policy](/windows/client-management/mdm/policy-csp-devicelock) is enabled as part of an ESP profile, the OOBE or user desktop autologon could fail unexpectantly for two reasons.
  - If the device didn't reboot before exiting the ESP Device setup phase, the user may be prompted to enter their Azure AD credentials. This prompt occurs instead of a successful autologon where the user sees the Windows first login animation.
  - The autologon will fail if the device rebooted after the user entered their Azure AD credentials but before exiting the ESP Device setup phase. This failure occurs because the ESP Device setup phase never completed. The workaround is to reset the device.
- ESP doesn't apply to a Windows device that was enrolled with Group Policy (GPO).
- Scripts that run in user context ('Run this script using the logged on credentials' on the script properties is set to 'yes') may not execute during ESP.  As a workaround, execute scripts in System context by changing this setting to 'no'.

## Next steps

After you set up Windows enrollment pages, learn how to [manage Windows devices](../remote-actions/device-management.md).

[Troubleshoot the Windows Enrollment Status page](/troubleshoot/mem/intune/understand-troubleshoot-esp#troubleshooting)
