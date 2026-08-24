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
