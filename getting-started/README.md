---
description: Installation instructions for PowerShell Universal.
---

# ⬇️ Installation

## MSI Install (Windows)

The MSI install will create a PowerShell Universal service. By default, PowerShell Universal will be listening on port 5000. You will be able to navigate to `http://localhost:5000`

MSI downloads are available on our [download page](https://ironmansoftware.com/downloads).

System installs will run as a Windows service. User installs will run when the user logins into the machine and runs in the user's context.

### MSI Parameters

The following table contains the parameters you can specify if running `msiexec` against our MSI install for automation purposes.

| Parameter              | Description                                                                                                                  | Default Value                                             |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| INSTALLFOLDER          | The installation folder for PowerShell Universal                                                                             | %ProgramFiles(x86)%\Universal                             |
| TCPPORT                | The TCP port the HTTP server will be listening on.                                                                           | 5000                                                      |
| REPOFOLDER             | The repository folder to save the configuration files to.                                                                    | %ProgramData%\UniversalAutomation\Repository              |
| CONNECTIONSTRING       | The SQL, SQLite, or PostgreSQL connection string.                                                                            | Data Source=%ProgramData%\UniversalAutomation\database.db |
| DATABASETYPE           | SQL, SQLite, or PostgreSQL                                                                                                   | SQLite                                                    |
| STARTSERVICE           | Whether to start the service after install (0 or 1)                                                                          | 1                                                         |
| SERVICEACCOUNT         | The service account to set for the Windows service. Use the format of domain\username.                                       | None                                                      |
| SERVICEACCOUNTPASSWORD | The service account password to set for the Windows Service. The password will be masked with \*\*\*'s in the installer log. | None                                                      |
| TELEMETRY              | Anonymous telemetry collection                                                                                               | 0                                                         |
| ADDPSMODULEPATH        | Adds the PowerShell Universal module directory to the PSModulePath environment variable.                                     | 1                                                         |
| STARTSERVICE           | Whether to start the service after install.                                                                                  | 1                                                         |
| INSTALLTYPE            | Whether to perform a server or user install.                                                                                 | Server                                                    |

### Example

Below is an example of how to run `msiexec.exe` to install PowerShell Universal and provide parameters to the installer.

{% code overflow="wrap" %}
```powershell
 Start-Process msiexec.exe -ArgumentList "/I C:\Users\adamr\Downloads\PowerShellUniversal.4.2.7.msi /q /norestart /L*V `"C:\users\adamr\desktop\msi.log.txt`" STARTSERVICE=0 SERVICEACCOUNT=contoso\service_account SERVICEACCOUNTPASSWORD=ThisPasswordWillBeReplacedWithAsterisksInTheMSILogs" -Wait -NoNewWindow
```
{% endcode %}

## ZIP Install

You can also download the ZIP from our [Downloads page](https://ironmansoftware.com/downloads/) if you would like to xcopy deploy the files on Windows or Linux.

### Windows

You can start Universal by unzipping the contents, unblocking the files and then executing `Universal.Server.exe`.

```
Expand-Archive -Path .\Universal.zip -DestinationPath .\Universal
Get-ChildItem .\Universal -Recurse | Unblock-File
Start-Process .\Universal\Universal.Server.exe
```

### Linux

You can use the following command line on Linux to install and start PowerShell Universal.

```
 wget https://imsreleases.blob.core.windows.net/universal/production/4.2.7/Universal.linux-x64.4.2.7.zip
 sudo apt install unzip 
 unzip Universal.linux-x64.4.2.7.zip -d PSU
 chmod +x ./PSU/Universal.Server
 ./PSU/Universal.Server
```

## PowerShell Module

You can use the PowerShell Universal PowerShell module to install the Universal server. To install the module, use `Install-Module`.

```
Install-Module Universal
```

To install the Universal server, you can use `Install-PSUServer`.

```
Install-PSUServer -LatestVersion
```

If you run this command on Windows, a Windows service will be created and started on your machine. If you run this command on Linux, a systemd service will be created and started. If you run this command on Mac OS, the PowerShell Universal server will be downloaded and extracted.

## Chocolatey Package (Windows)

{% hint style="warning" %}
Chocolatey packages for PowerShell Universal are usually available within a week of release but will not be available the day of a release.
{% endhint %}

You can install PowerShell Universal using the [Chocolatey package](https://chocolatey.org/packages/powershelluniversal). The package runs the MSI install. It will install Universal as a service and open a web browser after the install.

You can login with the "admin" user and any password.

```
choco install powershelluniversal
```

### Docker

See the [Docker page](docker.md#installation).

## IIS Install

Please visit the [IIS hosting documentation](../config/hosting/hosting-iis.md) for information on how to configure PowerShell Universal as an IIS website.

## Antivirus Configuration

PowerShell Universal takes full advantage of PowerShell and the PowerShell SDK. It includes PowerShell scripts directly in the product. You will want to consider configuring antivirus to allow for execution of PowerShell scripts in PowerShell Universal.

### Directories

The following directories will contain scripts and executable files that may need to be excluded from antivirus checks.

The following are examples from a standard Windows system. Changing paths within appsettings.json or within the installer will require changing which directories are execluded.

| Path                              | Description                                                                                                  |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| %ProgramData%\PowerShellUniversal | Contains log files and appsettings.json                                                                      |
| %ProgramData%\UniversalAutomation | Contains PowerShell scripts and artifacts. Contains the single file database when not using SQL integration. |
| %ProgramFiles(x86)\Universal      | Contains PowerShell Universal application executables, libraries and modules.                                |

### Executables

It may be necessary to exclude certain executables that will run PowerShell scripts. The below is a list of executables that will run PowerShell from PowerShell Universal.

| Name                 | Description                                            |
| -------------------- | ------------------------------------------------------ |
| Universal.Server.exe | The PowerShell Universal core service.                 |
| Universal.Agent.exe  | The PowerShell Universal agent environment executable. |
| pwsh.exe             | PowerShell 7.x                                         |
| PowerShell.exe       | PowerShell 5.x                                         |

## Default Admin Name and Password

By default, the administrator username is `admin` and password is `admin`. You can use the `$ENV:PSUDefaultAdminName` and `$ENV:PSUDefaultAdminPassword` environment variables to change this behavior. These values are only used if no administrator account already exists. This is useful for cloud-based installations.&#x20;

## Next Steps

At this point, Universal is up and running. You can navigate to the admin console by visiting `http://localhost:5000` by default. Login with the default admin name and password.&#x20;
