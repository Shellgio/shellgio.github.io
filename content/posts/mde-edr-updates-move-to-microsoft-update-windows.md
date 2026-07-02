---
date: '2026-07-02T19:45:00+01:00'
draft: false
title: 'Microsoft Defender for Endpoint EDR updates are moving to Microsoft Update on Windows'
author: 'Shellgio'
categories:
- Microsoft Defender for Endpoint
- Microsoft Security
tags:
- MDE
- Windows
- EDR
- Microsoft Update
description: 'Microsoft Defender for Endpoint EDR updates on Windows are moving from the monthly Windows security update to Microsoft Update, allowing Microsoft to deliver EDR security improvements independently from operating system updates.'
featuredimage: "/images/mde-edr-updates-move-to-microsoft-update-windows.png"
---

Until now, **updates for Microsoft Defender for Endpoint** were bundled with the monthly Windows security update. Microsoft is changing this behavior and moving those EDR updates to **Microsoft Update**, aligning them with the way other Microsoft Defender components are serviced.

And I think this is a good thing.

Why? Because it allows Microsoft to deliver security improvements for the EDR component independently from the regular operating system update cycle.

## What is changing?

Microsoft Defender for Endpoint EDR updates will no longer be delivered as part of the monthly Windows security update. Instead, they will be delivered through **Microsoft Update** using **KB5005292**, once the required prerequisites are installed on the device.

Microsoft is also introducing a new **Defender Update Service**. After the first update is installed, a new directory will be created on the device:

```text
%ProgramData%\Microsoft\Microsoft Defender\Defender Update
```

The rollout started with **Windows 10** in late May 2026 and will expand to **Windows 11** and the remaining supported Windows versions. Microsoft expects the rollout for Windows 10 and Windows 11 to be completed by fall 2026.

## What does this mean for security teams?

For most organizations, probably not much needs to be changed.

If your devices already receive updates from **Microsoft Update**, there is no action required. EDR updates should continue to arrive through the normal Microsoft update channels.

But if you are in an environment where updates are deployed manually, or where update flows are tightly controlled, this is something worth reviewing. In that case, the new Defender update package needs to be included in the standard update process.

This is especially important for organizations using:

- Manual update package deployment
- Strict update rings
- Offline or restricted network scenarios
- Internal documentation that still assumes EDR updates arrive only with monthly Windows security updates

In other words, if your patching process is very controlled, make sure this new servicing path is understood and documented.

## Prerequisites

Devices must be running **Sense version 10.8798.25857.1000 or later** and have the required Windows update installed.

Microsoft lists the following prerequisite updates, or later:

- Windows 11 24H2: KB5062660, 2025-07 Cumulative Update Preview
- Windows 11 23H2: KB5062663, 2025-07 Cumulative Update Preview
- Windows 11 22H2: KB5062663, 2025-07 Cumulative Update Preview
- Windows 10 22H2: KB5062649, 2025-07 Cumulative Update Preview
- Windows 10 1809: KB5063877, 2025-08 Cumulative Update
- Windows Server 2019: KB5063877, 2025-08 Cumulative Update
- Windows Server 2022: KB5063880, 2025-08 Cumulative Update
- Windows Server 2025: KB5063878, 2025-08 Cumulative Update

## Restarts and rollback

Another good detail: these EDR updates typically do not require a device restart.

Microsoft mentions that a restart may be required only in rare failure scenarios.

If a rollback is needed, administrators can use the Microsoft Defender command-line utility.

To revert EDR to the inbox version stored in `%ProgramFiles%\Windows Defender Advanced Threat Protection`:

```powershell
MpCmdRun.exe -RevertMde -Product Edr -ToVersion Inbox
```

To revert EDR to the previous version, if there is an available backup in `%ProgramData%\Microsoft\Windows Defender Advanced Threat Protection\Platform`:

```powershell
MpCmdRun.exe -RevertMde -Product Edr -ToVersion Previous
```


## Sources:
- [MC1381119 - Microsoft Defender for Endpoint security updates move to Microsoft Update on Windows](https://mc.merill.net/message/MC1381119)
- [Microsoft Defender for Endpoint EDR Updates are now Separate from Monthly Windows Security Updates for Faster Protection](https://www.anoopcnair.com/microsoft-defender-for-endpoint-edr-updates-are/)
