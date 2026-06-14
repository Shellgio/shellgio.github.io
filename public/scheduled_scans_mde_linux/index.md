# Scheduled scans are coming to AV policies in Defender for Endpoint on Linux


One of the things I've always missed when deploying antivirus policies for **Microsoft Defender for Endpoint** on **Linux** devices were **scheduled scans**.

And I said 'were' because they are coming. Currently in preview, now you can set up scheduled scans settings used a custom JSON setting or, if you use Intune or security settings management an antivirus policy instead of using a cron job like until now.
{{< image src="/images/image.png" caption="Screenshot of the new settings on a `Antivirus Intune Policy` for Linux" src_s="/images/image.png" src_l="/images/image.png" >}}

Flexible scheduling options include hourly, daily, and weekly scans with configurable scan types and advanced options to control scan behavior.

To be able to use it **you need agent version 101.26032.0000 or later** (production ring), and **configure scheduled scans** using **managed JSON** or **Defender AV policies** using Intune or security settings management.

# Sources
- [Introducing scheduled antivirus scans on Microsoft Defender Linux](https://techcommunity.microsoft.com/blog/microsoftdefenderatpblog/introducing-scheduled-antivirus-scans-on-microsoft-defender-linux/4524578)






