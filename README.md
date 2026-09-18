# FC Highbury — BI & CRM Consulting Project

Group project (3 members) for the Business Intelligence and Business Analytics module: a consulting
engagement for a fictional football club, FC Highbury, covering fan/CRM tracking, operating cost
control, and commercial revenue growth.

**My contribution (1 of 3 members, ~33%):** the Salesforce CRM implementation, the Fans and
Operations Power BI dashboard, and an AI-generated dashboard used as a comparison benchmark. The
Team and Financial dashboards in this repo were built by teammates Arun and Cariappa and are
included for context, not as my work.

## Business context

FC Highbury earns ~€210M over 5 seasons but sits below the "medium club" revenue threshold and
draws only ~22% of income from kits and sponsorship, versus ~40% at comparable clubs. No CRM
existed to track fans, bookings, or support tickets.

## What I built

**Salesforce CRM.** Four custom objects (Region, Member, Booking, Ticket) linked by lookup
relationships, with validation rules on guest counts and booking/member dates. Two automated
flows: one confirms bookings on status change, one posts a Chatter alert and creates a follow-up
task for high-value bookings. Three summary reports (bookings by package tier, members by region,
SLA compliance by month) roll up into one operational dashboard.
Result: SLA compliance rose from 33% to 100% (3/9 to 9/9 tickets on time), and average resolution
time fell from 27.1 to 8.6 hours. Build documented in `CRM/` (24 screenshots).

**Fans and Operations Power BI dashboard.** Operating spend by category, and merchandise profit by
channel, country, and customer tier. Kits and footwear drive ~78% of merchandise profit; organic
search outperforms influencer channels; Asia-Pacific and the Americas are flagged as underexploited
markets. File: `dashboards/operations/operational_expenses(subhash).pbix`.

**AI-generated dashboard comparison.** Built a fourth dashboard with an AI tool over the same
combined dataset and compared it against the three manual ones. It independently reproduced the
CRM's SLA figures from raw ticket data, produced a revenue forecast (9.4% CAGR) not present in any
manual workbook, and surfaced a net-income decline the three separate manual dashboards had missed
because they were never cross-referenced. Conclusion: AI is faster at cross-dataset synthesis, but
the manual dashboards carry data-quality fixes an AI tool would not have caught without that
context — the two approaches work best combined.

**Data-quality fixes.** Found that a player injury-frequency field used a 1.5–4.0 scale rather
than the assumed 0–1 scale (would have flagged every player as high-risk); corrected with
min-max normalisation before use in the weighted risk score. Cleaned inconsistent position labels,
text-encoded substitute ratings, and open injuries recorded as text instead of null dates. On the
CRM side, removed a duplicated region record and fixed an invalid merge-field token in the
high-value booking alert flow.

## Data sources

11 datasets combined into a star schema (fact table `FactSales`; dimensions for customer, product,
region, date): financial and player data from Mockaroo, merchandise/customer/product data from an
open academic dataset, a real Premier League injuries dataset and stadium-supply dataset from
Kaggle, and Sorare NFT sales data via the CryptoSlam API (used to evaluate, and rule out, an NFT
revenue stream).

## Folder structure

```text
Fotb Club BIBA/
├── report.pdf                # full specification and implementation report
├── Work_Split.docx           # team task allocation (33/33/34%)
├── presentation video.url    # recorded walkthrough
├── CRM/                      # Salesforce build screenshots (my work)
├── all datasets/             # raw CSV/XLSX source files
└── dashboards/
    ├── finances/              # Cariappa's dashboard
    ├── operations/            # my dashboard
    └── players/               # Arun's dashboard
```

## How to view

Power BI files (`.pbix`) open in Power BI Desktop. The Salesforce CRM build has no live org
attached — see the screenshots in `CRM/`. Full write-up in `report.pdf`.
