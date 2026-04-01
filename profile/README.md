# 🍜 Roll'd Vietnamese

Engineering platform for [Roll'd](https://rolld.com.au) — centralising operational data, automating workflows, and powering business intelligence across 100+ stores in Australia.

## 📦 Repos

| Repo | Description | Status |
|------|-------------|--------|
| [rolld-data-platform](https://github.com/Roll-d-Vietnamese/rolld-data-platform) | AWS data pipeline — ingests data from Tanda, Abacus, CCTV into S3, transforms to Parquet, queries via Athena | ✅ Production |
| [rolld-dashboard](https://github.com/Roll-d-Vietnamese/rolld-dashboard) | Superset BI dashboards — KPI scorecards, leadership coverage, clock compliance | ✅ Production |
| [rolld-catering](https://github.com/Roll-d-Vietnamese/rolld-catering) | Automated catering email processor — SES + Claude Haiku parse orders and reply to customers | ✅ Production |
| [rolld-wiki](https://github.com/Roll-d-Vietnamese/rolld-wiki) | Engineering wiki — architecture, data schemas, guides, runbooks | 📚 Active |

## 🏗️ Architecture

```
Source Systems              Ingestion                    Storage & Catalog         BI
──────────────              ─────────                    ─────────────────         ──

Tanda (workforce)       ──> Lambda + Step Functions ──>
Abacus (POS/finance)    ──> Lambda + EventBridge    ──>  S3 Parquet ──> Glue ──> Athena ──> Superset
CCTV (Uniview NVR)      ──> Raspberry Pi + Lambda   ──>

Catering emails         ──> SES + Lambda + Claude Haiku ──> Auto-reply to customer
```

## 🚀 Ingestion Pipelines

**6 pipelines deployed** across 3 source systems:

| Source | Pipeline | Trigger | What it does |
|--------|----------|---------|-------------|
| 👥 Tanda | 8 workforce entities | EventBridge cron (daily 1am AEST) | REST API → JSON → Parquet via Step Functions orchestration |
| 💰 Abacus | Invoices (43 cols) | EventBridge S3 | Hourly POS CSV → normalised Parquet |
| 💰 Abacus | Stores | EventBridge S3 | Store dimension CSV → Parquet |
| 💰 Abacus | Forecasts | EventBridge S3 | Sale forecast CSV → Parquet |
| 📹 CCTV | Edge capture | Pi cron (configurable) | 4K snapshots from NVR via RTSP → S3 |
| 📹 CCTV | Gap detection | EventBridge S3 | Claude Vision (Sonnet) analyses cabinet fill → SNS alerts |

## 📊 Dashboards (Superset)

| Dashboard | Data | Purpose |
|-----------|------|---------|
| 🏢 Leadership Coverage | Schedules + shifts + users | Supervisor coverage by 15-min slot per store (historical + forward) |
| ⏰ Clock Compliance | Shifts + clock-ins | Clock-in/out compliance with smart back-to-back shift handling |
| 📈 KPI Scorecard | Shifts + schedules + departments | Daily/weekly store performance metrics |

All dashboards use **Row Level Security (RLS)** — standardised `State` and `Store` columns so users only see their assigned stores.

## 🍽️ Catering Automation

Automated email processor for catering orders:

1. Customer emails `bot@catering.rolld.com.au`
2. **Claude Haiku** classifies the email and extracts order details (boxes, delivery, state)
3. Calculates pricing from CSV price sheet (Box #1–#8 + delivery fee)
4. Looks up nearest store manager from directory
5. **Claude Haiku** drafts professional confirmation email
6. Sends reply via SES — under **$2/month** at ~50 orders/month

## 🗂️ Data Sources

### ✅ Active

| Source | Type | Entities | Storage |
|--------|------|----------|---------|
| **Tanda** | Workforce management | Shifts, schedules, users, leave, timesheets, departments, locations, clock-ins | S3 Parquet, Glue tables |
| **Abacus** | POS / financial | Invoices (hourly), stores, forecasts | S3 Parquet |
| **CCTV** | Visual monitoring | Camera snapshots, gap analysis | S3 JPEG + JSON |

### 🔮 Planned

Shopify, Mastercard/Tyro, Operandio, Google Analytics, Birdeye, Netsuite

## ⚙️ Tech Stack

| Layer | Technology |
|-------|-----------|
| 🐍 Language | Python (Lambda), SQL (Athena) |
| 🏗️ IaC | Terraform (workspace per environment) |
| 📥 Ingestion | AWS Lambda, EventBridge, Step Functions |
| 💾 Storage | S3 (raw + transformed zones, Snappy Parquet) |
| 📚 Catalog | AWS Glue Data Catalog (partition projection) |
| 🔍 Query | Amazon Athena ($5/TB scanned) |
| 📊 BI | Apache Superset (self-hosted) |
| 🤖 AI | Claude Haiku (catering), Claude Vision/Sonnet (CCTV) |
| 🔐 Secrets | AWS Secrets Manager |
| 📢 Monitoring | CloudWatch Alarms, SNS |

## 🌍 Environments

| Environment | Purpose | Region |
|-------------|---------|--------|
| `dev` | Personal sandbox | ap-southeast-2 |
| `staging` | Mirrors prod for integration testing | ap-southeast-2 |
| `prod` | Live, cost-monitored | ap-southeast-2 |
| `dev` (catering) | Catering email processor | us-east-1 |

All resources namespaced by environment — S3 buckets, Glue databases, Athena workgroups, Secrets Manager paths.

## 📖 Documentation

| Resource | Where |
|----------|-------|
| 🏗️ Architecture | [rolld-wiki/architecture](https://github.com/Roll-d-Vietnamese/rolld-wiki/blob/main/architecture/overview.md) |
| 📐 Data schemas | [rolld-wiki/data](https://github.com/Roll-d-Vietnamese/rolld-wiki/blob/main/data/tanda-schema.md) |
| 📝 ADRs (13) | [rolld-wiki/decisions](https://github.com/Roll-d-Vietnamese/rolld-wiki/blob/main/decisions/index.md) |
| 📋 Runbooks (5) | [rolld-wiki/runbooks](https://github.com/Roll-d-Vietnamese/rolld-wiki/blob/main/runbooks/index.md) |
| 🔀 dbt guide | [rolld-wiki/guides/dbt](https://github.com/Roll-d-Vietnamese/rolld-wiki/blob/main/guides/dbt.md) |
