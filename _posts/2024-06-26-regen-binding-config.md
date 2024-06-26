---
layout: post
title: ".NET Framework: Regenerating Binding Config"
categories: tech snippet
---

When the assembly binding redirects in App.config become inconsistent or out of date, running the following from the Package Manager Console (Visual Studio > View > Other Windows > Package Manager Console) will regenerate them.

```powershell
Get-Project –All | Add-BindingRedirect
```
