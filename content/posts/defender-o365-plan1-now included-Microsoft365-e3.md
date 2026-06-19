---
date: '2026-06-19T15:13:43+01:00'
draft: false
title: 'Defender for Office 365 Plan 1 Is Coming to M365 E3'
author: 'Shellgio'
categories:
- Microsoft Defender for Office 365
- Microsoft Security
tags:
- MDO
- Licensing
- EOP
- Microsoft 365 E3
description: "If you manage Exchange Online for an E3 tenant, there's a change rolling out right now that deserves a spot on your review list. Microsoft has started rolling out Defender for Office 365 (MDO) Plan 1 to Microsoft 365 E3/G3 and Office 365 E3/G3 licenses"
featuredimage: "../images/defender-o365-plan1-now included-Microsoft365-e3.jpg"
---

If you manage Exchange Online for an **E3** tenant, **you are getting more bang for your buck**. Microsoft has started rolling out Defender for Office 365 (MDO) Plan 1 to **Microsoft 365 E3/G3 and Office 365 E3/G3** licenses, with completion expected by **August 2026**. This doesn't requiere any purchase, or separate provisioning, it just show up automatically for licensed users as the rollout reaches your tenant.

## What you had vs. what you're getting

Up to now, E3 mailboxes were protected by **Exchange Online Protection (EOP)** — spam filtering, basic anti-malware, and standard anti-phishing. That's table stakes, and it stays in place.

| Capability | EOP (already in E3) | New with MDO Plan 1 | Still requires Plan 2 |
|---|---|---|---|
| Spam / bulk mail filtering | ✅ | — | — |
| Signature-based anti-malware | ✅ | — | — |
| Basic anti-phishing / spoof intelligence | ✅ | — | — |
| Safe Links (time-of-click URL scanning, Teams + Office docs) | — | ✅ | — |
| Safe Attachments (sandbox detonation, incl. SharePoint/OneDrive/Teams) | — | ✅ | — |
| Zero-day malware protection | — | ✅ | — |
| Real-time detections reporting | — | ✅ | — |
| Anti-phishing enhancements (impersonation insight, mailbox intelligence) | — | Partial | ✅ Full |
| Attack Simulation Training | — | — | ✅ |
| Automated Investigation and Response (AIR) | — | — | ✅ |
| Threat Explorer / advanced hunting | — | — | ✅ |

The SOC-facing tooling like simulation, automation or hunting stays E5-exclusive or a Plan 2 add-on. What you're getting is the prevention layer, not the investigation layer.


## Why this matters for your rollout planning

Because protections turn on automatically, this isn't a "review when convenient" item — it's closer to an unannounced policy change landing in your tenant. A few things worth checking now, before it hits:

1. **Preset security policies** — if you're not already on Standard or Strict preset policies, now's the time to evaluate them rather than let default settings apply blind.
2. **Mail flow impact** — Safe Links rewriting and Safe Attachments detonation can introduce latency or interact with existing transport rules; review connectors and any third-party security layers (Cisco SES, Mimecast, Proofpoint, etc.) for redundancy or conflict.
3. **End-user experience** — clicked-link warning pages and quarantine notifications are new touchpoints; a short heads-up to your user base avoids a spike in help desk tickets.
4. **Exclusions** — if you have legitimate reasons to exempt certain users or domains, build those into custom policies now rather than reactively.

## The takeaway

For E3 tenants currently relying solely on EOP or paying for a third-party bolt-on, this is a meaningful entitlement bump at no extra licensing cost. The catch is timing: because it auto-enables, your window to shape *how* it behaves in your environment is now — before rollout reaches your tenant, not after users start asking why a link got blocked.

If you're running E3 today, this is worth treating as a mini-project or a checkup: confirm your current mail flow baseline, decide on Standard vs. Strict, and document exclusions before the toggle flips for you automatically.

## Sources 
[Microsoft Defender for Office 365 Plan 1 is now rolling out to Microsoft 365 E3 and Office 365 E3](https://techcommunity.microsoft.com/blog/microsoftdefenderforoffice365blog/microsoft-defender-for-office-365-plan-1-is-now-rolling-out-to-microsoft-365-e3-/4527287), Microsoft Tech Community.*
