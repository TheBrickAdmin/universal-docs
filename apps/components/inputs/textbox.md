---
description: Textbox component for Universal Apps
---

# Textbox

A textbox lets users enter and edit text.

## Textbox

![](<../../../.gitbook/assets/image (541).png>)

```powershell
New-UDTextbox -Label 'Standard' -Placeholder 'Textbox'
New-UDTextbox -Label 'Disabled' -Placeholder 'Textbox' -Disabled
New-UDTextbox -Label 'Textbox' -Value 'With value'
```

## Password Textbox

A password textbox will mask the input.

![](<../../../.gitbook/assets/image (497).png>)

```powershell
New-UDTextbox -Label 'Password' -Type password
```

## Multiline

You can create a multiline textbox by using the `-Multiline` parameter. Pressing enter will add a new line. You can define the number of rows and the max number of rows using `-Rows` and `-RowsMax`.

```powershell
New-UDTextbox -Multiline -Rows 4 -RowsMax 10
```

## Interaction

### Retrieving a textbox value

You can use `Get-UDElement` to get the value of a textbox

```powershell
New-UDTextbox -Id 'txtExample' 
New-UDButton -OnClick {
    $Value = (Get-UDElement -Id 'txtExample').value 
    Show-UDToast -Message $Value
} -Text "Get textbox value"
```

### Setting the textbox value

```powershell
New-UDTextbox -Id 'txtExample' -Label 'Label' -Value 'Value'

New-UDButton -OnClick {

    Set-UDElement -Id 'txtExample' -Properties @{
        Value = "test123"
    }

} -Text "Get textbox value"
```

## Icons

You can set the icon of a textbox by using the `-Icon` parameter and the `New-UDIcon` cmdlet.

```powershell
New-UDTextbox -Id "ServerGroups" -Icon (New-UDIcon -Icon 'server') -Value "This is my server"
```

![](<../../../.gitbook/assets/image (46).png>)

## OnEnter

The `-OnEnter` event handler is executed when the user presses enter in the text field. It is useful for performing other actions, like clicking a button, on enter.

```powershell
New-UDTextbox -OnEnter {
    Invoke-UDEndpoint -Id 'submit' -Session
}

New-UDButton -Id 'submit' -OnClick {
    Show-UDToast -Message 'From Textbox'
}
```

## OnBlur

The `-OnBlur` event handler is executed when the textbox loses focus.

```powershell
New-UDTextbox -OnBlur {
    Show-UDToast "Blurred"
}
```

## OnValidate

Use the `-OnValidate` event handler to validate input typed in the textbox.

```powershell
New-UDTextbox -OnValidate {
    if ($EventData.Length -lt 10)
    {
        New-UDValidationResult -ValidationError 'String needs to be longer than 10'
    }
}
```

## API

[New-UDTextbox](https://github.com/ironmansoftware/universal-docs/blob/master/cmdlets/New-UDTextbox.txt)
