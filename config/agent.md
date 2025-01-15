---
description: Information about PowerShell Universal agents.
---

# Agent

You can learn more about installing the agent on our [Installation page](../getting-started/).

## agent.json

After installing the agent, you will need to configure the client by using an `agent.json` file.&#x20;

This JSON file configures the Agent to connect to the hub and run scripts when invoked.

```json
{
    "Connections": [
        {
            "Url": "http://localhost:5000",
            "Hub": "eventHub",
            "AppToken": "tokenXyz",
            "ScriptPath": "script.ps1"
        }
    ]
}
```

### Location&#x20;

#### System&#x20;

The system location of `agent.json` is in `$ENV:ProgramData\PowerShellUniversal`.&#x20;

#### User

The user specific location of `agent.json` is in `$ENV:AppData\PowerShellUniversal.`

### Options

The below options can be included in the `agent.json` file.

#### Connections

These are the connections to Event Hubs. Each connection can contain it's own URL, hub, authentication and script to execute.

#### URL (Required)

The URL of the PowerShell Universal service.

#### Hub (Required)

The name of the Event Hub.

#### AppToken

The app token used to authentication against the hub.

#### UseDefaultCredentials

Windows Authentication will be used to authenticate against the hub.

#### ScriptPath

The script to execute when an event is received. This script is read into memory and not from disk. Variables such as `$PSScriptRoot` are currently not supported. This is optional as event hubs can also run commands directly.

## Environment Variables

Environment variables can be used to configure various operational settings for the agent.&#x20;



| Name               | Description                               | Default Value |
| ------------------ | ----------------------------------------- | ------------- |
| PSU\_AgentLogLevel | Sets the log level for the agent service. | Information   |
|                    |                                           |               |
|                    |                                           |               |
