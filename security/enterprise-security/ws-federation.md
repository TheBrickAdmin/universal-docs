# WS-Federation

{% hint style="info" %}
WS-Federation requires a [license](https://ironmansoftware.com/pricing/powershell-universal).
{% endhint %}

WS-Federation supports both Active Directory Federation Services and Azure Active Directory.

You first need to configure ADFS or AzureAD to support Universal.

## Configuring ADFS for Universal <a href="#configuring-adfs-for-universal-dashboard" id="configuring-adfs-for-universal-dashboard"></a>

### Service Settings <a href="#service-settings" id="service-settings"></a>

These are the current Federation Service settings for our domain.

![](https://gblobscdn.gitbook.com/assets%2F-L9mVQO4zbOX7ZcHvIte%2F-Lob6ow15SQRLl3vo8ZV%2F-Lob7luBvuEGUTrLIors%2Fimage.png?alt=media\&token=64c3c00f-1d2c-4346-bcc1-dd89e7cf4c24)

### Relying Parties <a href="#relying-parties" id="relying-parties"></a>

You need to configure the following Relying Parties settings for Universal. On the Identifiers tab, provide the URL to the Universal website. HTTPS is required.

![](https://gblobscdn.gitbook.com/assets%2F-L9mVQO4zbOX7ZcHvIte%2F-Lob6ow15SQRLl3vo8ZV%2F-Lob8DOuN3sGBzbdctQb%2Fimage.png?alt=media\&token=8b2fac2f-c5e1-4ceb-963e-ab9c7eb85484)

On the Endpoints tab. You'll need to include a WS-Federation Passive Endpoint. Make sure to include the trailing slash.

![](https://gblobscdn.gitbook.com/assets%2F-L9mVQO4zbOX7ZcHvIte%2F-Lob6ow15SQRLl3vo8ZV%2F-Lob8hIg0Ot1uN1PsimG%2Fimage.png?alt=media\&token=e6673c7c-a125-4b04-b0c0-ba7d0a677d6a)

Finally, you'll need to configure a Claim Issuance Policy for the Relying Party Trust. Create an Issuance Transform Rule that sends at least the Name and Name ID to Universal.![](https://gblobscdn.gitbook.com/assets%2F-L9mVQO4zbOX7ZcHvIte%2F-Lob6ow15SQRLl3vo8ZV%2F-Lob92zcF4qYpWtR0g\_4%2Fimage.png?alt=media\&token=34dfd4db-d742-4f8b-a271-86d37542dc35)

You can configure additional claims you'd like to use if you are using policies in Universal.

## Configuring For Azure Active Directory <a href="#configuring-for-azure-active-directory" id="configuring-for-azure-active-directory"></a>

Follow the documentation for the Azure Active Directory configuration found on this [Microsoft Document](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/ws-federation?view=aspnetcore-2.2#azure-active-directory).

## Configuring Universal <a href="#configuring-universal-dashboard" id="configuring-universal-dashboard"></a>

### Use Appsettings.json

After configuring ADFS or AAD, you can now provide the properties to Universal for the MetadataAddress and Wtrealm. Read about these settings on the our [Settings ](../../config/settings.md)page.

Here is an example of how to update the `appsettings.json` file to accommodate the correct settings for WS-Federation.

```javascript
{
  "Kestrel": {
    "Endpoints": {
      "HTTP": {
        "Url": "http://*:5000"
      }
    },
    "RedirectToHttps": "false"
  },
  "ApplicationInsights": {
    "InstrumentationKey": ""
  },
  "Logging": {
    "Path": "%PROGRAMDATA%/PowerShellUniversal/log.txt",
    "RetainedFileCountLimit": 31,
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*",
  "CorsHosts": "",
  "Data": {
    "RepositoryPath": "%ProgramData%\\UniversalAutomation\\Repository",
    "ConnectionString": "%ProgramData%\\UniversalAutomation\\database.db",
    "GitRemote": "",
    "GitUserName": "",
    "GitPassword": "", 
    "ConfigurationScript": ""
  },
  "Api": {
    "Url": ""
  },
  "Authentication" : {
    "Windows": {
      "Enabled": "false"
    },
    "WSFed": {
        "Enabled": "true",
        "MetadataAddress": "https://ironman.local:443/FederationMetadata/2007-06/FederationMetadata.xml",
        "Wtrealm": "https://ironman.local:12345",
        "CallbackPath": "/auth/signin-wsfed"
    },
    "OIDC": {
      "Enabled": "false",
      "CallbackPath": "/auth/signin-oidc",
      "ClientID": "",
      "ClientSecret": "",
      "Resource": "",
      "Authority": "",
      "ResponseType": "",
      "SaveTokens": "false"
    },
    "SessionTimeout": "25"
  },
  "Jwt": {  
    "SigningKey": "PleaseUseYourOwnSigningKeyHere",  
    "Issuer": "IronmanSoftware",
    "Audience": "PowerShellUniversal"
  },
  "UniversalDashboard": {
    "AssetsFolder": "%ProgramData%\\PowerShellUniversal\\Dashboard"
  },
  "ShowDevTools": false,
  "HideAdminConsole": false
}
```

When running your server, you should now be prompted for your credentials either via the Internet Explorer single-sign system or you will be forwarded to the WS-Fed login page.

![](https://gblobscdn.gitbook.com/assets%2F-L9mVQO4zbOX7ZcHvIte%2F-Lob6ow15SQRLl3vo8ZV%2F-Lob9yeDdGENbUiyz4Sj%2Fimage.png?alt=media\&token=910db2dd-85f3-46eb-b3ec-9f551f244439)

### Use Authentication.ps1

You can configure WS-Federation authentication in the admin console. To do so, navigate to Security \ Authentication. Add the WS-Federation provider by selecting it from the drop down in the top right.

![](<../../.gitbook/assets/image (462).png>)

Next, edit the properties of the authentication provider and specify the configuration details for your ADFS setup.

![](<../../.gitbook/assets/image (232).png>)

Once configured, enable the WS-Federation provider. Then, log out and navigate to `/admin` You will be prompted to login to your WS-Federation provider.
