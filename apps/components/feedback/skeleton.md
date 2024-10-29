---
description: A skeleton component for PowerShell Universal Apps.
---

# Skeleton

A skeleton is a form of a loading component that can show a placeholder while data is received.

## Variants

There are three variants that you can use for a skeleton. You can use a circle, text or a rectangle. You can also define the height and width of the skeleton.

```powershell
New-UDSkeleton
New-UDSkeleton -Variant circle -Width 40 -Height 40
New-UDSkeleton -Variant rect -Width 210 -Height 118
```

![Skeletons](<../../../.gitbook/assets/image (476).png>)

## Animations

Skeletons will use the pulsate animation by default. You can also disable animation or use a wave animation.

```powershell
New-UDSkeleton
New-UDSkeleton -Animation disabled
New-UDSkeleton -Animation wave
```

![Animations](../../../.gitbook/assets/animation.gif)

## API

* [New-UDSkeleton](https://github.com/ironmansoftware/universal-docs/blob/v5/cmdlets/New-UDSkeleton.txt)
