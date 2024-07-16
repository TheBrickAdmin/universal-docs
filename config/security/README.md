---
description: Security features of PowerShell Universal.
---

# About

## Authorization

User authorization is accomplished with roles. Roles can either be assigned through claims mapping, a policy script or by assigning the role directly to the identity.

{% hint style="info" %}
By default, users will receive all roles when logging in. Multiple role assignments are valid in PowerShell Universal. While you configure roles, you can choose to disable roles you are not yet or do not plan to use.

Disabling the Administrators role will prevent you from making changes to roles within the Admin Console.
{% endhint %}

### Role to Claim Mapping

You can map roles to a claim (such as a group membership) by using the `-ClaimType` and `-ClaimValue` parameters of `New-PSURole`. Settings are also available in the role properties dialog within Security \ Roles.

![](<../../.gitbook/assets/image (235).png>)

For example, with Windows authentication, if you wanted to map a group to a role, you could configure it such that the group SID maps to the administrator role.

```powershell
New-PSURole -Name Administrator -ClaimType 'http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid' -ClaimValue 'S-123-123-123'
```

Mapping roles to claims in this manner is faster than Policy scripts because it does not require PowerShell to be run when the user is logging in.

### View Claim Information

To help develop policy scripts or assign roles to claims, you can view claim information by clicking View Claim Information in Security \ Roles.

![View Claim Information](<../../.gitbook/assets/image (363).png>)

### Example: Azure Active Directory

You can map an Azure Active Directory group to a role by looking up the group Object ID in Azure. For example, within the Ironman Software domain, we have a group called Dashboard Administrators. This group has an object ID of `61849bf2-e44b-4057-b589-6cd1812d7545`.

Within PowerShell Universal, I can assign users of this group to the Administrator group by setting up the claim mapping. The Claim Type will be `groups` and the Claim Value will be `61849bf2-e44b-4057-b589-6cd1812d7545`. Once I have mapped the claim, users of the Dashboard Administrators group will be part of the PowerShell Universal Administrators group. The resulting `roles.ps1` will look like this.

All other roles are disabled.

```powershell
New-PSURole -Name Administrator -ClaimType 'groups' -ClaimValue '61849bf2-e44b-4057-b589-6cd1812d7545'
New-PSURole -Name "Operator" -Description "Operators have access to manage and execute scripts, create other entities within PowerShell Universal but cannot manage PowerShell Universal itself." -Policy {} -Disabled
New-PSURole -Name "Reader" -Description "Readers have read-only access to PowerShell Universal. They cannot make changes to any entity within the system." -Policy { } -Disabled 
New-PSURole -Name "Execute" -Description "Execute scripts within PowerShell Universal." -Policy { } -Disabled
New-PSURole -Name "User" -Description "Does not have access to the admin console but can be assigned resources like APIs, scripts, dashboards and pages." -Policy { } -Disabled
```

### Policy Assignment

By default, roles are assigned by policies. Policies are run when the user logs in. You can change the policy scripts by visiting the Security / Roles page. Click the Edit Policy button to configure the Policy script.

![](<../../.gitbook/assets/image (14).png>)

Policy scripts will receive a `ClaimsPrincipal` object as a parameter and need to return true or false. Policies that throw errors will be assumed to be false. The `ClaimsPrincipal` object contains the user's identity and the claims that the user has received. These may include group assignments or other features of a user's account.

You can expect an object with this structure.

```csharp
public class ClaimsPrincipal
{
    public List<Claim> Claims { get; set; } = new List<Claim>();
    public Identity Identity { get; set; } = new Identity();
}

public class Identity 
{
    public string Name { get ;set; }
}

public class Claim 
{
    public string Type { get; set; }  
    public string Value { get; set; }
    public string ValueType { get; set; } 
    public string Issuer { get; set; }
    public Dictionary<string, string> Properties { get; set; } = new Dictionary<string, string>();
}
```

### Role Assignment

To assign a role to a user, you can create their identity within Universal and then select the role in the drop down on the Identities page.

![](<../../.gitbook/assets/image (565).png>)

By default, identities receive a role through claim mapping or policy.

![](<../../.gitbook/assets/image (555).png>)

### Built in Roles

#### Administrator

Full access to the entire PowerShell Universal platform and settings.

#### Operator

Operators have access to add and remove resources such as APIs, Scripts and Dashboards. Operators cannot change settings like environments, roles, or general settings.

#### Execute

The Execute role grants the ability to run scripts and read access for everything else.

#### Reader

The Reader role provides read-only access to PowerShell Universal.

### Default Route per Role

You can change which page the user sees when logging in by setting the `Default Route` property for the role. For example, you may want HR users to go to the Human Resources dashboard while you want IT users to go to the IT Dashboard.

### Users with Many Groups

If your users are members of more than about 40 groups you may experience problems logging in. This is due to size limits of the HTTP headers in IIS and Kestrel. The more groups a user is a member of, the more authorization claims they have and the large the header.

You can increase the header limit for Kestrel by using the limits configuration in `appsettings.json` file. You will need to increase the header size. It is a value in bytes and defaults to 32kb.

```
{
  "Kestrel": {
    "Endpoints": {
      "HTTP": {
        "Url": "http://*:5000"
      }
    },
    "Limits": {
      "MaxRequestHeadersTotalSize": 132768
    },
    "RedirectToHttps": "false"
  },
```

### IIS Authorization

Authorization in IIS works as with any other method but you need to be aware of the request header size limit. You may receive errors when you enable claims that include many groups. They can exceed the header size limit and IIS will return errors. We have found that about 40 Azure Active Directory groups will cause this issue on a default IIS installation.

The error you will receive will either be a 400 error with the request is too long.

![](<../../.gitbook/assets/image (18).png>)

If you have HTTPS enabled, you will receive an error about a HTTP2 protocol error.

![](<../../.gitbook/assets/image (15).png>)

You can increase the IIS request size by setting the following registry keys. You will need to restart you machine in order for them to take affect.

```
HKLM:\System\CurrentControlSet\Services\Http\Parameters
    MaxFieldLength: DWORD

HKLM:\System\CurrentControlSet\Services\Http\Parameters
    MaxRequestBytes: DWORD
```

More information can be found on [Microsoft's documentation](https://docs.microsoft.com/en-us/troubleshoot/iis/http-bad-request-response-kerberos#workaround-2-set-maxfieldlength-and-maxrequestbytes-registry-entries).

As an alternative to increasing the request size, you can also reduce the number of groups sent. In Azure Active Directory, you can set to just the groups assigned to the application to prevent all groups from being sent.

In Azure go to **App registrations** > (Select the app) > **Token Configuration**, and specify Groups assigned to the application. ![](https://support.ironmansoftware.com/api/v1/threads/548223000001702109/inlineImages/edbsndabe757f382adbd6bf97fb8f980f999a0299e9db01c2972b353b3c8c4ffe29e6ae82d11d75341af2d224cd8f4103b9b009cb9d18e2809c18ece1c19c88900fe13e6b2866bb6604db1b7c9360f4da552d?et=17798841e64\&ha=5d1aea069a1f58d946c8dad4e93abec12ca48d705aad7500a321b0c311d7e581\&f=1.png)

Now go to **Enterprise Application** > (Select the app) > **Users and groups**. Assign the group(s) you are interested in including in the claims. (Note: this can also be used as a security boundary if you set “User Assignment Required” to Yes in the ‘Properties’ section of the app)

![](https://support.ironmansoftware.com/api/v1/threads/548223000001702109/inlineImages/edbsndabe757f382adbd6bf97fb8f980f999a0299e9db01c2972b353b3c8c4ffe29e6ae82d11d75341af2d224cd8f4103b9b0ba101ed249f6cf46a1cf05c5cfe5650d84cedc32ef9ab20a4535665ec422da7b?et=17798841e64\&ha=0397e6316da5e7ab913c1ec8c932ba854c1a88d27c54044ea299ca2dac438aef\&f=2.png)

## App Tokens

App Tokens can be assigned to services that cannot login interactively. You can grant a new app token to your account by clicking the Grant App Token button within the Security / App Tokens tab.

The token will have a expiration of one year and have the valid roles for your account. To copy the App Token to your account, click the Copy action. To revoke an App Token, click the Revoke action.

You can use App Tokens with the Universal cmdlets or by using web requests directly using Bearer authorization.

![](<../../.gitbook/assets/image (421).png>)

## Environment

By default, the forms authentication and policy assignment scripts run within the PowerShell Universal process. You can optionally configure an external [Environment ](../environments.md)to run your authentication and authorization scripts. When you configure a security environment, an external PowerShell process will be started and configured use your Environment's settings.

To adjust the environment used by the security process, set the `-SecurityEnvironment` in `settings.ps1`.

```powershell
Set-PSUSetting -SecurityEnvironment '5.1'
```

## Example: Forms Authentication with Active Directory

The following example shows performing a simple "LDAP BIND" in order to validate a users Active Directory Credentials. If a user attempting to access PowerShell Universal is not the Default Admin User they will have to successfully authenticate their credentials with Active Directory via a simple LDAP bind. This can be combined with a AD Group Member check in the Admin, Operator, and Reader role policies to effectively use Active Directory Authentication AND Active Directory Group membership to provide Role Based Access to PowerShell Universal.

```powershell
param(
    [PSCredential]$Credential
)

#
#   You can call whatever cmdlets you like to conduct authentication here.
#   Just make sure to return the $Result with the Success property set to $true
#

$Result = [Security.AuthenticationResult]::new()
if ($Credential.UserName -eq 'Admin') 
{
    #Maintain the out of box admin user
    $Result.UserName = 'Default Admin'
    $Result.Success = $true 
}
else
{
    # Get current domain using logged-on user's credentials - this validates their credential
    $CurrentDomain = "LDAP://DC=mydemodomain,DC=com"  # Insert Your Domain Here
    $domain = New-Object System.DirectoryServices.DirectoryEntry($CurrentDomain,($Credential.UserName),$Credential.GetNetworkCredential().password)

    if ($domain.name -eq $null)
    {
        "Authentication failed for $($Credential.UserName)!" | Out-File "C:\test\adlogin.txt"
        write-host "Authentication failed - please verify your username and password."
        $Result.UserName = ($Credential.UserName)
        $Result.Success = $false 
    }
    else
    {
        write-host "Successfully authenticated with domain $($domain.name)"
        "Authentication success for $($Credential.UserName)!" | Out-File "C:\test\adlogin.txt"
        $Result.UserName = ($Credential.UserName)
        $Result.Success = $true 
    }
}

$Result
```

## Example: Policy based on Active Directory Group Membership (Windows Authentication)

{% hint style="info" %}
This example requires an authentication method that will provide group information during the authentication process. Methods like Windows authentication and WS-Federation can provide this information. Forms authentication will not work with this type of policy.
{% endhint %}

This example takes advantage of the claims that are provided during authentication. You can check to see if the user has a groupsid (group membership) by using claim mappings. Map the groupid claim type to the value you want to assign the role to.

```powershell
$Parameters = @{
    Name = "Administrators"
    ClaimType = 'http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'
    ClaimValue = 'S-1-5-21-22222222-111111-3333333-153'
}

New-PSURole @Parameters
```

## Example: Policy based on Active Directory Group Membership

In this example we will configure out Administrator Policy Script to use LDAP to retrieve the membership of an Active Directory Group. Here we have created a group called "PowerShell Universal Admins" where members of the group should be granted Administrator Access in PowerShell Universal. Here we are doing a simple samaccountname check for the user to ensure they are a member of the group. For more robust environments a SID/DN/ObjectGUID check would be more appropriate.

![](<../../.gitbook/assets/image (161).png>)

```powershell
param(
$User
)

$UserName = ($User.Identity.Name)
$UserName = $UserName.Substring($UserName.IndexOf('\')+1,($UserName.Length -($UserName.IndexOf('\')+1)))

$IsMember = $false;

# Perform LDAP Group Member Lookup
$Searcher = New-Object DirectoryServices.DirectorySearcher
$Searcher.SearchRoot = 'LDAP://CN=Users,DC=berg,DC=com' # INSERT ROOT LDAP HERE
$Searcher.Filter = "(&(objectCategory=person)(memberOf=CN=PowerShell Universal Admins,OU=Information Technology,DC=berg,DC=com))" #GROUP INSERT DN TO CHECK HERE
$Users = $Searcher.FindAll()
$Users | ForEach-Object{
    If($_.Properties.samaccountname -eq $UserName)
    {
        $IsMember = $true;
        "$UserName is a member of admin group!" | Out-File "C:\test\adgroup.txt"
    }
    else {
        "$UserName is NOT member of admin group!" | Out-File "C:\test\adgroup.txt"
    }
}

return $IsMember
```

## Example: Group membership based on Azure Active Directory

This example takes advantage of [OpenID Connect and Azure Active Directory](../../security/enterprise-security/openid-connect.md#configuring-azuread).

Once you have configured PowerShell Universal and Azure Active Directory, you can configure role scripts to verify whether users are members of groups found in Azure AD. You can take advantage of claims mappings to map from the Azure AD Group ID to a PowerShell Universal role.

First, ensure that you have group membership claims enabled in the manifest for your application registration. This will include all group membership, so it is accessible in PowerShell Universal.

![](<../../.gitbook/assets/image (99).png>)

```
"groupMembershipClaims": "All",
```

Once configured, you can update your role script to check for a group membership. First make note of the object ID of the group you are looking to check within Azure AD.

![](<../../.gitbook/assets/image (49).png>)

Next, within your `roles.ps1` script, you can validate a user has a particular role by using claims mapping.

```powershell
New-PSURole -Name 'Administrators' -ClaimType 'groups' -ClaimValue '4acabc67-56cc-4590-9de6-164f3c4faf10'
```

As users login, their group membership will be validated against their claims and a role will be assigned.
