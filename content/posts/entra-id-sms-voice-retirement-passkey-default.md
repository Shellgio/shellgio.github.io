---
date: '2026-07-14T22:15:43+01:00'
draft: false
title: 'Microsoft is retiring SMS and Voice authentication methods'
author: 'Shellgio'
categories:
- Microsoft Entra
tags:
- Passkeys
- MFA
- Entra ID
- Phishing-resistant
- Identity
- Authentication Methods
description: 'Microsoft is retiring native telephony MFA and transitioning Entra ID to a passkey-by-default model. This guide outlines the implementation timeline and steps for administrators to maintain authentication continuity.'
featuredimage: "/images/entra-id-sms-voice-retirement-passkey-default.png"
---

I usually say that **if you're only doing MFA you're already late**. The industry is moving beyond traditional MFA and into a model where phishing-resistant authentication becomes the expected baseline.

Proof ot that is that Microsoft has announced that **passkeys will become the default authentication method** for Microsoft Entra ID tenants.

At the same time, M**icrosoft is retiring its role as a native telecom provider for MFA**. This does not mean that SMS and voice are being banned as authentication methods at the protocol level, but **Microsoft will no longer provide the underlying delivery service** for those codes.

For users currently using telephony methods, Microsoft will move the **Registration Campaign** to a **Microsoft managed** state. This means users will be proactively prompted to register a passkey during the sign-in flow.

Organizations that still have a regulatory, business, or operational requirement to **continue using SMS or voice** will need to procure and manage their own telecom services through supported **third-party providers**.

## The rollout roadmap

The transition is phased, which gives organizations time to audit their authentication methods, understand dependency on phone-based MFA, and prepare alternative servicing paths where needed.

| Date | Milestone / impact |
| --- | --- |
| **September 1, 2026**| Rollout begins. Passkeys are auto-enabled for SMS and voice users. Registration Campaign is set to Microsoft managed. |
| **September 18, 2026** | Pricing and commercial terms for third-party telecom providers become available in the Microsoft Security Store. |
| **October 30, 2026** | Administrators can begin selecting and configuring third-party telecom providers for legacy support. |
| **February 1, 2027** | Microsoft-provided SMS and voice retire. Passkey registration prompts become enforced with no opt-out after this date. |

## What does this mean for organizations?

If your organization can move fully to passkeys and other phishing-resistant methods, this is the right moment to accelerate that work.

If you still need telephony-based MFA for specific users, regions, scenarios, or legacy processes, you will need to plan for that explicitly. Microsoft will no longer absorb the operational and commercial complexity of telecom delivery.

The key areas to review are:

- Users still registered only with phone-based MFA
- Break-glass and emergency access processes
- Account recovery flows
- Legacy applications or workflows that assume SMS or voice fallback
- Helpdesk procedures for authentication method resets
- Conditional Access policies that still tolerate phishable methods
- User communication and adoption campaigns

The biggest risk is not that passkeys are technically difficult. The risk is discovering too late that some critical population still depends on SMS or voice.

## Technical requirements

Microsoft Entra ID supports several passkey options, which gives organizations flexibility depending on the workforce and device strategy:

- **Synced passkeys**, stored in platform credential managers such as iCloud Keychain or Google Password Manager
- **Device-bound passkeys**, including Microsoft Authenticator passkeys and Entra passkeys on Windows
- **FIDO2 security keys**, which remain a strong option for high-security users and privileged roles

To help with adoption, Microsoft recommends using the **Conditional Access Optimization Agent** to support deployment campaigns and move users toward phishing-resistant authentication. There is also lots of documentation about deploying passkeys in Microsoft Learn:


> [!NOTE]
> | Method                             | Guidance                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
> | ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
> | FIDO2 security keys                | - FIDO2 security keys must be turned on in Microsoft Entra ID. You can [enable FIDO2 security keys in the Authentication methods policy](how-to-authentication-passkeys-fido2).<br>- Consider registering keys on behalf of your users with the Microsoft Entra ID provisioning APIs. For more information, see [Provision FIDO2 security keys using Microsoft Graph API](how-to-authentication-passkeys-fido2#provision-fido2-security-keys-using-microsoft-graph-api-preview).                              |
> | Synced passkey                     | - Synced passkeys must be turned on in Microsoft Entra ID. You can [enable FIDO2 security keys in the Authentication methods policy](how-to-authentication-passkeys-fido2)<br>- Users can use synced passkeys managed by Apple Keychain and Google cloud or 3rd party passkey managers                                                                                                                                                                                                                        |
> | Passkey in Microsoft Authenticator | - Passkey in Microsoft Authenticator must be turned on in Microsoft Entra ID. You can [enable FIDO2 security keys in the Authentication methods policy](how-to-authentication-passkeys-fido2)<br>- Users sign in to Microsoft Authenticator App directly to bootstrap a passkey in the app.<br>- Users can use their TAP to sign into Microsoft Authenticator directly on their iOS or Android device. [Register passkeys in Authenticator on Android or iOS devices](how-to-register-passkey-authenticator). |
> 
> Source: [Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/authentication/how-to-deploy-phishing-resistant-passwordless-authentication#step-2-bootstrap-a-portable-credential)


## Audit users still using SMS or voice methods

Before changing policy, I would start with visibility.

In the Microsoft Entra admin center:

1. Go to **Entra ID**.
2. Open **Authentication methods**.
3. Go to **User registration details**.
4. Filter **Methods registered** for:
   - Mobile phone
   - Office phone
   - Alternative mobile phone
5. Export the report.
6. Identify users who do not have a phishing-resistant method registered.

![Phone/SMS User registration](/images/entra-authenticationmethods-registered-phonesms-mfa.png)

For bulk reporting, you can use Microsoft Graph and review the `userRegistrationDetails` endpoint, especially the `methodsRegistered` property, to identify users still tied to phone-based authentication methods. Learn more about it on [this post](https://ourcloudnetwork.com/microsoft-are-set-to-retire-sms-voice-authentication-methods/) from Daniel Bradley at OurCloudNetwork.

## Enforcement and opt-out considerations

From September 2026 to February 2027, **organizations will have a temporary opt-out window** to support migration and testing.

After **February 1, 2027**, that opt-out goes away. Tenants that still depend on Microsoft-provided native telephony after that date can face sign-in disruptions.

This is why the work should not be treated as a last-minute MFA cleanup. It is an identity architecture change, and it should be planned as such.

## My take

My recommendation is simple: start with the **audit**, **identify** users still relying on phone methods, **define your strategy**, and **update recovery and helpdesk processes** before enforcement arrives.

And remember, **single factor authentication is dead, just MFA is the least secure minimal required.** So plan accordingly and start deploying and enforce secure autentication methods and identity strategy.

## Sources
- [Microsoft Entra ID security updates: Passkeys are the default authentication method in Entra ID](https://www.microsoft.com/en-us/security/blog/2026/07/13/microsoft-entra-id-security-updates-passkeys-are-the-default-authentication-method-in-entra-id/)
- [MC1426371 - Microsoft Entra ID retirement of SMS/Voice and passkey by default](https://mc.merill.net/message/MC1426371)
- [Microsoft Learn - SMS and voice retirement in Microsoft Entra ID](https://learn.microsoft.com/entra/identity/authentication/concept-sms-voice-retirement)
- [Microsoft - Passkey by default](https://aka.ms/passkeybydefault)
- [Microsoft are set to retire SMS & Voice authentication methods](https://ourcloudnetwork.com/microsoft-are-set-to-retire-sms-voice-authentication-methods/)
