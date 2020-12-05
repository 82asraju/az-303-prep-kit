# Implement Azure Active Directory

[Azure Active Directory - Azure Docs](https://docs.microsoft.com/en-us/azure/active-directory/)

## add custom domains

[Add your custom domain name using the Azure Active Directory portal](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/add-custom-domain)

### Create your directory in Azure AD

After you get your domain name, you can create your first Azure AD directory. Sign in to the Azure portal for your directory, using an account with the Owner role for the subscription.

Create your new directory by following the steps in Create a new tenant for your organization.

> Important
>
> The person who creates the tenant is automatically the Global administrator for that tenant. The Global administrator can add additional administrators to the tenant.

For more information about subscription roles, see Azure roles.

> Tip
>
> If you plan to federate your on-premises Windows Server AD with Azure AD, then you need to select I plan to configure this domain for single sign-on with my local Active Directory when you run the Azure AD Connect tool to synchronize your directories.
>
> You also need to register the same domain name you select for federating with your on-premises directory in the Azure AD Domain step in the wizard. To see what that setup looks like, see Verify the Azure AD domain selected for federation. If you don't have the Azure AD Connect tool, you can download it here.

In the Portal, select Azure Active Directory -> Custom domain names -> Add Custom Domain Name. Enter the custom domain name and click "Add Domain".

> Important
>
> You must include .com, .net, or any other top-level extension for this to work properly.

The unverified domain is added. The contoso.com page appears showing your DNS information. Save this information. You need it later to create a TXT record to configure DNS.

### Add your DNS information to the domain registrar

After you add your custom domain name to Azure AD, you must return to your domain registrar and add the Azure AD DNS information from your copied TXT file. Creating this TXT record for your domain verifies ownership of your domain name.

Go back to your domain registrar and create a new TXT record for your domain based on your copied DNS information. Set the time to live (TTL) to 3600 seconds (60 minutes), and then save the record.

> Important
>
>You can register as many domain names as you want. However, each domain gets its own TXT record from Azure AD. Be careful when you enter the TXT file information at the domain registrar. If you enter the wrong or duplicate information by mistake, you'll have to wait until the TTL times out (60 minutes) before you can try again.

### Verify your custom domain name

After you register your custom domain name, make sure it's valid in Azure AD. The propagation from your domain registrar to Azure AD can be instantaneous or it can take a few days, depending on your domain registrar.

To verify your custom domain name, follow these steps:

1. Sign in to the Azure portal using a Global administrator account for the directory.
2. Search for and select Azure Active Directory from any page, then select Custom domain names.
3. In Custom domain names, select the custom domain name. In this example, select contoso.com.
4. On the contoso.com page, select Verify to make sure your custom domain is properly registered and is valid for Azure AD.

After you've verified your custom domain name, you can delete your verification TXT or MX file.

## configure Azure AD Identity Protection

[What is Azure Active Directory Identity Protection?](https://docs.microsoft.com/en-us/azure/active-directory/identity-protection/overview-identity-protection)

Identity Protection is a tool that allows organizations to accomplish three key tasks:

1. Automate the detection and remediation of identity-based risks.
2. Investigate risks using data in the portal.
3. Export risk detection data to third-party utilities for further analysis.

Identity Protection uses the learnings Microsoft has acquired from their position in organizations with Azure AD, the consumer space with Microsoft Accounts, and in gaming with Xbox to protect your users. Microsoft analyses 6.5 trillion signals per day to identify and protect customers from threats.

### Why is automation important?

In his blog post in October of 2018 Alex Weinert, who leads Microsoft's Identity Security and Protection team, explains why automation is so important when dealing with the volume of events:

> Each day, our machine learning and heuristic systems provide risk scores for 18 billion login attempts for over 800 million distinct accounts, 300 million of which are discernibly done by adversaries (entities like: criminal actors, hackers).

### Risk Detection and Remediation

Identity Protection identifies risks in the following classifications:

|Risk detection type	|Description|
|:--|:--|
|Atypical travel	|Sign in from an atypical location based on the user's recent sign-ins.|
|Anonymous IP address	|Sign in from an anonymous IP address (for example: Tor browser, anonymizer VPNs).|
|Unfamiliar sign-in properties	|Sign in with properties we've not seen recently for the given user.|
|Malware linked IP address	|Sign in from a malware linked IP address.|
|Leaked Credentials	|Indicates that the user's valid credentials have been leaked.|
|Password spray	|Indicates that multiple usernames are being attacked using common passwords in a unified, brute-force manner.|
|Azure AD threat intelligence	|Microsoft's internal and external threat intelligence sources have identified a known attack pattern.|

### Risk investigation

Administrators can review detections and take manual action on them if needed. There are three key reports that administrators use for investigations in Identity Protection:

- Risky users
- Risky sign-ins
- Risk detections

More information can be found in the article, How To: Investigate risk.

### Risk levels

Identity Protection categorizes risk into three tiers: low, medium, and high.

While Microsoft does not provide specific details about how risk is calculated, we will say that each level brings higher confidence that the user or sign-in is compromised. For example, something like one instance of unfamiliar sign-in properties for a user might not be as threatening as leaked credentials for another user.

### Permissions

Identity Protection requires users be a Security Reader, Security Operator, Security Administrator, Global Reader, or Global Administrator in order to access.

|Role	|Can do	|Can't do|
|:--|:--|:--|
|Global administrator	|Full access to Identity Protection	||
|Security administrator	|Full access to Identity Protection	|Reset password for a user|
|Security operator	|View all Identity Protection reports and Overview blade<br />Dismiss user risk, confirm safe sign-in, confirm compromise|Configure or change policies</br>Reset password for a user<br />Configure alerts|
|Security reader	|View all Identity Protection reports and Overview blade|Configure or change policies<br />Reset password for a user<br />Configure alerts<br />Give feedback on detections|

Currently, the security operator role cannot access the Risky sign-ins report.

Conditional Access administrators can also create policies that factor in sign-in risk as a condition. Find more information in the article [Conditional Access: Conditions](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/concept-conditional-access-conditions#sign-in-risk).

### License requirements

Using this feature requires an Azure AD Premium P2 license. To find the right license for your requirements, see Comparing generally available features of the Free, Office 365 Apps, and Premium editions.

|Capability	Details	|Azure AD Free / Microsoft 365 Apps	|Azure AD Premium P1	|Azure AD Premium P2|
|:--|:--|:--|:--|
|Risk policies	|User risk policy (via Identity Protection)|	No	|No|	Yes|
|Risk policies	|Sign-in risk policy (via Identity Protection or Conditional Access)|	No	|No|	Yes|
|Security reports|	Overview|	No	|No|	Yes|
|Security reports	|Risky users	|Limited Information. Only users with medium and high risk are shown. No details drawer or risk history.	|Limited Information. Only users with medium and high risk are shown. No details drawer or risk history.	|Full access|
|Security reports	|Risky sign-ins	|Limited Information. No risk detail or risk level is shown.|	Limited Information. No risk detail or risk level is shown.	|Full access|
|Security reports	|Risk detections	|No	|Limited Information. No details drawer.	|Full access|
|Notifications	|Users at risk detected alerts	|No	|No|	Yes|
|Notifications	|Weekly digest	|No	|No|	Yes|
||MFA registration policy	|No	|No|	Yes|

## implement self-service password reset

[Plan an Azure Active Directory self-service password reset](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-sspr-deployment)

> Self-Service Password Reset (SSPR) is an Azure Active Directory (AD) feature that enables users to reset their passwords without contacting IT staff for help. The users can quickly unblock themselves and continue working no matter where they are or time of day. By allowing the employees to unblock themselves, your organization can reduce the non-productive time and high support costs for most common password-related issues.

SSPR has the following key capabilities:

- Self-service allows end users to reset their expired or non-expired passwords without contacting an administrator or helpdesk for support.
- [Password Writeback](https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-sspr-writeback) allows management of on-premises passwords and resolution of account lockout though the cloud.
- Password management activity reports give administrators insight into password reset and registration activity occurring in their organization.

### Licensing

Azure Active Directory is licensed per-user meaning each user requires an appropriate license for the features they use. We recommend group-based licensing for SSPR.

### Prerequisites

- A working Azure AD tenant with at least a trial license enabled. If needed, create one for free.
- An account with Global Administrator privileges.

### Solution architecture

The following example describes the password reset solution architecture for common hybrid environments.

![SSPR User Flow](https://docs.microsoft.com/en-us/azure/active-directory/authentication/media/howto-sspr-deployment/solutions-architecture.png)

Description of workflow

To reset the password, users go to the password reset portal. They must verify the previously registered authentication method or methods to prove their identity. If they successfully reset the password, they begin the reset process.

- For cloud-only users, SSPR stores the new password in Azure AD.
- For hybrid users, SSPR writes back the password to the on-prem Active Directory via the Azure AD Connect service.

Note: For users who have Password hash synchronization (PHS) disabled, SSPR stores the passwords in the on-prem Active Directory only.

### Best practices

You can help users register quickly by deploying SSPR alongside another popular application or service in the organization. This action will generate a large volume of sign-ins and will drive registration.

Before deploying SSPR, you may opt to determine the number and the average cost of each password reset call. You can use this data post deployment to show the value SSPR is bringing to the organization.

### Enable combined registration for SSPR and MFA

Microsoft recommends that organizations enable the combined registration experience for SSPR and multi-factor authentication. When you enable this combined registration experience, users need only select their registration information once to enable both features.

The combined registration experience does not require organizations to enable both SSPR and Azure AD Multi-Factor Authentication. Combined registration provides organizations a better user experience. For more information, see [Combined security information registration](https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-registration-mfa-sspr-combined)

### Required administrator roles

|Business Role/Persona	|Azure AD Role (if necessary)|
|:--|:--|
|Level 1 helpdesk	|Password administrator|
|Level 2 helpdesk	|User administrator|
|SSPR administrator	|Global administrator|

### Password Writeback

Password Writeback is enabled with Azure AD Connect and writes password resets in the cloud back to an existing on-premises directory in real time. For more information, see What is Password Writeback?

We recommend the following settings:

- Ensure that Write back passwords to on-premises AD is set to Yes.
- Set the Allow users to unlock account without resetting password to Yes.

By default, Azure AD unlocks accounts when it performs a password reset.

### Administrator password setting

Administrator accounts have elevated permissions. The on-premises enterprise or domain administrators can't reset their passwords through SSPR. On-premises admin accounts have the following restrictions:

- can only change their password in their on-prem environment.
- can never use the secret questions and answers as a method to reset their password.

We recommend that you don't sync your on-prem Active Directory admin accounts with Azure AD.

### Plan rollback

To roll back the deployment:

- for a single user, remove the user from the security group
- for a group, remove the group from SSPR configuration
- For everyone, disable SSPR for the Azure AD tenant

### Manage SSPR

Azure AD can provide additional information on your SSPR performance through audits and reports.

### Password management activity reports

You can use pre-built reports on Azure portal to measure the SSPR performance. If you're appropriately licensed, you can also create custom queries. For more information, see Reporting options for Azure AD password management

> Note
>
> You must be a global administrator, and you must opt-in for this data to be gathered for your organization. To opt in, you must visit the Reporting tab or the audit logs on the Azure Portal at least once. Until then, the data doesn't collect for your organization.

Audit logs for registration and password reset are available for 30 days. If security auditing within your corporation requires longer retention, the logs need to be exported and consumed into a SIEM tool such as Azure Sentinel, Splunk, or ArcSight.

[How it works: Azure AD self-service password reset](https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-sspr-howitworks)
[Licensing requirements for Azure AD self-service password reset](https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-sspr-licensing)

### Compare editions and features

SSPR is licensed per user. To maintain compliance, organizations are required to assign the appropriate license to their users.

The following table outlines the different SSPR scenarios for password change, reset, or on-premises writeback, and which SKUs provide the feature.

|Feature	|Azure AD Free	|Microsoft 365 Business Standard	|Microsoft 365 Business Premium	|Azure AD Premium P1 or P2|
|:--|:--|:--|:--|:--|
|Cloud-only user password change<br />When a user in Azure AD knows their password and wants to change it to something new.|	●	|●	|●	|●|
|Cloud-only user password reset<br />When a user in Azure AD has forgotten their password and needs to reset it.||		●|	●|	●|
|Hybrid user password change or reset with on-prem writeback<br />When a user in Azure AD that's synchronized from an on-premises directory using Azure AD Connect wants to change or reset their password and also write the new password back to on-prem.|||●|●|

### Enable group or user-based licensing

Azure AD supports group-based licensing. Administrators can assign licenses in bulk to a group of users, rather than assigning them one at a time. For more information, see Assign, verify, and resolve problems with licenses.

## implement Conditional Access including MFA

[Conditional Access: Require MFA for all users](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/howto-conditional-access-policy-all-users-mfa)
[Conditional Access: Risk-based Conditional Access](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/howto-conditional-access-policy-risk)

## configure user accounts for MFA

[Tutorial: Secure user sign-in events with Azure Multi-Factor Authentication](https://docs.microsoft.com/en-us/azure/active-directory/authentication/tutorial-enable-azure-mfa)

## configure fraud alerts

[Reports in Azure Multi-Factor Authentication](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-reporting)
[Configure Azure Multi-Factor Authentication settings](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings)

## configure bypass options

[Configure Azure Multi-Factor Authentication settings](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings)

## configure Trusted Ips

[Quickstart: Configure named locations in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)
[What is the location condition in Azure Active Directory Conditional Access?](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/location-condition)

## configure verification methods

[Change your two-factor verification method and settings](https://docs.microsoft.com/en-us/azure/active-directory/user-help/multi-factor-authentication-end-user-manage-settings)
[What is the Additional verification page?](https://docs.microsoft.com/en-us/azure/active-directory/user-help/multi-factor-authentication-end-user-first-time)

## implement and manage guest accounts

[What is guest user access in Azure Active Directory B2B?](https://docs.microsoft.com/en-us/azure/active-directory/b2b/what-is-b2b)
[Manage guest access with Azure AD access reviews](https://docs.microsoft.com/en-us/azure/active-directory/governance/manage-guest-access-with-access-reviews)
[Quickstart: Add guest users to your directory in the Azure portal](https://docs.microsoft.com/en-us/azure/active-directory/b2b/b2b-quickstart-add-guest-users-portal)

## manage multiple directories

[Understand how multiple Azure Active Directory tenants interact](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/licensing-directory-independence)