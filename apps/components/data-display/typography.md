---
description: Typography component for Universal Apps
---

# Typography

Use typography to present your design and content as clearly and efficiently as possible.

Too many type sizes and styles at once can spoil any layout. A typographic scale has a limited set of type sizes that work well together along with the layout grid.

![](<../../../.gitbook/assets/image (83).png>)

## All Typography Types

```powershell
@("h1", "h2", "h3", "h4", "h5", "h6", "subtitle1", "subtitle2", "body1", "body2", 
"caption", "button", "overline", "srOnly", "inherit", 
"display4", "display3", "display2", "display1", "headline", "title", "subheading") | ForEach-Object {
    New-UDTypography -Variant $_ -Text $_ -GutterBottom
    New-UDElement -Tag 'p' -Content {}
}
```

## Colored Text

You can use the `-Style` parameter to define colors for your text.

```powershell
New-UDTypography -Text 'My Text' -Style @{ color = 'blue' }

```

## Theme-based Styling

You can use [theme-based](../../themes/#setting-the-default-theme) styling by using the `-Sx` parameter of `New-UDTypography`. For example, to apply the secondary text color, you can use the following syntax.&#x20;

```powershell
New-UDTypography -Text 'Secondar' -Sx @{
    color = 'text.secondary'
}
```

## API

* [New-UDTypography](https://github.com/ironmansoftware/universal-docs/blob/v5/cmdlets/New-UDTypography.txt)
