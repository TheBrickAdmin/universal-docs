---
description: This document outlines how to build custom Universal app components.
---

# Building Custom JavaScript Components

Universal is extensible and you can build custom JavaScript components and frameworks. This document will cover how to build custom components that integrate with the Universal app platform.

{% hint style="warning" %}
This is an advanced topic and not required if you simply want to use Universal Apps.
{% endhint %}

## Step-By-Step

This following section will take you step-by-step through the different aspects of building a Universal App component.

### 1. Installing Dependencies

You will need to install the following dependencies before creating your component.

* [NodeJS](https://nodejs.org/en/)

### 2. Install PowerShellUniversal.Apps.Tools Module

The `PowerShellUniversal.Apps.Tools`module is available on the PowerShell Gallery.

```bash
Install-Module PowerShellUniversal.Apps.Tools
```

### 3. Create a New Project

Use the `New-UDReactComponent`cmdlet to create a new project. This will scaffold the project and all the necessary files.&#x20;

```powershell
New-UDReactComponent -Path .\project -Name 'ReactIcon'
```

### 4. Install JavaScript Packages

You can now install the JavaScript React packages you want to use in your component. For example, you could install `react-icons`.&#x20;

```bash
Add-UDReactComponentLibrary -Path .\project -Library 'react-icons'
```

### 5. Update Your Project

Now you will need to author the code to use the React component. You will need to update the PSM1 and JSX file to author the logic for your component.&#x20;

For example, the PSM1 would load the JavaScript file into the PSU AssetService. It would then define functions to load the JavaScript component and pass in properties.&#x20;

```powershell
$IndexJs = Get-ChildItem "$PSScriptRoot\index.*.bundle.js"
$AssetId = [UniversalDashboard.Services.AssetService]::Instance.RegisterAsset($IndexJs.FullName)
function New-UDReactIcon {
    param(
        [Parameter()]
        [string]$Id = (New-Guid).ToString(),
        [Parameter(Mandatory)]
        [string]$Icon
    )
    @{
        assetId  = $AssetId 
        isPlugin = $true 
        type     = "ReactIcon"
        id       = $Id
        icon     = $icon
    }
}
```

In the component's JSX file, update the JavaScript to handle the properties passed to the component from PowerShell.

```javascript
import React from 'react';
import { withComponentFeatures } from 'universal-dashboard'
import * as Icons from 'react-icons/bs';
import { IconContext } from 'react-icons/lib';
const UDComponent = props => {
    return <IconContext.Provider value={{ ...props }}>
        {React.createElement(Icons[props.icon])}
    </IconContext.Provider>
}

export default withComponentFeatures(UDComponent)
```

### 6. Build The Project

You can now build your project. It will output a module you can load into PowerShell Universal.&#x20;

```powershell
Invoke-UDReactComponentBuild -Path .\project -OutputPath .\output -Force
```

### 6. Use in PowerShell Universal

Within your app, load your module and execute the function.&#x20;

```powershell
Import-Module ReactIcon 

New-UDApp -Content {
   New-UDReactIcon -Icon 'User'
}
```

## Props

Props are values that are either passed from the PowerShell hashtable provided by the user or by the Universal App `withComponentsFeature` high-order function.

### Standard

The properties that you set in your hashtable in PowerShell will automatically be sent in as props to React component.

For example, if you set the `text` property of the hashtable like this.

```powershell
function New-UDText {
    param(
        [Parameter()]
        [string]$Text
    )

    @{
        type = "text"
        isPlugin = $true
        assetId = $AssetId 

        text = $Text
    }
}
```

Then you will have access to that prop in React.

```javascript
import React from 'react';
import { withComponentFeatures } from 'universal-dashboard';

const UDText = props => {
    return <div>{props.text}</div>
}

export default withComponentFeatures(UDText);
```

### Endpoints

Endpoints are special in the way they are registered and the way that they are passed as props to your component. You will need to call `Register` on the endpoint in PowerShell and pass in the Id and PSCmdlet variables.

```powershell
function New-UD95Button {
    param(
        [Parameter()]
        [string]$Id = [Guid]::NewGuid(),
        [Parameter()]
        [string]$Text,
        [Parameter()]
        [Endpoint]$OnClick
    )

    if ($OnClick)
    {
        $OnClick.Register($Id, $PSCmdlet)
    }

    @{
        type = "ud95-button"
        isPlugin = $true 
        assetId = $AssetId

        id = $Id 
        text = $Text 
        onClick = $OnClick
    }
}
```

Endpoints are created from ScriptBlocks and are executed when that event happens.

```powershell
New-UD95Button -Text 'Hello' -OnClick {
    Show-UDToast -Message 'Test' 
}
```

Universal will automatically wire up the endpoint to a function within JavaScript. This means that you can use the props to call that endpoint.

Notice the `props.onClick` function call. This will automatically call the PowerShell script block on the server.

```javascript
import React from 'react';
import { withComponentFeatures } from 'universal-dashboard';
import { Button } from 'react95';

const UD95Button = props => {

    const p = {
        onClick: () => props.onClick()
    }

    return <Button {...p}>{props.text}</Button>
}

export default withComponentFeatures(UD95Button);
```

### setState

The `setState` prop is used to set the state of the component. This ensures that the state is tracked and your component will work with `Get-UDElement`.

For example, with a text field, you'll want to call `props.setState` and pass in the new text value for the state.

```javascript
const UDTextField = (props) => {
    const onChange = (e) => {
        props.setState({value: e.target.value})
    }

    return <TextField  {...props} onChange={onChange} />
}

export default withComponentFeatures(UDTextField);
```

### children

The `children` prop is a standard React prop. If your component supports child items, such as a list or select box, you should use the standard `props.children` prop to ensure that the cmdlets `Add-UDElement` , `Remove-UDElement` and `Clear-UDElement` function correctly.
