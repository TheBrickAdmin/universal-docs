---
description: Git integration for PowerShell Universal.
---

# Git

{% hint style="info" %}
Git integration requires a [license](https://ironmansoftware.com/pricing/powershell-universal).
{% endhint %}

PowerShell Universal is capable of synchronizing the configuration scripts with a remote git repository.

## Configuration

### Admin Console&#x20;

Git sync can be configured in the database by adjusting the settings within the admin console. This is the preferred approach. The benefit is that when you connect new instances of PowerShell Universal to your SQL instance, you will not need to configure git sync again.

To configure git sync, navigate to Settings \ Git within the Admin Console. You will be able to click the Git Settings button.

<figure><img src="../.gitbook/assets/image (199).png" alt=""><figcaption></figcaption></figure>

### appsettings.json

You can also use the [Configuration settings](settings.md) to setup git sync. This is useful if you have a single instance of PSU and would like to back up your appsettings.json file. Adjusting settings within the admin console will not update the appsettings.json file. You will need to do so manually and restart PowerShell Universal after changing the settings.

If git sync settings are specified in the database, settings defined in appsettings.json are ignored.

## Git Sync Settings

### Branch

By default, PowerShell Universal will sync with the `master` branch. If you wish to use a different branch, specify the `GitBranch` setting within your `appsettings.json`.

### Remote

Remotes are not required. If a remote is not specified, the git repository is stored locally in the Repository directory. If specified, PowerShell Universal will sync with the remote. Proper credentials are required for access to that remote.&#x20;

### Authentication

You will need to configure authentication to your remote git repository. We recommend a personal access token.

### External Git Client

You can choose to use an external git client rather than using the library built into PowerShell Universal. This allows you additional configuration options such as using SSH authentication. PowerShell Universal will not use configured username, passwords or PATs when enabling this method. You will need to have a git client installed.

#### Setting Credentials

When using the external git client, you are responsible for configuring credentials before performing a synchronization.

You can configure credentials via git configuration or the URL passed to PowerShell Universal.

The following command will store the credentials in plaintext on the PowerShell Universal server. You will need to run this command as the service account user so that they have access to the credentials. This will create a `.git-credentials` file that will be used when authenticating against the target URL. You may need to change the URL depending on your git remote.

```batch
git config --global credential.helper store
echo "https://${username}:${password_or_access_token}@github.com" > ~/.git-credentials
```

You can also store credentials directly in the URL provided to PowerShell Universal.

```
https://adam:APP_TOKEN@github.com/myOrg/myRepo.git
```

#### Example: User Name and PAT

To use an external git client and pass a user name and PAT to authenticate with, you can specify them in the git remote URL. For example:

```
https://username:PAT@github.com/ironmansoftware/universal
```

#### Example: SSH and GitHub

First, you will need to configure your local ssh-agent and GitHub account with SSH keys.

You can follow their [guide here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

Next, you will provide a SSH URI for the git remote URL in PowerShell Universal. The configured SSH key will be used for the connection.

```
git@github.com:ironmansoftware/universal.git
```

#### SSL Certificate Problem

{% hint style="warning" %}
SSL Certificate problem: unable to get local issuer certificate
{% endhint %}

If you are running on Windows and receive an SSL Certificate problem, you may need to ensure that you have [enabled schannel support](https://stackoverflow.com/questions/23885449/unable-to-resolve-unable-to-get-local-issuer-certificate-using-git-on-windows).

#### Credential Persistence

{% hint style="warning" %}
Unable to persist credentials with the 'wincredman' credential store.
{% endhint %}

If you are on Windows and receive an error about persisting credentials in wincredman, you may need to set the credential persistence to DAPI. You can [learn how to do that here](https://github.com/GitCredentialManager/git-credential-manager/issues/633).

### Manual Mode

Manual mode requires users editing the PowerShell Universal instance to click Edit in order to make changes in the system.

<figure><img src="../.gitbook/assets/image (210).png" alt=""><figcaption></figcaption></figure>

Once the changes are complete, the user can then click Save Changes to begin a commit

<figure><img src="../.gitbook/assets/image (219).png" alt=""><figcaption></figcaption></figure>

On the git commit page, you can view the changed files and enter a commit message.

<figure><img src="../.gitbook/assets/image (466).png" alt=""><figcaption></figcaption></figure>

Once changes have been committed, they will be pushed to the remote and the service will start to synchronize with git again. There is also a chance that a git merge conflict can take place at this time. See [Dealing with Conflicts](git.md#dealing-with-conflicts) for more information.

Manual mode can be set in the git settings within the admin console or within `appsettings.json`.

```json
"Data": {
  "RepositoryPath": "%ProgramData%\\UniversalAutomation\\Repository",
  "ConnectionString": "filename=%ProgramData%\\UniversalAutomation\\database.db;upgrade=true",
  "RunMigrations": true,
  "GitRemote": "",
  "GitUserName": "",
  "GitPassword": "",
  "GitBranch": "",
  "GitSyncBehavior": "TwoWay",
  "GitInitializeBehavior": "",
  "GitSyncInterval": "1",
  "ConfigurationScript": "",
  "Mode": "Manual" // Or Automatic
},
```

### Method One: Pushing Local Files to Remote

{% hint style="info" %}
The default location for the local repository is `C:\ProgramData\UniversalAutomation\Repository`
{% endhint %}

You must ensure that your local folder is not a pre-existing git repository. Your repository directory should be simply a folder with your PowerShell Universal files. If a `.git` folder exists, and you do not wish to use this repository, you should delete it and PowerShell Universal will create a new repository.

If you are populating a new git repository, you need to ensure that the remote does not have any files in it. It should be a completely bare repository. For example, when creating a repository on GitHub, do not select a license or readme.md file to be created.

![](<../.gitbook/assets/image (275).png>)

Once the repository has been created, you can retrieve the git remote URL and provide that to the PowerShell Universal `appsettings.json` file.

![](<../.gitbook/assets/image (496).png>)

Once you have the branch, git remote URL and credentials, you can provide them to your `appsettings.json` file and the git remote will be populated with your local files.

### Method Two: Pulling from a Git Remote

{% hint style="info" %}
The default location for the local repository is `C:\ProgramData\UniversalAutomation\Repository`
{% endhint %}

You can configure PowerShell Universal to pull from a git remote. In this configuration, you must ensure that your local repository folder is completely empty. Any files within the folder will cause the git sync to fail and prevent it from recovering.

Configure the `appsettings.json` file to include the branch, credentials and git remote URL that you are cloning. Once the fields have been set, you can start the PowerShell Universal service. The first thing the service will do is clone the repository and configure it locally.

### Git Sync Timeout

Git sync will timeout if it cannot contact the remote after 60 minutes. This allows for time to download large repositories or delay with slow networks. You may see the PowerShell Universal service hang on "Synchronizing with git" during start up while the server waits for this to happen. If you wish to reduce this time, you can use the following appsetting.json setting. The value is in minutes.

```json
"Data" : {
    "GitSyncTimeout": 30
}
```

## Included Files

The files that are included with a git sync are any files within the local repository. This includes PS1 configuration files, pages XML files and any other files you may add manually.

The following are not included:

* appsettings.json
* database.db
* web.config
* PowerShell Universal Application Binaries

## Git History and Status Page

The history tab displays all the git commit history for the current repository.

<figure><img src="../.gitbook/assets/image (256).png" alt=""><figcaption></figcaption></figure>

The sync status tab displays the current status of nodes within the PSU cluster.

<figure><img src="../.gitbook/assets/image (214).png" alt=""><figcaption></figcaption></figure>

### Testing Git Sync

The best way to ensure that your git sync is working properly is to click the Synchronize Now button. This will force a sync to run, and you can verify whether the settings entered worked properly.

![](<../.gitbook/assets/image (138).png>)

### Viewing Changes

You can view changes within the table of git syncs. Each sync includes the number of changes found since the last sync, the SHA of the commit and a list of changes between that SHA and the previous one. When files are in a modified state, you will be able to view the diffs using the file diff tool.

![](<../.gitbook/assets/image (372).png>)

### Dealing with Conflicts

{% hint style="info" %}
Conflict resolution is only available in manual git sync mode.
{% endhint %}

When multiple users are editing the PowerShell Universal configuration files, there may be conflicts. PowerShell Universal will display that a particular node is in a conflicted state when attempting to commit changes. You will see a list of changes that are conflicted on the git commit page. Click the Resolve Conflict button to view the conflict in an editor.

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

In this example, the string for this endpoint was edited on both the remote and the local repository.

<figure><img src="../.gitbook/assets/image (226).png" alt=""><figcaption></figcaption></figure>

Edit the text to remove the conflict.

<figure><img src="../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

Save the changes and navigate back to the git commit page. Enter a new commit message for the merge conflict and click Commit Changes.

<figure><img src="../.gitbook/assets/image (469).png" alt=""><figcaption></figcaption></figure>

This will resolve the merge conflict and push to the remote.

## Git Synchronization Behavior

### Two-Way

By default, git synchronization will work both ways. If you make changes within the PSU admin console, those changes will be committed and sync'd to the configured remote.

Any changes that are made in the remote will be pulled locally.

If you setup git sync with a preexisting git remote, the changes will be pulled and synchronized locally. You cannot have any changes locally.

If you setup git sync with a bare repository, the local changes will be sync'd and the repository will be initialized.

### One-Way

You can adjust the git synchronization behavior by changing the `GitSyncBehavior` setting in `appsettings.json`. When set to `OneWay`, the admin console and management API will become read-only. The PowerShell Universal system will pull from the remote but will never push or commit locally.

```javascript
    "Data": {
    "RepositoryPath": "%ProgramData%\\UniversalAutomation\\Repository",
    "ConnectionString": "%ProgramData%\\UniversalAutomation\\database.db",
    "DatabaseType": "LiteDB",
    "GitRemote": "https://github.com/myorg/myrepo.git",
    "GitUserName": "any",
    "GitPassword": "MYPAT----------------"
    "GitBranch": "dev",
    "GitSyncBehavior": "OneWay",
    "ConfigurationScript": ""
  },
```

### Push-Only

Push-only git sync mode will not pull changes from the remote. Any changes made locally will be pushed up to the remote. The console will not be read-only. This configuration is useful for scenarios where you have one machine that is used to for the source-of-truth configuration for a pool of servers that are read-only.

```json
"Data": {
  "RepositoryPath": "%ProgramData%\\UniversalAutomation\\Repository",
  "ConnectionString": "%ProgramData%\\UniversalAutomation\\database.db",
  "DatabaseType": "LiteDB",
  "GitRemote": "https://github.com/myorg/myrepo.git",
  "GitUserName": "any",
  "GitPassword": "MYPAT----------------"
  "GitBranch": "dev",
  "GitSyncBehavior": "PushOnly",
  "ConfigurationScript": ""
},
```

## Personal Access Token

We recommend that you use a personal access token (PAT) over a user name and password. You can configure a personal access token by setting the password property in the `appsettings.json` or other configuration methods.

```javascript
    "Data": {
    "RepositoryPath": "%ProgramData%\\UniversalAutomation\\Repository",
    "ConnectionString": "%ProgramData%\\UniversalAutomation\\database.db",
    "GitRemote": "https://github.com/myorg/myrepo.git",
    "GitUserName": "any",
    "GitPassword": "MYPAT----------------"
  },
```

In GitHub, you can retrieve a personal access token by clicking your avatar in the top right, selecting Settings, Developer Settings and then Personal Access Tokens.

When generating your access token, ensure that you select the Repo permissions.

![](<../.gitbook/assets/image (36).png>)

Note, that if you are using BitBucket, you will need to specify the user name in addition to the PAT in `appsettings.json`.

## User name and password

You can also configure a git remote to authenticate with a user name and password. Set the user name and password either with the `appsettings.json` file or another configuration method.

```javascript
    "Data": {
    "RepositoryPath": "%ProgramData%\\UniversalAutomation\\Repository",
    "ConnectionString": "%ProgramData%\\UniversalAutomation\\database.db",
    "GitRemote": "https://github.com/myorg/myrepo.git",
    "GitUserName": "myusername",
    "GitPassword": "mypassword"
  },
```

## Benefits of Git Sync vs Manual Git Sync

It is possible to manually git sync a repository. PowerShell Universal uses very basic commands when dealing with git. Any changes made to PowerShell Universal through the admin console or API invoke a `git commit` and the author is set to the identity of the user making the change. During a git sync operation, we first perform a `git pull` to ensure that we have the latest version of files on the remote. Next, we perform a `git push` to push up local commits that have happened since the last sync.

You could achieve this functionality with a scheduled job.

That said, one feature of the git sync is that it analyzes the commit to ensure that only files that were changed during the sync are reloaded. This will stop dashboards from auto-deploying when they haven't changed or APIs services from restarting when environments haven't been updated. So there is a performance gain here.

The other issue is that due to the way that PowerShell Universal watches files (with a `FileSystemWatcher`) and the way that git updates files, the configurations will not reload automatically after a pull. You will have to ensure that you force the configurations to be reevaluated.

## Branching Strategies

Branching strategies in git dictate how changes are moved from one branch to another. Utilizing different types of branching strategies in PowerShell Universal can ensure that teams of different sizes can work effectively together in the platform.&#x20;

### Single Users and Small Teams

For single users and small teams, it may not be necessary to have more than a single branch. The branch is used for history tracking and provides the ability to roll back changes. Users will access PowerShell Universal directly to make changes or commit changes to the single branch from local clones of the repository directory using a tool like VS Code.&#x20;

* main - Single branch that accepts all changes directly

### Single Users and Smalls Teams with a Staging Branch

Even single users and small teams may find it advantageous to employ a staging, or dev, branch. This branch will receive changes during development. A standalone instance of PowerShell Universal will be configured to run against this dev branch so that users can validate changes before pushing to production.&#x20;

When using a staging branch configuration, the PowerShell Universal environments are completely separate. They use a different database and scheduler. Data such as identities, app tokens and job history are not shared across the environments.

{% hint style="info" %}
Each instance of PowerShell Universal requires a [license](../licensing.md). In this configuration, two licenses would be required.&#x20;
{% endhint %}

A separate PowerShell Universal instance can then be configured to point to a main, or production, branch that will receive updates via merges or Pull Requests in a system like GitHub. In this configuration, it is possible to use one-way git sync to pull changes from main but never push from the platform. This also prevents most merge conflicts as they will be addressed in the dev branch or via the merge tool in the source repository.&#x20;

* main - Production branch that receives changes from pull requests of the dev branch
* dev - The staging branch used to accept commits and validate changes before pushing to master.&#x20;

### Medium to Large Size Teams

When considering teams with more than a couple of developers, a more complex branching strategy may be helpful to better evaluate code changes and avoid merge conflicts. PowerShell Universal provides basic merge tools, but better tooling is available for this purpose, such as GitHub pull request, GitLab merge requests and local tools like GitKraken.&#x20;

In medium size teams, it may be desirable to have additional feature branches that isolate specific changes to a certain branch. For example, a developer may be creating a new set of APIs to manage Azure in PowerShell Universal. In order to avoid breaking changes in the dev branch, developers will create their own feature branch that contains all their changes until it is complete enough to be merged into the development branch.&#x20;

{% hint style="info" %}
PowerShell Universal provides [developer licenses](../licensing.md) to avoid having to purchase a license for every developer on your team. Licenses would still be required for the production and staging environments.
{% endhint %}

In this type of configuration, local development is ideal because the developers will work on their local PowerShell Universal instance within their feature branch. When the feature is complete, they will create a Pull or Merge request in the source repository to move changes into the dev branch. Testing will be completed on the dev branch before merging to production.&#x20;

Similar to a main\dev branching strategy, all PowerShell Universal instances will be isolated and will not share a database or scheduler.&#x20;

Once a set of features is ready for production, a Pull or Merge request will be made from dev to the main branch.&#x20;

* main - Production branch that will only receive changes from dev
* dev - Staging branch that receives changes from feature branches but is not changed directly
* feature - Feature branch that is committed to directly by developers and merged to dev when ready

### Large Teams

In large teams, additional levels of branching may be necessary. Branches for releases or hotfixes may be important to isolate upcoming releases and bug fixes from new feature development. As users develop new features, other developers can be working on patching existing releases or preparing the next release without accepting changes coming in from new feature work. An optional staging branch could also be employed to provide a QA environment that is isolated from development. This would mimic the production environment more closely and avoid accepting changes directly to the branch.&#x20;

In this configuration, production environments could be pinned to release or hotfix branches. When a new release comes out, PowerShell Universal would have to sync with the new branch. The benefit of such a configuration is the ability to quickly change versions in the event of unintended consequences of changes in the release.&#x20;

{% hint style="warning" %}
PowerShell Universal currently has limited support for switching branches on live systems. We recommend switching the branch and restarting the PowerShell Universal service to avoid issues.&#x20;
{% endhint %}

* main - Main branch that holds the current, ready to release code but may not be run in production directly
* staging - Quality assurance branch that receives changes from development
* dev - Development branch that receives changes from feature branches but is not changed directly
* feature - Feature branch that is committed to directly by developers and merged to dev when ready
* release - A versioned release branch that contains a set of features and fixes. Created off of the main branch.&#x20;
* hotfix - A child branch of a release that contains fixes. This could be considered a release branch but is created from a release rather than the main branch.&#x20;

