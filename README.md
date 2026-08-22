# FC Highbury — BI & CRM Consulting Project

Group project (3 members) for the *Business Intelligence and Business Analytics* module: a
consulting engagement for a fictional football club, "FC Highbury", that wanted to identify growth
opportunities and modernise how it tracks fans, finances and squad data. Final report title: *FC
Highbury Business Intelligence & Analytics Solutions*.

**My contribution (Subhash — 1 of 3 members, ~33% of the work):** CRM implementation in Salesforce,
the Fans & Operations Power BI dashboard, and the AI-generated dashboard comparative analysis
(full breakdown below). Team dashboard (Arun) and Financial/Management dashboard + specification
report (Cariappa) are also in this folder for context but were not my individual work.

## Business context

FC Highbury has ~€210M revenue over 5 seasons but sits below the "medium club" threshold (~€200M/yr)
and earns only ~22% of income from kits/sponsorship vs. ~40% for comparable clubs. No CRM existed
to track fans, bookings, or support tickets. Five business goals were set, spanning matchday
attendance, commercial revenue share, fan retention, operating cost, and ticket/membership revenue —
see the report for the full target table.

## My work in detail

### 1. Salesforce CRM implementation
Built to close the "no CRM, fan data untracked" gap identified in the analysis.
- **4 custom objects:** Region, Member, Matchday Hospitality Booking, Ticket — linked via
  lookup relationships (a Member belongs to one Region; a Member can have many Bookings/Tickets).
- **Validation rules:** guest count bounds and no-confirming-past-matches on Bookings; no
  future-dated birth/join dates on Members.
- **Automation (Salesforce Flows):** a Booking-Confirmed flow triggered on status change, and a
  High-Value Booking Alert flow that posts to a Chatter group and creates a follow-up task when a
  booking exceeds a value threshold — removing the need to manually monitor the booking list.
- **Reporting:** filtered list views plus 3 summary reports (Bookings by Package Tier, Members by
  Region, SLA Compliance by Month) combined into one operational dashboard.
- **Result:** SLA compliance rose from 3/9 tickets resolved on time in January to 9/9 in April
  (33% → 100%), and average resolution time fell from 27.1 to 8.6 hours.
- Screenshots of the full build are in `CRM/` (24 images: object manager, custom fields, validation
  rules, flows, Chatter, list views, reports, dashboard).

### 2. Fans & Operations Power BI dashboard
Addresses the operational cost-control and fan/merchandise-engagement gaps.
- Operating spend breakdown by category (electricity, scouting fees, stadium rent/lease are the
  largest lines) — used to support a supplier-diversification recommendation.
- Merchandise profit by acquisition channel, country, customer loyalty tier and category — kits/
  footwear drive ~78% of merchandise profit; Asia-Pacific and Americas together contribute ~53%,
  flagged as underexploited markets; organic search outperforms influencer collabs by a wide margin.
- File: `dashborads/operations/operational expensed.pbix` (data: `Merged_Club_Inventory_Orders_2021_2025.xlsx`).

### 3. AI-generated dashboard — comparative analysis
Built a fourth dashboard using an AI tool over the same combined dataset (finance, retail, squad,
membership, CRM) and compared it against the three manually-built dashboards.
- The AI dashboard produced a revenue forecast (9.4% CAGR → £2.84M in 2026, £3.11M in 2027) and a
  squad-value projection (£415.5M by 2027/28) that weren't in any manual workbook.
- It correctly reproduced the CRM's SLA figures (33% → 100%, 27.1 → 8.6 hrs) independently from the
  raw ticket data — evidence it aggregated genuine source data rather than guessing.
- It surfaced an insight the manual dashboards missed entirely: net income has fallen every season
  since 2023/24 and turned negative (–£5.4M) in 2025/26 due to transfer spending — invisible in the
  manual dashboards because they were built as three separate files by three different people with
  no cross-referencing.
- Conclusion drawn: AI is faster and better at cross-dataset synthesis, but the manual dashboards
  still carry data-quality fixes (see below) an AI tool wouldn't have caught without that
  transparency. Recommendation was to use both in combination, not one or the other.

### Data-quality fixes I made
While building the Injury Risk Score (bridging squad data with a Premier League injury reference
dataset via playing position), I found the positional injury-frequency values actually ranged
1.5–4.0 rather than the assumed 0–1 scale, which would have flagged every player as high-risk. Fixed
by min–max normalising before use in the weighted score. Also cleaned inconsistent position labels,
text-encoded substitute ratings (e.g. `"6(S)"`), and open injuries recorded as `"Present"` instead of
a return date (converted to proper nulls). On the CRM side, found and removed a duplicated Region
record and fixed an invalid merge-field token in the High-Value Booking Alert flow during testing.

## Rest of the team's work (for context)

- **Team (Player) dashboard** (Arun): Injury Risk Score vs. market value, minutes-played/rotation
  analysis, full on-ball stats table. File: `dashborads/palyers/players.pbix`.
- **Financial (Management) dashboard + Specification Report** (Cariappa): income/expense/squad-value
  trends, GAP/SWOT analysis, system architecture. File: `dashborads/finances/finances.pbix`.

## Data sources

11 datasets combined into a star schema (fact table `FactSales`, dimensions `DimCustomer`,
`DimProduct`, `DimRegion`, `DimDate`): financial data from Mockaroo, merchandise sales and customer/
product/region/date dimensions from an open academic GitHub repo, player stats from Mockaroo, a
real Premier League player-injuries dataset and stadium-supply dataset from Kaggle, and real Sorare
NFT sales data pulled via the CryptoSlam API (used to evaluate — and rule out — an NFT revenue
stream, since sales were trending down).

## Folder structure

```text
Fotb Club BIBA/
├── BIBA_report_final.pdf        # full specification + implementation report (26 pages)
├── Work_Split.docx                # team task allocation (33/33/34%)
├── presentation video.url         # https://youtu.be/D3EeBpRjEQg
├── CRM/                           # 24 Salesforce build screenshots (my work)
├── all datasets/                  # raw CSV/XLSX source files
└── dashborads/
    ├── finances/                  # Cariappa's Power BI dashboard
    ├── operations/                # my Power BI dashboard
    └── palyers/                   # Arun's Power BI dashboard
```

## How to view

Power BI dashboards (`.pbix`) open in Power BI Desktop. Salesforce CRM build is documented via the
screenshots in `CRM/` (no live org attached). Full write-up and figures are in
`BIBA_report_final.pdf`; walkthrough video is linked above.
