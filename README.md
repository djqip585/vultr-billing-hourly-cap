# Vultr Hourly Billing: What the 672-Hour Cap Really Means, Why a Stopped Server Still Charges You, and How to Estimate a Real Monthly Bill (Every Plan Type Compared)

Vultr bills every server by the hour, with a one-hour minimum, and the meter starts the moment the instance is deployed, not when it starts handling traffic. Cloud Compute, Optimized Cloud Compute and Bare Metal stop accruing charges at 672 hours in a calendar month, which is 28 days. GPU instances and VX1 Cloud Compute run on a 730-hour model and are billed for the real number of hours in each month: 720 in a 30-day month, 744 in a 31-day month. And a stopped server keeps billing. Only a destroyed server stops.

What follows covers which plan type fits which job, the arithmetic behind 672 versus 730, the add-ons that quietly change the total, and the questions people ask most often before their first invoice lands.

## Which Vultr plan type should you open? (short answer)

Most readers land in one of five buckets:

- **A few hours of testing, a build job, a migration rehearsal** → the smallest Cloud Compute instance you can find, destroyed at the end. You pay for the hours it existed, rounded up to the hour.
- **A site or API that runs all month** → Cloud Compute or High Frequency Compute. The charge stops at the 672-hour cap regardless of how long the month is.
- **Workloads needing dedicated vCPU or a lot of RAM per core** → Optimized Cloud Compute, same 672-hour rule.
- **VX1 Cloud Compute** → 730-hour model, billed on the actual length of the month.
- **GPU work** → 730-hour model since 1 October 2024.

Hourly rates differ between data centers, and Vultr directs customers to the Control Panel for the price that applies to their account. 👉 [See the current plan list and hourly rates](https://bit.ly/VuLtr)

## Every Vultr product family and its billing rule

Plan names, configurations and prices vary by location and change over time, so the numbers you see at deploy time are the ones that count. The billing rules below apply across the board.

| Product family | Billing cap | What it suits | Billing detail worth knowing | Deploy |
| --- | --- | --- | --- | --- |
| Cloud Compute | 672 hrs/month | General web apps, dev and staging boxes | Hourly rate works out to monthly price ÷ 672 | [Check Cloud Compute rates](https://bit.ly/VuLtr) |
| High Frequency Compute | 672 hrs/month | CPU-bound apps that benefit from higher clock speeds | Same 672 cap, one-hour minimum | [Check High Frequency rates](https://bit.ly/VuLtr) |
| Optimized Cloud Compute | 672 hrs/month | Dedicated vCPU, memory-heavy or storage-heavy work | Same 672 cap | [Check Optimized Cloud Compute rates](https://bit.ly/VuLtr) |
| VX1 Cloud Compute | 730-hour model, actual hours billed | Vultr's newer Cloud Compute line | 720 hrs in a 30-day month, 744 in a 31-day month | [Check VX1 rates](https://bit.ly/VuLtr) |
| Cloud GPU | 730-hour model, actual hours billed | Training and inference | Moved from the 672 cap to the 730 model on 1 Oct 2024 | [Check GPU rates](https://bit.ly/VuLtr) |
| Bare Metal | 672 hrs/month | Whole-machine workloads | A powered-down server bills until you release it | [Check Bare Metal rates](https://bit.ly/VuLtr) |

## 672 hours versus 730 hours: 28 days versus a real month

672 is 28 days at 24 hours a day. A 30-day month contains 720 hours and a 31-day month contains 744, so once a standard instance passes hour 672 the meter simply stops for the rest of that month. The last 48 or 72 hours cost nothing. That is why a plan advertised at a monthly price behaves like a ceiling rather than a forecast.

GPU and VX1 plans work differently. Vultr divides the monthly price by 730 to derive the hourly rate, then bills the actual hours in each calendar month. Vultr's own example: a GPU plan that previously billed $0.179 per hour across 672 hours now bills $0.165 per hour across 730. Both routes land near the same monthly figure, roughly $120 in that case, only the hourly number looks cheaper.

The 730 is a rate divisor, not a hard stop. A 31-day month bills up to 744 hours on those products.

Where this bites is budgeting. If you assume every Vultr plan caps at 672 hours and size a GPU deployment against that number, your full-month estimate will come in low by about 9%.

## Stop versus destroy: the one line in the docs that saves the most money

> "To stop billing for a server, you must destroy it from your account. Stopping a server does not suspend billing."

A stopped instance still holds RAM, SSD storage, IP aliases and vCPU reserved for it, and Vultr charges for those reservations until the instance is destroyed. Powered-off Bare Metal behaves the same way until it is released. The hourly clock starts at deployment, not at boot, and it does not pause when the OS shuts down.

For anyone running temporary infrastructure, the practical sequence is: take a snapshot if you might need the state again, then destroy the instance. Snapshots are billed separately at $0.05 per GB per month, which is nearly always cheaper than leaving a stopped instance ticking for a week. 👉 [Start a test instance you can destroy when you're done](https://bit.ly/VuLtr)

## What else lands on the invoice

Hourly compute is the headline, not the whole bill.

**Automatic backups.** Enabling them raises the base monthly and hourly fee by 20%. On a long-running instance that is a permanent surcharge, so decide deliberately rather than leaving the toggle on by default.

**Bandwidth overage.** The monthly bandwidth allocation is spread across the month's hours, and Vultr applies an overage charge if you exceed your currently allocated bandwidth at any point during the month. A single spike at 3 a.m. can trigger it even when the monthly total looks healthy. Included free egress is 2 TB per month and does not roll over; beyond that, outbound traffic is $0.01 per GB worldwide.

**Snapshots.** $0.05 per GB per month, subject to change.

**Software licences.** cPanel is sold separately: Admin (5 accounts) at $31.99/month, Pro (30) at $45.99, Plus (50) at $55.99, Premier (100) at $63.99, and Premier + Bulk at $63.99 plus $0.47 per additional account.

**Object Storage,** if you use it alongside compute: Archival at $6/month, Standard at $18, Premium at $36, Performance at $50, Accelerated at $100, each including 1,000 GB of storage and 1,000 GB/month of egress, with per-GB overage rates that differ by tier.

**Tax.** Listed prices exclude VAT and sales tax, which appear as a separate invoice line. Rates depend on your country or US state, ranging from 5% in parts of the US and Pakistan to 20% in the UK and up to 27% in parts of the EU. Changing your tax location does not apply retroactively to invoices already finalised.

## How to estimate a real monthly bill

The method takes about two minutes:

1. Find the hourly rate for the exact plan and data center you want (monthly price ÷ 672; ÷ 730 for GPU and VX1).
2. Multiply by the hours the instance existed, with a one-hour minimum and capped at 672 for standard plans or the month's real hour count for 730-model products.
3. If automatic backups are on, multiply the compute portion by 1.2.
4. Add $0.05 per GB per month for any snapshots you keep.
5. Add $0.01 per GB for egress above the allowance.
6. Add local tax.

A worked example using round numbers: a plan listing at $24/month works out to about $0.036 per hour. Run it 50 hours for a migration test and destroy it, and you owe roughly $1.79. Leave the same instance stopped for the rest of a 30-day month and the charge keeps climbing toward the full $24.

Mid-cycle upgrades get their own line. Vultr bills each plan at its own hourly rate for the time it was active, so a single invoice can show more than 672 hours across line items. Total billed hours never exceed the actual hours in the month. 👉 [Check live pricing before you size the instance](https://bit.ly/VuLtr)

## FAQ

**I used a server for three hours. Will I be charged for a full month?**
No, as long as you destroy it. Charges accrue hourly until destruction, with a one-hour minimum, and stop at the 672-hour cap on standard plans.

**Does powering off the server stop the charges?**
No. Reserved compute, storage and IP resources keep billing until the instance is destroyed.

**Does a stopped Bare Metal server bill too?**
Yes, until it is released from your account.

**When is the invoice generated, and in what time zone?**
On the 1st of each month, covering the previous billing period. Billing periods have run on UTC since 1 January 2017.

**Is the price the same in every data center?**
No. Vultr states that prices can differ by location because of network, hardware, staffing and tax differences. The Control Panel shows the price for your account.

**Do sub-accounts get separate invoices?**
Sub-account charges roll up to the main account by default. Separate parent and child billing has to be requested through a support ticket.

**Which payment methods are accepted?**
Visa, Mastercard, American Express, Discover, JCB, PayPal, Alipay, UnionPay and BitPay cryptocurrencies including BTC, ETH, LTC and USDC.

## Picking the right model for your case

- Testing, one-off jobs, migration rehearsals → smallest suitable plan, a calendar reminder, destroy at the end. Snapshots at $0.05/GB/month are the cheap way to keep the state.
- Anything running all month → budget the monthly cap, then add 20% if backups are on.
- GPU or VX1 → expect 720 or 744 hours of billing in a full month, not 672.
- Bandwidth-heavy projects → watch the point-in-time overage rule rather than the monthly average.

Before you deploy, confirm the data center price in the Control Panel, decide whether backups are worth the 20%, check what your free egress covers, and remember that "stop" in the customer portal is a power state, not a billing switch. 👉 [Compare Vultr's plan types and open an account](https://bit.ly/VuLtr)
