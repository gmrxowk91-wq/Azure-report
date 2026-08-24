# Azure Data Fundamentals — Data Technician Bootcamp (Week 5)

Cloud data platform concepts, UK data legislation, and a full **Microsoft Azure migration proposal** written as a business report. Completed during Week 5 of a Data Technician bootcamp.

---

## Overview

| | |
|---|---|
| **Topics** | Cloud service and deployment models, UK computing and data law, Azure data services |
| **Tools** | Microsoft Azure (hands-on labs), Power BI |
| **Main deliverable** | 1,500–2,000 word Azure migration report for a retail scenario, with ERD and cited sources |
| **Duration** | 1 week (4 days) |

---

## Skills demonstrated

### Cloud service models

| Model | What it is | Where it fits |
|---|---|---|
| **IaaS** | Renting virtual machines, storage, networking and operating systems, managing everything above the hardware | Lift-and-shift migration of existing on-premises applications; high-control environments |
| **PaaS** | Provider manages infrastructure and runtime; you deploy code | Web apps and managed databases — Azure SQL Database, Azure App Service |
| **SaaS** | Fully managed application accessed in a browser | Microsoft 365, Google Workspace, Salesforce |

### Cloud deployment models

| Model | Appropriate when | Example |
|---|---|---|
| **Public** | General workloads, rapid scaling, cost optimisation, no data-residency constraint | Netflix on AWS |
| **Private** | Strict security, regulatory compliance, full control, predictable high-performance workloads | A bank holding client financial records |
| **Hybrid** | Sensitive workloads isolated privately, burst capacity taken from public cloud | Retailer keeping payment data private while scaling the storefront at peak |
| **Community** | Related organisations sharing infrastructure cost and a common compliance standard | Government bodies such as the NHS or police |

Also covered: the business case for cloud (no server purchase or maintenance, access independent of location) and the alternatives — on-premises hardware and edge computing.

### UK legislation and compliance

Researched, cited and applied to practical scenarios:

**Computer Misuse Act 1990** — the three principal offences:

| Offence | Example |
|---|---|
| Unauthorised access to computer material | Using a colleague's login credentials without permission |
| Unauthorised access with intent to commit further offences | Breaching a financial server to steal card details or divert funds |
| Unauthorised acts with intent to impair operation | Deploying malware or ransomware to corrupt files |

**Police and Justice Act 2006** — three powers added to the 1990 Act:

- **Section 3A** — creating, adapting, supplying or obtaining "hacking tools" intended or likely to be used in a computer misuse offence
- A broadened **Section 3** explicitly criminalising **denial-of-service** attacks, covering recklessness as well as intent
- **Increased sentences** — unauthorised access from 6 months to 2 years; impairing a system from 5 to 10 years

Also covered: **Data Protection Act 2018 / UK GDPR**, **Copyright, Designs and Patents Act 1988**, **Copyright (Computer Programs) Regulations 1992**, **Health and Safety (Display Screen Equipment) Regulations 1992** and the **Consumer Rights Act 2015** — including an exercise matching legal clauses to the correct Act, and identifying which stated activity was *not* unlawful (penetration testing tools used lawfully).

Employment data practice: which employee data an employer may hold by default (name, address, date of birth) versus which requires explicit consent (race and ethnicity, religion, genetic data).

### Azure data services

Recommended and justified against a business requirement, not listed in the abstract:

| Service | Role in the architecture |
|---|---|
| **Azure Blob Storage** | Raw and unstructured landing layer — CSV exports, JSON from APIs, historical sales files, product images |
| **Azure Data Lake Storage Gen2** | Large-volume historical data for analytics |
| **Azure SQL Database** | Structured operational data — customers, products, orders, order items, inventory, categories |
| **Azure Data Factory** | Scheduled and event-triggered ETL pipelines automating movement and transformation |
| **Azure Synapse Analytics** | Warehousing and large-scale analysis — sales trends, seasonality, inventory turnover, product performance |
| **Azure Machine Learning** | Deferred future capability — demand forecasting, repeat-purchase likelihood, anomaly detection |
| **Power BI** | Management dashboards over the Azure layer |
| **Azure Backup / Site Recovery** | Backup and cross-region replication for disaster recovery |
| **Azure Key Vault** | Secrets, keys and credential storage |

---

## Main deliverable — "Paws & Whiskers" Azure migration report

A formal report (not an essay) written for a line manager, proposing a migration path for a growing pet shop currently running on manual data collection and spreadsheets.

### Problem statement

Spreadsheet-based operations become progressively harder to manage at scale — data duplicates, drifts out of consistency, resists analysis, and is vulnerable to loss and unauthorised access.

### 1. Legal and regulatory position

**UK GDPR** — the report works through all seven data protection principles (lawfulness, fairness and transparency; purpose limitation; data minimisation; accuracy; storage limitation; integrity and confidentiality; accountability) and translates each into a concrete obligation: collect only what is necessary, explain why it is collected, keep it accurate, set real retention periods rather than keeping data indefinitely, protect against unauthorised access and loss, and keep records demonstrating compliance. Data subject rights (access, rectification, erasure, restriction) and the **72-hour ICO breach notification** requirement are both addressed.

**Data Protection Act 2018** — positioned alongside UK GDPR, with the recommendation that policies be reviewed against **current ICO guidance** rather than settled procedure, given that the **Data (Use and Access) Act 2025** has altered the framework and some ICO guidance is under review.

**PCI DSS** — the recommendation is explicitly *not* to store full card details in the Azure environment. A PCI-compliant provider processes the transaction and the business retains only a transaction reference. Sensitive authentication data such as CVV/CVC must not be retained after authorisation.

### 2. Data model

Six entities designed with primary and foreign keys, documented with an ERD:

| Table | Key fields |
|---|---|
| **Customers** | Customer ID (PK), name, date of birth, telephone, address |
| **Sales** | Sales ID (PK), date, total amount, Customer ID (FK) |
| **SoldItems** | Links a sale to the products in it, updating inventory |
| **Inventory** | Product ID (PK), name, category, price, stock |
| **Pets** | Pet ID (PK), name, date of birth, Customer ID (FK), Care Plan ID (FK) |
| **CarePlans** | Care Plan ID (PK), tier, price, duration |

The `SoldItems` table is the deliberate addition — it resolves the many-to-many between sales and inventory and is what makes stock decrement correctly per line item rather than per transaction.

### 3. Storage formats

| Format | Used for | Reason |
|---|---|---|
| **CSV** | Raw spreadsheet imports and exports | Simple, universally supported |
| **JSON** | Customer profiles, application data | Flexible structure for varying fields |
| **Parquet** | Analytics and reporting | Columnar, highly compressed, faster queries at volume |
| **SQL tables** | Operational data | Frequent updates and retrieval |

### 4. Security

- **Encryption at rest** — AES-256 applied automatically to stored data
- **Encryption in transit** — HTTPS and TLS
- **Role-Based Access Control (RBAC)** — access limited to what a job role requires
- **Multi-Factor Authentication** on user accounts
- **Azure Key Vault** for passwords, encryption keys and credentials

### 5. Continuity and growth


**Azure Backup** for automated backup and recovery from accidental deletion, cyberattack or system failure; **Azure Site Recovery** for cross-region replication to limit downtime. Storage, database performance and analytics capacity scale without infrastructure redesign as the business grows.

### Sources cited

Information Commissioner's Office (UK GDPR guidance, data protection principles, personal data breaches), legislation.gov.uk (Data Protection Act 2018, Data (Use and Access) Act 2025), PCI Security Standards Council (PCI DSS, card verification code FAQ), and Microsoft Learn documentation for each Azure service referenced.

---

## Hands-on labs

Five **DP-900** labs completed in the Azure portal via a hosted lab environment, covering relational, non-relational, analytics and visualisation workloads:

| Lab | Title | Progress |
|---|---|---|
| Lab 01 | Explore Azure SQL Database | 100% |
| Lab 02 | Explore Azure Storage | 100% |
| Lab 03 | Explore Azure Cosmos DB | 96% |
| Lab 04b | Explore data analytics in Microsoft Fabric | 76% |
| Lab 06 | Fundamentals of data visualisation with Power BI | 93% |

### Lab 01 — Explore Azure SQL Database

Provisioned an Azure SQL Database (`Dealership` on server `sqlsever123`, resource group `RG1`, **UK South**), then built and queried a two-table relational schema entirely from the portal's query editor.

```sql
CREATE TABLE Manufacturer
(
    ManufacturerID   INT           PRIMARY KEY,
    ManufacturerName NVARCHAR(50)  NOT NULL,
    Country          NVARCHAR(50)
);

CREATE TABLE Vehicle
(
    VehicleID     INT PRIMARY KEY,
    ModelName     NVARCHAR(50),
    ManufacturerID INT,
    ModelYear     INT,
    BodyType      NVARCHAR(50),
    ListPrice     DECIMAL(10,2)
);
```

Populated with automotive sample data — four manufacturers (Toyota/Japan, Ford/United States, Volkswagen/Germany, Hyundai/South Korea) and their models — then joined across the foreign key:

```sql
SELECT  v.ModelName,
        m.ManufacturerName,
        m.Country,
        v.ListPrice
FROM Vehicle AS v
INNER JOIN Manufacturer AS m
    ON v.ManufacturerID = m.ManufacturerID
ORDER BY m.ManufacturerName;
```

Returned 8 rows across 4 columns. One error worth recording: re-running the `CREATE TABLE` block returned *"There is already an object named 'Manufacturer' in the database"* — a reminder that DDL is not idempotent by default, and that a re-run needs `DROP TABLE IF EXISTS` or `IF NOT EXISTS` guards rather than a second execution.

Cleanup: deleted the resource group to remove the database, server and everything inside it in one step — the practical habit that stops a lab environment quietly accruing cost.

### Lab 02 — Explore Azure Storage

Created a storage account (`dp900store123`, resource group `dp900-lab-rg`, **East US**) and explored the three ways Azure stores non-relational data: **blob storage**, **Data Lake Storage Gen2** and **Azure Files**. This is the practical grounding for the Blob Storage / Data Lake recommendations made in the report above.

### Lab 03 — Explore Azure Cosmos DB

Created a Cosmos DB account (`dp900-cosmos-123`) with `SampleDB` → `SampleContainer`, added JSON items through Data Explorer and queried them with a SQL-like language:

```json
{
    "name": "Road Helmet,45",
    "id": "123456789",
    "categoryId": "123456789",
    "SKU": "AB-1234-56",
    "description": "The product called \"Road Helmet,45\"",
    "price": 48.74
}
```

The instructive part is what Cosmos DB adds on save — system properties `_rid`, `_self`, `_etag`, `_attachments` and `_ts` appear automatically alongside the authored fields. A schemaless store still maintains internal metadata; "no fixed schema" is not the same as "no structure".

### Lab 04b — Explore data analytics in Microsoft Fabric

Built an ingestion pipeline in **Microsoft Fabric** — a copy job moving the **NYC Taxi (Green)** sample dataset in **Parquet** format into a Fabric **Lakehouse** (`dp900` workspace, root folder `Tables`), landing as a `taxi_rides` table under `Tables > dbo`.

Noted during the run: because the source is Parquet, the *Rows read* and *Rows written* counters can both report `0` while the table is in fact created and populated. A progress counter is not a verification — the table contents are.

### Lab 06 — Fundamentals of data visualisation with Power BI

A retail `Sales Report` built in Power BI Desktop over a three-table model — `customers` (City, CountryRegion, CustomerID, Name, PostalCode), `orders` (CustomerID, OrderDate, OrderItemID, ProductID, Quantity, Revenue) and `products` (Category, ProductID, ProductName).

The report combined a **pie chart** of quantity by product category, a **column chart** of revenue by product, and a **map visual** keyed on `City` with revenue as bubble size, plus page-level filters on city and revenue.

**On the map visual:** the lab's own step 1 requires enabling *Use Map and Filled Map visuals* under Options → Global → Security, and that setting was ticked as instructed. The visual nonetheless reported map visuals as disabled and rendered a placeholder. The field wells were configured correctly — `City` on Location, `Sum of Revenue` on Bubble size — so this is an environment restriction that survived the documented workaround, not an incomplete configuration.

> The full Power BI work across the course, including the PL-300 labs, is documented in the **[Power BI repository](https://github.com/gmrxowk91-wq/Power-BI-report)**.

---

## Repository contents

```
.
├── README.md
├── Paws_and_Whiskers_Azure_Proposal.pdf   # the full report
├── erd/                                    # entity relationship diagram
└── screenshots/                            # Azure and DP-900 lab evidence (8 images)
```

---

## Key takeaways

- Recommending a service is the easy half; justifying it against a stated business requirement and a legal constraint is the work
- Sequence matters — establish reliable, well-structured data before reaching for machine learning, or the model learns the mess
- The safest way to handle payment card data is to never hold it; delegate to a compliant processor and keep only a reference
- Compliance is a moving target: the Data (Use and Access) Act 2025 changed the UK framework, so citing current regulator guidance beats citing a settled textbook
- A backup that has never been restored is an assumption, not a backup
