# 🍜 Roll'd Vietnamese

Engineering platform for [Roll'd](https://rolld.com.au) — centralising operational data, automating workflows, and powering business intelligence across 100+ stores.

## 📦 Repos

| Repo | Description |
|------|-------------|
| [rolld-data-platform](https://github.com/Roll-d-Vietnamese/rolld-data-platform) | AWS data pipeline — ingests data from Tanda, Abacus, CCTV into S3, transforms to Parquet, queries via Athena |
| [rolld-dashboard](https://github.com/Roll-d-Vietnamese/rolld-dashboard) | Superset BI dashboards — KPI scorecards, leadership coverage, clock compliance |
| [rolld-catering](https://github.com/Roll-d-Vietnamese/rolld-catering) | Automated catering email processor — SES + Claude Haiku parse orders and reply to customers |
| [rolld-wiki](https://github.com/Roll-d-Vietnamese/rolld-wiki) | Engineering wiki — architecture, data schemas, guides, runbooks |

## 🏗️ Architecture

```
Source Systems              Ingestion                    Query & BI
──────────────              ─────────                    ──────────

Tanda (workforce)       ──> Lambda + Step Functions ──>
Abacus (POS/finance)    ──> Lambda + EventBridge    ──>  S3 Parquet ──> Glue ──> Athena ──> Superset
CCTV (Uniview NVR)      ──> Raspberry Pi + Lambda   ──>

Catering emails         ──> SES + Lambda + Claude Haiku ──> Auto-reply to customer
```

## ⚙️ Tech stack

AWS (Lambda, S3, Athena, Glue, Step Functions, EventBridge, SES) | Python | Terraform | Superset | Claude AI
