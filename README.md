# VPS Ubuntu guide: which SpinServers KVM plan to pick, 24.04 LTS or 26.04 LTS, and what the sticker price leaves out

SpinServers lists seven fixed KVM VPS configurations, from **2 vCPU / 2 GB RAM / 40 GB RAID-10 SSD with 4 TB of monthly transfer** up to a 32 vCPU / 32 GB machine with 500 GB of SSD and 20 TB. Every one of them comes with a 1 Gbps port, one IPv4 address and one IPv6 address.

For a single site, a small Docker stack or a CI runner, the 4 vCPU / 4 GB and 6 vCPU / 8 GB tiers are where most people should land. The 12 vCPU and 16 vCPU boxes only pay for themselves when you actually saturate them. On the OS side, install **Ubuntu 24.04 LTS** unless you specifically want the longest support runway, in which case 26.04 LTS keeps standard security updates until May 2031.

One caveat before the table. The public KVM VPS page publishes the hardware for all seven plans but only one price point, an advertised "starting from $62.00 USD monthly" attached to the featured configuration. The per-plan figure appears on the order page, so the table below gives you the specs to match against whatever number you see there.

> **Quick pick**
> - **Default choice:** Simple Application Server, 4 vCPU / 4 GB RAM / 80 GB RAID-10 SSD / 8 TB transfer. Enough room for a web server, a database and a few containers on one box.
> - **Cheapest way in:** 2 vCPU / 2 GB / 40 GB / 4 TB, which handles a static site, a VPN endpoint or a staging environment.
> - **Ubuntu release:** 24.04 LTS (updates to May 2029) or 26.04 LTS (to May 2031). Nothing older than 22.04.
> - 👉 [See the KVM VPS lineup and the price for each tier](https://bit.ly/Spinservers)

## The full SpinServers KVM VPS lineup

| Plan | vCPU / RAM | Storage | Monthly transfer | Price (USD/mo) | Order |
| --- | --- | --- | --- | --- | --- |
| Simple Application Server | 2 vCPU / 2 GB DDR4 | 40 GB RAID-10 SSD | 4 TB | Shown at checkout | [Order the 2 vCPU tier](https://bit.ly/Spinservers) |
| Simple Application Server | 4 vCPU / 4 GB DDR4 | 80 GB RAID-10 SSD | 8 TB | Shown at checkout | [Order the 4 vCPU tier](https://bit.ly/Spinservers) |
| Simple Application Server | 6 vCPU / 8 GB DDR4 | 120 GB RAID-10 SSD | 8 TB | Shown at checkout | [Order the 6 vCPU tier](https://bit.ly/Spinservers) |
| Simple Application Server | 8 vCPU / 8 GB DDR4 | 160 GB RAID-10 SSD | 10 TB | Shown at checkout | [Order the 8 vCPU tier](https://bit.ly/Spinservers) |
| Simple Application Server | 12 vCPU / 12 GB DDR4 | 160 GB RAID-10 SSD | 10 TB | Shown at checkout | [Order the 12 vCPU tier](https://bit.ly/Spinservers) |
| Simple Application Server (Intel Xeon) | 16 vCPU / 16 GB DDR4 | 320 GB RAID-10 SSD | 15 TB | Shown at checkout | [Order the 16 vCPU tier](https://bit.ly/Spinservers) |
| Advanced Application Server | 32 vCPU / 32 GB | 500 GB SSD | 20 TB | Shown at checkout | [Order the 32 vCPU tier](https://bit.ly/Spinservers) |

Two details from that table are worth flagging. The storage is described as RAID-10 SSD on the six Simple plans, while the Advanced plan is listed as 500 GB SSD without the RAID-10 wording, so ask if redundancy at the disk level is part of your decision. The bandwidth column says "monthly transfer" and does not say whether inbound traffic counts against it. Get that in writing before you pick a size based on the 4 to 20 TB range.

## Ubuntu 24.04 or 26.04? Work backwards from the support end date

Ubuntu LTS releases arrive every two years and each one gets five years of standard security maintenance, which is why the version you pick sets a deadline you'll live with. Interim releases get nine months, so they are a poor fit for a server you intend to leave running.

| Release | Standard security updates until | Verdict |
| --- | --- | --- |
| 26.04 LTS | May 2031 | Longest runway; pick it for a new build you won't touch for years |
| 24.04 LTS | May 2029 | The safe default: most guides, images and third-party repos are tested against it |
| 22.04 LTS | May 2027 | Fine if you're already on it, a short deadline for anything new |
| 20.04 LTS | Ended in 2025 | Not for new deployments |
| 18.04 LTS | Ended in 2023 | Not for new deployments |

A trap worth naming: plenty of providers still keep 18.04 and 20.04 in their image menus because old customers ask for them. Being installable is not the same as being patched. A box running 20.04 in late 2026 is receiving no security fixes for the base system unless you've bought extended coverage, and that is a strange thing to do with a brand-new VPS.

Upgrading later is possible with `do-release-upgrade`, and it generally works. The cleaner path across a gap of two or more LTS releases is to build the new server on 24.04 or 26.04, migrate your data and cut DNS over, keeping both machines alive for a week. You keep a rollback option instead of hoping an in-place upgrade finishes.

Whether SpinServers offers both 24.04 and 26.04 as install images is not stated on the KVM VPS page, so check the OS list during checkout. KVM guests can also boot an ISO, which means a release missing from the menu can still be installed by hand.

👉 [Start an order and check which Ubuntu images are available for your tier](https://bit.ly/Spinservers)

## What the monthly price doesn't include

Ubuntu itself costs nothing to license; you're paying for the virtual machine, the disk and the transfer. The extras are where quotes drift.

Backups and snapshots are usually billed separately across this market. Arct Cloud charges $0.04 per GB per month for snapshots and 20% of the server's monthly price for daily automatic backups. Timeweb Cloud bills backups at 6 ₽/GB/month and images at 3 ₽/GB/month, with one free snapshot per server that is deleted automatically after seven days. OVHcloud lists backup as a paid add-on at subscription time. A $6 VPS with 40 GB of daily backups is not a $6 VPS.

Traffic metering varies just as much. Arct Cloud counts outbound transfer only and treats inbound as free. SpinServers states a monthly transfer allowance per plan without specifying direction. If you run a backup target, a media host or anything that pulls large files in, the difference between "4 TB total" and "4 TB outbound" decides whether you stay inside the plan.

Then there is a restriction that is specific to SpinServers and easy to miss. **Port 25 (SMTP) is closed by default on all of its VPS**, and opening it requires a support ticket with a justification; the provider reserves the right to refuse. If you plan to run a mail server, or any application that delivers mail directly rather than handing it to a relay, that ticket has to be approved before the project works. A contact form that passes mail to an external relay is a different case, but confirm which outbound ports are open before you build around them.

The things that are included are the ones people forget to check elsewhere: one IPv4 and one IPv6 address on every plan, a 1 Gbps port, and full root access over SSH, which is the standard for this class of product.

## The renewal price is the number that matters

Introductory pricing is a normal structure in this market, and the gap can be large. Hostinger's Brazilian storefront advertises KVM 1 at R$29.99/month when you prepay 24 months as a single R$719.76 payment, and states automatic renewal at R$59.99/month for the same period. The other three tiers follow the same shape: R$1,031.76 upfront renews at R$77.99/month, R$1,439.76 renews at R$149.99/month, R$2,879.76 renews at R$259.99/month. Read the second number before you commit to the first.

SpinServers does not publish a renewal rate on its KVM VPS page. Before you prepay anything, open the cart and confirm what the second billing period costs, whether the plan is monthly or prepaid, and whether the advertised figure is a first-term price. If a page won't tell you, the checkout total usually will.

👉 [Compare the KVM VPS tiers side by side before you commit](https://bit.ly/Spinservers)

## How the entry tiers compare across providers

Prices below come from each provider's own storefront in its own currency, so don't convert them into a single number and declare a winner.

| Provider | Entry configuration | Advertised price | Billing |
| --- | --- | --- | --- |
| Arct Cloud | 1 vCPU / 1 GB / 15 GB NVMe / 1 TB outbound | $3.89/mo | Monthly |
| ColossusCloud | G5.1, 2 vCPU / 1 GB / 100 GB / 2 TB | $5.00/mo | Monthly or $0.007/hr |
| Timeweb Cloud | MSK Ultra 15, 1 core / 1 GB / 15 GB | 1,920 ₽/mo | Hourly or monthly |
| Hostinger | KVM 1 | R$29.99/mo on 24-month prepay, renews R$59.99/mo | 24-month prepay |

SpinServers doesn't compete at the very bottom of that list, and it doesn't try to. Its smallest plan is 2 vCPU with 2 GB of RAM and 40 GB of disk, which is closer to a 4 vCPU / 4 GB tier elsewhere. That's the fair comparison to make when you're weighing it: match cores, RAM and disk, then look at the price and the transfer allowance together.

Where it stands apart is the shape of the plans. Fixed configurations with generous transfer allowances and both IP versions included suit projects that expect steady outbound traffic. If you need 1 GB of RAM and nothing more, cheaper entry plans exist and will do the job.

## Picking your tier

- Static site, personal blog, WireGuard endpoint, staging copy: **2 vCPU / 2 GB**, 40 GB is enough disk for a LEMP stack and a few months of logs.
- One production site with a database and 3–6 containers: **4 vCPU / 4 GB** or **6 vCPU / 8 GB** if MySQL and a couple of Node services share the box.
- Multiple client sites, a self-hosted GitLab, or a build runner that compiles: **8 vCPU / 8 GB** and up.
- Anything past 12 vCPU: only if you can name the process that will use the cores. Idle cores are the most expensive thing on this list.

Two things to confirm at checkout, whatever tier you choose: the Ubuntu release you want is in the image list, and the transfer allowance counts outbound only or both directions. If you're planning mail, raise the port 25 ticket before you migrate anything.

👉 [Open the SpinServers order page and confirm the live price for your tier](https://bit.ly/Spinservers)
