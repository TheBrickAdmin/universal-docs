---
description: Changelog for PowerShell Universal.
---

# Changelog

## 5.1.1 - 12/16/2024

### Bug Fixes

#### Admin Console

* Fixed an issue with the log view not loading properly (#4129)
* Fixed an issue with Code Editor crashing (#4131)
* Fixed an issue with inconsistent and slow IntelliSense

#### Apps

* Fixed an issue with New-UDEndpoint -Schedule (#4130)

#### Agents

* Fixed an issue with agent reconnection (#4126)

#### Cmdlets

* Fixed an issue with Get-PSUCache -List (#4124)

#### Platform

* Fixed an issue groom idle terminal instances (#4138)

## 5.1.0 - 12/11/2024

### Features

#### Admin Console

* Added -DarkAppBarLogo to New-PSUBranding (#3170)
* Added .gitignore editor to git page (#3560)
* Added configuration file reload dropdown (#3812)
* Table page sizes are now sticky (#4023)
* Added Nested Role field to roles property form.
* Added Import Secrets button (#4080)
* Added localized date time display to admin console (#3780)

#### APIs

* Added contact, license, version and terms of service to OpenAPI endpoint docs (#3900)
* Added -ResponseVariable to Invoke-PSUEndpoint (#4054)

#### Apps

* Added -JavaScript to New-UDEndpoint (#3121)
* Added support for enter key in prompts in apps (#3326)
* Added support for Read-Host -AsSecureString
* Added support for nested modals (#3163)
* Added Columns and SelectedRowIds to $EventData in -LoadData for New-UDTable (#3982)

#### Automation

* Child jobs not display parent schedule (#3373)
* Added support for copy\paste in terminals (#4096)

#### Platform

* Added -DefaultTokenLifetime to Set-PSUSettings and admin console (#2872)
* Added preview support for Deployments (#4101)

#### Portal

* Added support for SimpleSelect in Widgets

#### Tools

* Added psu.exe git command line tools
* Added psu.exe db migration command line tools

### Bug Fixes

#### Admin Console

* Fixed an issue with the tab URLs updating incorrectly (#4062)
* Fixed an issue when creating secrets (#4074)
* Fixed a display issue with online licenses (#4068)
* Fixed an issue logging in with SAML2 from the login page (#4075)
* Fixed an issue displaying string arrays in schedule parameters (#4084)
* Fixed an issue with the copy button in the API tester in non-secure sites (#4108)

#### Apps

* Fixed an issue with the unauthorized page (#4065)
* Fixed an issue where the docs app would not load (#4067)
* Fixed an issue with favicons (#4055)
* Fixed an issue with dynamically registered components and Add-UDElement (#3585)
* Fixed an issue with Invoke-UDRedirect -OpenInNewWindow (#4104)

#### APIs

* Fixed an issue with the endpoint table (#4066)
* Fixed an issue with the endpoint tester and non-JSON values (#4098)

#### Cmdlets

* Fixed an issue where Get-PSUCache threw an exception rather than returning $null when the key was not found (#4078)

#### Automation

* Fixed an issue with Write-PSFramework and job logs (#4071)

#### Platform

* Fixed an issue loading the OpenTelemetry plugin in Docker (#4081)
* Fixed an issue where user sessions were groomed before the data retention setting (#4089)
* Fixed an issue with user scoped logging (#4103)
* Fixed an issue with the configuration file system watcher

#### Portal

* Fixed an issue with script table display (#4083)

## 5.0.16 - 11/18/2024

#### Admin Console

* Renamed Functions tab in app editor to Module
* Fixed an issue creating a CRON schedule on the script page (#4035)
* Fixed an issue with the Run button on the health checks page (#4024)
* Added Start Time, Created Time and Elapsed Time to the jobs table (#4028)
* Fixed an issue where form authentication was shown on the login page when disabled (#4041)
* Added reset branding button (#4030)
* Fixed an issue where non-persistent cache data had missing time stamps
* Added Available in Branch to UI
* Improved memory usage output on process page (#3828)
* Fixed an issue with the failed login configuration page
* Added variable value form (#4049)
* Fixed an issue where System would show in the permission identity selector (#4040)
* Fixed an issue where Reader role could execute scripts in admin console (#4061)
* Fixed an issue where the admin console could exhaust SQL connections (#4056)

#### APIs

* Added -Computer to Send-PSUEvent (Invoke-PSUCommand)
* Fixed an issue where variables were retained between invocations in a persistent runspace
* Fixed an issue executing endpoints that use -Module-Command (#4053)

#### Apps

* Added -Checkbox to New-UDSelect (#1996)
* Fixed an issue with New-UDAutocomplete -Multiple layout (#3756)
* Added -SortType numeric to New-UDTableColumn (#3837)
* Fixed an issue with -Locale in New-UDDatePicker (#4017)
* Fixed margin of UDTextbox with an icon (#4051)

#### Automation

* Fixed an issue scheduling the groom job past 59 minute intervals (#4031)
* Fixed an issue renaming schedules (#4034)

#### Module

* Fixed an issue where module help was not updating properly.
* Fixed an issue with Get-PSUCache -Key case sensitivity (#4043)

#### Portal

* Links now open in a new tab (#4027)
* Added disabled and roles settings for portal (#4037)
* Fixed an issue with portal authentication redirects (#4010)

#### Platform

* Fixed an issue grooming jobs when using PostgreSQL (#4032)
* System log level can now be set in the admin console

## 5.0.15 - 11/5/2024

#### Admin Console

* Fixed an issue with tagged variables (#4002)
* Added Telemetry option to General Settings
* Added Report Bug button to error alert
* Added tooltip help to maintenance mode and readonly mode for computers (#3996)
* Added Help menu
* Documentation links now open in a new tab
* Fixed an issue with job execution time (#4016)
* Reduced Blazor log level
* Fixed an issue when jobs had multiple requests for feedback

#### APIs

* Fixed an issue where event hub agent would not log Write-\* calls (#3983)
* Added -RemoteDomainName and -RemoteUserName to Get-PSUEventHubConnection (#4009)
* Fixed an issue where event hubs would only run one command at a time (#3988)

#### Apps

* Fixed an issue with button colors (#3974)
* Fixed an issue where apps would auto-deploy when secrets were changed (#4022)

#### Automation

* Fixed an issue grooming jobs (#3929)

#### Cmdlets

* Added -Integrated mode for cmdlets
* Added -Silent to Invoke-PSUScript and Wait-PSUJob
* Fixed an issue with switch parameters and Invoke-PSUScript (#4011)

#### Platform

* Fixed an issue where the service could crash when losing database connection (#4012)
* Fixed an issue where secrets could be deleted in multi-node environments during git sync (#3959)

## 5.0.14 - 10/31/2024

#### Admin Console

* Fixed an issue where a custom app bar logo would not display (#3945)
* Fixed an issue pressing the tab key in the debug console (#3939)
* Added support for deleting computer tags (#3961)
* Fixed an issue starting an app that was in a start failed state (#3965)
* Added Reload Configuration Files button to the Configuration Files page
* Fixed an issue where nodes in git merge conflict status would not display state properly (#3971)
* Added file editor to apps and modules pages
* Fixed an issue with additional info buttons on home page in nested sites (#3979)
* Fixed a display issue with endpoint methods (#3975)
* Fixed an issue editing scripts and view jobs when using Windows Auth and PostgreSQL (#3952)
* Added secret value form (#3954)
* Fixed an issue where readonly endpoint wouldn't expose the test or log tabs (#3947)
* Fixed an issue with job execution time when using PostgreSQL (#3915)
* Added Auto Deploy option to app settings (#3992)

#### Apps

* Fixed an issue where a button group drop down item would not be shown when used within a modal (#3765)

#### API

* Fixed an issue with requests timing out too early (#3946)
* Fixed an issue with an erroneous error message with integrated APIs (#3955)
* Fixed an issue where endpoint docs wouldn't include base path (#3958)
* Fixed an issue with endpoint logging (#3950)
* Fixed an issue where remote server would not be indicated in event hub connections (#3980)
* Added -Active, -Hub, -RemoteComputerName, -ServerComputerName to Get-PSUEventHubConnection (#3981)

#### Platform

* Fixed an issue with configuration files reloading unnecessarily during git operations (#3956)
* Fixed an issue where a module PSM1 file could be locked when updating the content (#3948)

#### Portal

* Fixed an issue loading resources on the portal when using Windows Authentication (#3962)

#### Cmdlets

* Fixed an issue with Invoke-PSUEndpoint (#3969)

## 5.0.13 - 10/22/2024

#### Admin Console

* Fixed an issue where dynamic parameter wouldn't update properly when other parameters changed (#3843)
* Added Permissions to role properties (#3931)
* Fixed an issue where the API tester incorrectly replaced route variables (#3937)
* Improved feedback of developer license usage
* Fixed an issue with app debugging

#### Apps

* Fixed an issue rendering static tabs (#3928)
* Fixed an issue where Show-UDModal would throw errors (#3930)
* Fixed an issue with favicons and a base URL of / (#3873)

#### Automation

* Fixed scheduling a script against a computer group with a space in the name (#3927)
* Fixed an issue when scheduling scripts with the same name but different paths
* Fixed an issue loading scripts in nested folders that had differing names and paths

#### Cmdlets

* Improved certificate error information

## 5.0.12 - 10/18/2024

#### Security

* [Critical security fix for admin console ](https://docs.powershelluniversal.com/changelogs/cves#cve-tbd-10-17-2024-privilege-escalation-and-information-disclosure)(CVE-2024-50616)

#### Admin Console

* Fixed an issue with the back button on several pages (#3876)
* Fixed an issue with the tree view in the scripts page
* Fixed an issue adding secret variables (#3918)
* Fixed an issue where the incorrect execution was displayed for running jobs (#3915)
* Fixed an issue where the git status of only the current node would be shown (#3921)
* Fixed an issue with the display of nested jobs

#### API

* Fixed an issue where the swagger UI would default to schema rather than example (#3913)

#### Apps

* Removed the need to call Invoke-UDEndpoint with -Session for it to work (#2139)
* Fixed an issue where the tooltip arrow did not match the background color (#3580)
* Fixed an issue where the icon property in the page properties wouldn't persist (#3924)

#### Automation

* Fixed an issue scheduling scripts against computer groups (#3768)
* Fixed an issue running triggers when on trigger was missing the trigger script

#### Platform

* Fixed an issue where the Conflicting Modules health check could show conflicting information (#2850)

#### Security

* Fixed an issue logging in with Windows Authentication in IIS (#3916)

## 5.0.11 - 10/15/2024

#### Admin Console

* Fixed an issue updating the logging target file path (#3882)
* Fixed an issue saving non-PowerShell files in the Settings \ File editor (#3886)
* Added copy button to variables page (#3870)
* Fixed an issue with date time zone conversion when using PostgreSQL
* Added missing Identity page.
* Fixed an issue with the Show Streams and Show Timestamp buttons (#3902)
* Added log tab to roles page (#3896)
* Fixed an issue modifying module content (#3849)
* Fixed vertical sizing of code editor (#3883)

#### APIs

* Fixed an issue where swagger docs could crash the browser (#3877)
* Fixed an issue when editing endpoint files on disk (#3869)
* Fixed an issue specifying arrays of custom types with OpenAPI docs (#3874)
* Fixed an issue with nested IIS site and Swagger UI (#3867)

#### Apps

* Fixed an issue with apps that have a space at the end of the name (#3891)

#### Automation

* Fixed an issue where Invoke-PSUScript could cause job status to be stuck in "Running" state. (#3845)
* Fixed an issue where DateTime parameters without a value were set to an empty string in Schedules (#3893)

#### Platform

* Fixed server startup performance when the database has a large number of jobs
* Fixed an issue with schedule configuration files updating when they should not be (#3885)
* Improved performance of configuration file database queries
* Improved performance of database updates
* Fixed an issue when renaming or deleting environments (#3897)
* Added $PSUGitBranch automatic variable (#3909)

## 5.0.10 - 10/7/2024

#### Admin Console

* Fixed an issue where job search was case-sensitive (#3835)
* Fixed horizontal scrollbar on claim information table (#3834)
* Fixed an issue filtering jobs by multiple statuses (#3833)
* Fixed date time display
* Fixed issue with endpoint tab (#3848)
* Fixed an issue with script documentation editor (#3862)
* Added reset settings button to My Identity page (#3863)
* Fixed issue saving endpoint docs and tags (#3866)

#### APIs

* Fixed an issue with OpenAPI docs default value for properties (#3430)
* Fixed an issue with endpoint path format (#3838)
* Fixed an issue calling endpoints with Windows authentication (#3858)
* Fixed an issue with the view docs button in a nested IIS site

#### Apps

* Fixed an issue with nested table rendering and overall table performance
* Fixed a performance issue when loading and saving apps (#3809)
* Fixed an issue with New-UDForm -Script (#3854)

#### Automation

* Fixed an issue with PSCredential parameters (#3864)

#### Cmdlets

* Fixed an issue where valid certificates could be rejected by the cmdlet transport layer
* Improved gRPC errors (#3830)

#### Platform

* Fixed an issue where saving schedules could cause changes to parameter positions resulting in unnecessary file changes (#3847)
* Fixed an issue with database logging
* Configuration file resources are now sorted deterministically (#3857)
* Windows PowerShell 5.1 now targets .NET 4.7.2 to support the PartnerCenter Module (#3855)

#### Portal

* Fixed an issue editing Portal widgets (#3839)

#### Security

* Fixed an issue signing out of WS-Federation (#3861)

## 5.0.9 - 9/30/2024

#### Admin Console

* Fixed an issue working with array variables (#3802)
* Fixed an issue with the job list in a script's page (#3816)
* Fixed a UI issue with Enhanced Token Security (#3783)
* Fixed tree view scrolling for files (#3819)
* Improved feedback of the start\stop button for apps.
* Fixed issue with schedule search (#3813)
* Fixed an issue displaying date and time in the proper client time zone (#3753)
* Fixed an issue where the console would flash when in light mode
* Fixed an issue with one-time schedule time zones
* Fixed an issue where the console would show the license paywall when reloading pages in certain configurations (#3725)
* Fixed an issue when navigating directly to the scripts page when using SQL server and Windows Authentication (#3731)

#### Automation

* Fixed an issue running a script manually on a target node (#3818)
* Fixed an issue with scheduling scripts against computer groups
* Fixed an issue running one-time schedules in multi-node environments (#3784)

#### APIs

* Fixed an issue with Send-PSUEvent not being implemented in certain invocation methods (#3810)

#### Apps

* Fixed an issue loading data in New-UDDataGrid
* Fixed an issue with -IdentityColumn in New-UDDataGrid (#3832)
* Fixed an issue with Nivo chart tool tips (#3718)
* Fixed an issue with labels for New-UDNivo -Chart Bubble (#3719)

#### Module

* Improved the error message when attempting to create a schedule for a script that doesn't exist (#3824)

#### Platform

* Fixed an issue where improperly formatted configuration scripts would cause issues with the platform (#3800)
* Fixed an issue with redirection after login in when using a Linux docker container
* Fixed an issue with log file configuration (#3261)

## 5.0.8 - 9/23/2024

#### Admin Console

* Improved scrolling of git commits table (#3741)
* Fixed an issue with the Discover Environments button in Two Way Automatic git sync mode
* Added the ability to clear the Run In, On and As dialogs for schedules (#3772)
* Fixed an issue where computer groups did not display the (Any)(All) specifier and would only run on 1 node in the group (#3770)
* Fixed an issue with JavaScript caching between versions of the admin console (#3774)
* Child jobs are now shown as a nested table in the jobs page (#3767)
* Fixed issue with file parameters for scripts (#3778)
* Fixed an issue where computer group tags were not populated in the computer group dialog (#3771)
* Fixed an issue filtering API endpoints by documentation (#3642)
* Endpoint search now adds the search to the query string (#3643)
* Added search to schedule page (#3785)
* Fixed an issue where clicking on a script name in a job details page wouldn't navigate to the script (#3787)
* Added the schedule buttons to the scripts page (#3785)
* Fixed an issue with display app token expiration date (#3782)
* Extended the width of the script filter for the jobs table (#3795)
* Fixed validation in the reset password dialog (#3793)
* Fixed an issue where IntelliSense in the editor would overwrite content past the autocomplete suggestion (#3758)
* Fixed an issue where you could not set the value of secrets in one-way git sync (#3792)
* Added error stack trace to error tab in jobs page (#3807)
* Added color indicator to filters in the jobs page (#3805)
* Fixed an issue clearing the script filter for the jobs table (#3806)

#### Apps

* Fixed an issue with New-UDFloatingActionButton not staying in the bottom right corner of the screen. (#3720)
* Fixed an issue with intermittent runspace errors on startup (#3777)
* Improved startup performance and memory usage.
* Fixed an issue with apps running under Windows PowerShell 5.1 (#3801)

#### Automation

* Fixed an issue where Wait-Debugger wouldn't trigger the debug console in the Integrated environment (#3803)

#### Module

* Fixed an issue receiving large data from the server (#3775)

#### Platform

* Fixed an issue checking for updates
* Fixed an issue pushing to git remote using SSH keys (#3729)
* Fixed issue pausing git sync when set through appsettings.json (#3766)
* Fixed an issue where PSU would start slowly when many modules were installed in the Repository directory (#3790)
* Fixed an issue where the Missing Environment health check would fail for Universal.Agent

#### Security

* Fixed an open redirect issue (#3763)
* Fixed an issue with Windows authentication and authorization in the admin console (#3776)

## 5.0.7 - 9/16/2024

{% hint style="info" %}
This update adds an index to the database. If your service fails to start in a timely manner during the install, you may need to run a [manual schema update](https://docs.powershelluniversal.com/config/persistence#manual-schema-install-update).&#x20;
{% endhint %}

#### Admin Console

* Fixed an issue where computer groups weren't displayed in the Run On dropdown.
* Fixed an issue where DateTime parameters would not display the time portion of the selector
* Fixed an issue creating nested folders (#3723)
* Fixed an issue displaying schedules that do not include a computer but have a custom environment (#3721)
* Library has been renamed to Gallery to align with other nomenclature
* Fixed an issue where users logging in with a non-admin user would redirect to /admin rather than /portal
* Fixed issue autocompleting paths in the editor (#3726)
* Added a Not Authorized page, rather than a blank page, when a user tries to access a page, they do not have access to (#3735)
* Added confirmation dialog when navigating away from a page with unsaved changes (#3737)
* Added missing Startup Script and Process Startup Script controls to the environment properties modal (#3748)
* Fixed an issue displaying the default value for scripts in the run and schedule dialog (#3752)
* Fixed an issue displaying string array types in the run and schedule dialog (#3751)
* Fixed an issue where PowerShell Universal could crash when loading large job output in the admin console (#3745)
* Improved performance of job output in the admin console

#### Agent

* Added global exception handling and logging for the agent process

#### APIs

* Added /api/v1/first-run endpoint to set first run settings (#3749)

#### Apps

* Fixed an issue where apps would not start when using pwsh.exe (#3732)
* Fixed an issue where Set-UDElement did not work with UDExpansionPanel (#3626)

#### Automation

* Fixed an issue with schedule parameters
* Added DateOnly and TimeOnly parameter selectors
* Fixed an issue where long-running jobs would spawn a new job after some time (#3709)
* Added indices to the JobOutput table to improve performance

#### Module

* Invoke-PSUScript -Name is now more flexible to relative paths (#3727)
* Fixed an issue with New-PSUSchedule when changing parameter names of the target script
* Fixed an issue with TrustCertificate in appsettings.json
* Fixed an issue where license cmdlets were not implemented properly (#3738)

#### Portal

* Fixed an issue setting nullable bool attributes in PSBlazor

#### Platform

* Fixed an issue with Windows PowerShell $Env:PSModulePath (#3728)
* Modules are now installed in a background job to avoid long delays in the UI

#### Security

* Fixed an issue where certain APIs would not work when SAML2 was enabled (#3739)

#### Tools

* Fixed an issue migrating terminal history with psudb.exe

## 5.0.6 - 9/9/2024

#### Admin Console

* Fixed an issue where the git pause switch wouldn't display the proper status when paused (#3688)
* Added delete computer button for offline computers (#3693)
* Improved display of charts on dashboard
* Fixed property label text wrapping in modal dialogs in the admin console (#3703)
* Improved admin portal page header formatting (#3704)
* Fixed an issue where the Discover Environments button would be enabled when git sync wasn't editing (#3714)
* Fixed an issue where editing git settings when using SQL would cause it to delete the settings (#3688)
* Fixed a display issue with the job feedback modal (#3692)
* Fixed the performance of the schedules page (#3659)
* Added missing Diagnostics button

#### APIs

* Fixed an issue calling Invoke-PSUScript in an unauthenticated API using the permissive security policy

#### Automation

* Fixed a SQLite locking issue when running many jobs at once
* Fixed an issue calling Invoke-PSUScript -Name with a path (#3708)

#### Apps

* Fixed an issue calling OnRender in OnRowExpand in New-UDTable (#3695)
* Fixed an issue calling Invoke-PSUScript in apps using the permissive security policy
* Fixed an issue with a base URL of / and the admin console routing (#3711)
* Fixed an issue where duplicate apps could show up after server restart (#3706)
* General fixes to Nivo Charts (#3682, #2083)

#### Platform

* Fixed an issue with redirect URL after login (#3694)
* Added support for configuring -TrustCertificate flag at the server level (#3697)
* Fixed an issue where variables stored in database would be null after restart (#3701)
* Removed 4MB limit for Universal cmdlet calls (#3700)
* Fixed an issue with psudb.exe where it could throw an exception attempting to move job output
* Fixed an issue with background job scheduling when using multi-node configurations
* Added -Migrate to psudb.exe to enabling migration to the newest schema in SQLite
* Fixed an issue where upgrades from v4 with New-PSUEnvironment -EnableDebugger would cause the environment to not show up (#3712)
* Fixed an issue where git sync would push even when there were no changes (#3713)

#### Portal

* Fixed an issue where an error could be displayed on the portal for certain authentication types

#### Security

* Updated .NET (8.0.8) and PowerShell (7.4.5) SDK versions to address security issues in dependencies
* Added missing View Claim Information button
* Fixed an issue enabling Anonymous Authentication and Windows Authentication in IIS (#3715)

## 5.0.5 - 9/3/2024

#### APIs

* Fixed an issue where endpoint content wouldn't be displayed in the admin console when using -Path
* Fixed an issue changing the path of endpoints (#3685)

#### Apps

* Fixed an issue where -DisableAutoStart had no affect (#3658)
* Fixed issue with -OnClick and -Theme on New-UDNivoChart (#3667)
* Added icon selector to page properties (#3678)
* Fixed an issue where a blank file and folder would be created if the app pointed to a path that didn't exist (#3679)
* Fixed an issue where installing apps from the library wouldn't show up (#3686)

#### Automation

* Added missing trigger schedule button

#### Platform

* Fixed an issue where the computer page would incorrectly show an error about the number of licensed computers.
* Fixed an issue where changing branding would duplicate lines in branding.ps1 (#3666)
* Fixed an issue with psudb.exe failing to convert jobs in certain configurations (#3670)
* Added permissive security model to cmdlets (#3676)
* Display number of selected changes in git commit (#3645)
* Fixed an issue where secret variable values would not be deleted from the database (#3654)
* Added missing clear cached claims button on the roles page.
* Fixed an issue running Connect-AzAccount

## 5.0.4 - 8/28/2024

#### Platform

* Fixed an issue loading custom health checks

#### Security

* ❗Fixed an issue where First Run dialog could be called multiple times

## 5.0.3 - 8/28/2024

#### Automation

* Fixed an issue displaying the scripts page

#### Apps

* Fixed a license expiration issue with data grids and date\time pickers

## 5.0.2 - 8/27/2024

#### Apps

* Fixed an issue with displaying non-authenticated apps (#3641)

#### Automation

* Fixed an issue with scheduling on Windows Server 2016 (#3647)
* Fixed an issue with script folder display (#3639, #3660)
* Fixed an issue where the Time Zone selector was missing for schedules (#3649)
* Added missing list view for scripts (#3635)
* Fixed an issue where the inline debugger wouldn't trigger in the integrated environment (#3622)
* Fixed an issue where schedule parameters would not display

#### Portal

* Fixed issue with parameter sets on the portal (#3617)

#### Platform

* Fixed issue with license enforcement in the admin console
* Fixed an issue where the agent would give up retrying to connect after some time (#3614)
* Fixed an issue with psudb.exe throwing an exception then migrating app tokens and jobs (#3652)
* Improved performance of psudb.exe
* Fixed an issue where disabling Windows Auth would cause users to fail to login until cookies were deleted (#3648)
* Fixed an issue where the platform would attempt to use powershell.exe, if configured
* Fixed an issue changing Script Base Path when API Base Path was already set (#3651)
* Fixed a locking issue with SQLite databases
* Fixed issue copying app tokens on HTTP servers (#3655)
* Improved connection logic in cmdlets to support more server configurations
* Fixed an issue with the online license activation retry failing

## 5.0.1 - 8/23/2024

#### APIs

* Fixed an issue when viewing endpoint documentation (#3607)
* Added event hub connections table
* Fixed an issue with Send-PSUEvent and -Parameters
* Fixed an issue with the endpoint tester and nested IIS sites (#3611)
* Added endpoint search (#3631)

#### Apps

* Fixed issues with the Pages tab in the app editor
* Live docs menu item now opens in a new tab
* Fixed an issue with New-UDTransferList (#3634)
* Fixed an issue with a New-UDDataGrid example

#### Automation

* Fixed an issue with job output appearing out of order

#### Platform

* Reduced the default System Log Level from Debug to Information
* Fixed an issue with environment discovery and Windows PowerShell 5.1
* Improved error reporting when jobs fail to start
* Fixed an issue with the configuration file editor (#3603)
* Fixed an issue with Universal cmdlets and HTTP
* Fixed an issue with SQLite v4 to v5 database update
* Fixed an issue logging in with SAML2 (#3623)
* Fixed an issue with home page formatting (#3620)
* Added select all button to the git commit page (#3584)
* Fixed a theme issue with the permission drawer (#3618)

## 5.0.0 - 8/20/2024

{% hint style="warning" %}
Please review the [upgrade documentation](getting-started/upgrading.md) before updating your environment.
{% endhint %}

#### Breaking Changes

* Removal of Pages
* Removal of Desktop mode
* Removal of App Designer
* Removal of LiteDB support

#### APIs

* Added endpoint tester
* Added support for string array query string parameters
* Added -ApiBaseFolder to Set-PSUSettings

#### Automation

* Added support for selecting streams when running a script manually
* Added Hide Run Later
* Added Quick Run button
* Added in-line debugger
* Extended job filtering to include all columns
* Added support for dynamic parameters
* Added -Tags to Invoke-PSUScript
* Added -HideChildren, -HideTriggered, -HideScheduled to Get-PSUJob&#x20;
* Added a page to view jobs for a schedule&#x20;
* JobRunId has been promoted from an experimental to a full feature
* Added support for script documentation

#### Apps

* Added -Path to Start-UDDownload
* Added -Sx to New-UDBadge&#x20;
* Added -RemoveMargin to New-UDCard
* Added -SelectedTabIndex to New-UDTabs
* Added -Enhanced to New-UDTransferList
* Added -DisableArcLinkLabels and -DisableArcLabels to New-UDNivoChart
* Added module support for apps

#### Portal

* Added preview version of Portal
* Added Portal Page designer
* Added Portal Widget editor&#x20;

#### Platform

* Added New Blazor-based Admin Console
* Updated to .NET 8.0 and PowerShell 7.4
* Added opt-in telemetry collection
* Get-PSUCache now returns the entire cache item information
* Added custom PSScriptAnalyzer rule to check for built in variable usage.
* Added a first run setup to set the default admin user name and password
* Added support for role-based access with PSUCache
* Added password complexity and expiration enforcement
* Added support for module variables
* Added Password and KeySize to appsettings.json to configure AES 256 database secrets
* Added stack traces to notifications
* Added support for Emoji favicons
* Added support for discovering Python environments
* Added PSUDefaultAdminPassword and PSUDefaultAdminName environment variables
* Added granular permissions throughout the platform
* Implemented gRPC cmdlets across the platform
* Added a process that checks for module updates
* Added a health check that verifies an environment exists
* Added tags for variables
* Added PostgreSQL Support
* Added Script Library
* Added an option for updating git submodules during a pull
* Added Pause Git Sync button
* Added configuration settings for the loading page
* Added a page to view all tagged resources&#x20;
* Added support for uploading to Published Folders
* Added support for database cache

