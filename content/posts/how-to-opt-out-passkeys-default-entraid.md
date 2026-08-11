---
date: '2026-08-11T08:00:00+01:00'
draft: false
title: 'How to Temporarily Opt Out of Passkeys by Default in Microsoft Entra ID'
author: 'Shellgio'
categories:
  - Microsoft Entra
  - Microsoft Security
tags:
  - Microsoft Entra ID
  - Passkeys
  - Authentication
  - MFA
  - Passwordless
  - Microsoft Graph
description: 'How to temporarily opt out of Microsoft Entra ID automatic passkey enablement and Registration Campaign rollout before September 2026.'
featuredimage: "/images/How to Temporarily Opt Out of Passkeys by Default in Microsoft Entra ID.png"
---

Recently I posted how Microsoft announced that starting September 1, 2026, users who are enabled for SMS or voice authentication will be automatically enabled for passkeys. Microsoft will also move the Registration Campaign to a Microsoft-managed configuration for these users, which means they can start receiving prompts to register a passkey when they sign in and complete MFA.

For organizations that are already ready for passkeys, this is probably not a problem. But there are environments where the security team may need more time to migrate users, review authentication methods, or deal with specific operational requirements.

Microsoft has now documented a temporary opt-out for this transition so I'll explain how can you opt-out and verify if you have already opted-out.

### Requirements

Microsoft states that the following Microsoft Graph permission is required:

`Policy.ReadWrite.AuthenticationMethod`

The documented API request uses the Microsoft Graph **beta** endpoint:

`PATCH https://graph.microsoft.com/beta/policies/authenticationmethodspolicy`

### Microsoft Graph request

The request body is:

```http
PATCH https://graph.microsoft.com/beta/policies/authenticationmethodspolicy
Content-Type: application/json

{
   "optOutSettings": {
      "passkeyDynamicMigration": true
   }
}
```

The relevant property is:

`optOutSettings.passkeyDynamicMigration`

Setting this property to `true` excludes the tenant from the automatic passkey enablement and Registration Campaign rollout during the temporary opt-out period.

There is an important detail here: the documentation describes this as a **tenant-level opt-out**. The request is made against the tenant's authentication methods policy, rather than configuring an exception for individual users.

## What does the opt-out actually prevent?

The opt-out prevents the automatic changes associated with the September 1 transition.

According to Microsoft's documentation, once the setting is applied, the tenant is excluded from:

- Automatic passkey enablement.
- Automatic Registration Campaign rollout.

This applies during the temporary opt-out period.

In other words, this is useful if you are not ready for Microsoft to automatically move your SMS and voice users into the passkey registration experience on September 1.

It should not be interpreted as a way to permanently opt out of Microsoft's move away from Microsoft-provided SMS and voice.

## The opt-out has an expiration date

This is probably the most important operational detail.

The opt-out only delays the transition. It does **not** remove the February 1, 2027 enforcement.

Microsoft explicitly states that, beginning February 1, 2027, the standard passkey migration and enforcement timelines apply regardless of the opt-out setting.

There is no opt-out from that February 1 behavior.

Microsoft-provided SMS and voice authentication will be retired, and users who only have SMS or voice available for MFA will be required to register a passkey during sign-in before they can continue.

## How to opt-out step by step

### Using Graph Explorer

Go to [Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer) and log-in with your admin account:

![Log in Graph Explorer](</images/CleanShot 2026-08-11 at 08.30.24.png>)

To verify if you have opted-out select method as GET and URL https://graph.microsoft.com/beta/policies/authenticationMethodsPolicy and then run the query. If `optOutSettings` is null you haven't opted-out yet

![Verify Opt-out status](</images/CleanShot 2026-08-11 at 08.36.05.png>)

To enable the opt-out set the method to PATCH and this JSON as the body:
```json
{        
  "optOutSettings": {
    "passkeyDynamicMigration": true
  }
}
```
![PATCH method](</images/CleanShot 2026-08-11 at 08.39.38.png>)

If you get a 403 error this most likely caused by insufficient permissions. Go to the Modify permissions tab and grant yourself `Policy.ReadWrite.AuthenticationMethod`:

![403 error](</images/CleanShot 2026-08-11 at 08.39.57.png>)

![Grant Policy.ReadWrite.AuthenticationMethod permissions](</images/CleanShot 2026-08-11 at 08.40.13.png>)

Run the query and you should receive a 204 response 
![204 response](</images/CleanShot 2026-08-11 at 08.40.51.png>)

With the new setting added as true if you verify again with the GET method you should see the new setting:

![200 response](</images/CleanShot 2026-08-11 at 08.41.30.png>)

### Using PowerShell
First, connect to Microsoft Graph with permissiones to read and write authentication methods:
```PowerShell
Connect-MgGraph -Scopes "Policy.ReadWrite.AuthenticationMethod"
```

When you login, you may be prompted for permissions. If your account doesn't have enought permissions to approve those permisisions you'll need and admin to approve them for you ask your Global Admin or Application Administrator.
![Permissions prompt](</images/CleanShot 2026-08-11 at 09.33.58.png>)

After login run this to verify if you have already opt-out:
```PowerShell
(Invoke-MgGraphRequest -Method GET -Uri "https://graph.microsoft.com/beta/policies/authenticationmethodspolicy").optOutSettings
```
If it doesn't return anything it means you haven't opted out
![Verifying opt-out status](</images/CleanShot 2026-08-11 at 09.35.55.png>)

To opt-out run the following:

```PowerShell
Connect-MgGraph -Scopes "Policy.ReadWrite.AuthenticationMethod"
$body = @{
    optOutSettings = @{
        passkeyDynamicMigration = $true
    }
} | ConvertTo-Json

Invoke-MgGraphRequest `
    -Method PATCH `
    -Uri "https://graph.microsoft.com/beta/policies/authenticationmethodspolicy" `
    -Body $body
```

It won't return anything as a result but if you verify your opt-out status again it should return that `passkeyDynamicMigration`  has the value `True`
![Opt-out status enabled](</images/CleanShot 2026-08-11 at 09.36.46.png>)

## PowerShell Script

I've created a PowerShell script to perfom the opt-out or just verify if the opt-out is enabled. The script verify if the opt-out is enabled before doing so.

You can check it out in my Github: [set-EntraIDPasskeysDefaultOptout](https://github.com/Shellgio/set-EntraIDPasskeysDefaultOptout)

## Sources

- Microsoft Learn: [Passkeys by default and retirement of Microsoft-provided SMS and voice authentication](https://learn.microsoft.com/en-us/entra/identity/authentication/concept-sms-voice-retirement#temporarily-opt-out-of-the-automatic-passkey-enablement)
- YouTube: [Microsoft Entra / Passkeys by default](https://www.youtube.com/watch?v=8cupC_oRrMY)