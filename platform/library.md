---
description: The PowerShell Universal Gallery of scripts, widgets, triggers and more.
---

# Gallery

The Gallery is a collection of pre-built solutions for PowerShell Universal. It includes resources such as scripts, triggers, apps, and widgets.

The Gallery[ ](https://github.com/ironmansoftware/scripts/issues)is open-source, and each release of PowerShell Universal includes a copy that is downloaded during the build process. The library is registered as a PowerShell Module Repository in PowerShell Universal. It consists of a folder of `.nupkg` files for each module.&#x20;

## Registering the Gallery as a Resource Repository

The Gallery provides a NuGet feed that you can register with PowerShell Resource Get. This feed is automatically registered in PowerShell Universal. Once register, you can use commands like `Save-PSResource`to download and install modules for PowerShell Universal

```powershell
Install-Module -Name Microsoft.PowerShell.PSResourceGet -Force -SkipPublisherCheck -AllowClobber -Scope CurrentUser -ErrorAction SilentlyContinue
Register-PSRepository -Name 'PSUGallery' -SourceLocation 'https://gallery.powershelluniversal.com/feed/index.json'
```

## Installing the Gallery Locally

Navigate to the Gallery page on Platform \ Gallery and click Install Gallery to install the latest version of the library. It will download a ZIP file from GitHub, extract it locally and register the folder as a PowerShell Module Repository.

## Installing Resources from the Gallery

### Module Page

On the module page, click Galleries, and then select the PSUGallery to search and download resources directly from the PowerShell Universal Gallery feed.



### Gallery Page

The Gallery page uses the locally installed version of the Gallery to allow you to install resources into your current configuration.&#x20;

To install resources from the library, click Platform \ Gallery in the Universal Admin Console. Click the Install icon to save the resource into your environment. Each solution is a PowerShell module that will be included in your Repository's module directory.&#x20;

<figure><img src="../.gitbook/assets/image (65).png" alt=""><figcaption><p>Modules in the Gallery</p></figcaption></figure>

Resources installed from modules, like from the Gallery, are marked as read-only in the Admin Console.&#x20;

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Read-Only Resource in the Admin Console</p></figcaption></figure>

## Uninstalling Resources

Once a resource is installed, you cannot remove it unless you remove the module itself. You can navigate to the Platform \ Modules page and click the Repository Modules tab to view modules installed from the library.&#x20;

Clicking the Delete icon will remove the module and any resources associated with it.&#x20;

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption><p>Repository Modules</p></figcaption></figure>

## Contributing to the Gallery

You can contribute your own scripts to the PowerShell Universal Gallery. We accept pull requests on the[ Gallery GitHub repository](https://github.com/ironmansoftware/scripts).&#x20;
