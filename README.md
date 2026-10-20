# SQL vs. NoSQL: Put Database Performance to the Test

**One dataset. Two database engines. A closer look at the trade-offs.**

Explore how **Microsoft SQL Server** and **MongoDB** handle reads, aggregations, joins, and bulk writes with Apache JMeter. Built around Stack Overflow data, this project turns a familiar architecture debate into a hands-on performance investigation.

Originally developed by **Mariano Carreira** for the Database course in the **Master's in Software Engineering at Universidad Nacional de La Plata (UNLP)**, this English edition brings the experiment, its findings, and its limitations into a practical engineering portfolio.

## What makes this project useful

- **Real application data:** a Stack Overflow dataset, with a largest reported scenario of more than 17 million posts.
- **Workload variety:** selection, grouping, joins, complex queries, index-oriented lookups, and 500,000-record insert operations.
- **Two read passes:** explore how repeated execution changes observed response times.
- **Inspectable assets:** SQL scripts, MongoDB scripts, and an editable JMeter plan.
- **Transparent interpretation:** historical measurements are separated from claims that require further testing.

## What the original experiment found

MongoDB recorded lower times for several broad selection queries. SQL Server recorded lower times for joins and many aggregations in the relationally shaped dataset. Outcomes varied by query, dataset size, and execution pass.

These are observations from the original academic report, not a universal ranking or a new benchmark run. The checked-in plan uses **one thread and two loops**; higher-concurrency stress testing requires additional configuration and validation. Some SQL and MongoDB samplers consume results differently, limiting direct comparisons.

**Start here:** [Study and findings](docs/STUDY.md) · [Setup guide](docs/GETTING_STARTED.md) · [Workload catalog](docs/WORKLOADS.md) · [Methodology and limitations](docs/METHODOLOGY.md) · [Historical measurements](results/historical-results.csv)

## Repository map

| Path | Purpose |
| --- | --- |
| `Jmeter/Jmeter_SQLvsMongo.jmx` | Original workload logic with English labels and configurable connections |
| `Scripts/SQL/` | Standalone T-SQL workloads |
| `Scripts/MongoDb/` | Standalone MongoDB workloads |
| `docs/` | English study, setup, workload mapping, and methodology |
| `results/` | Historical measurements transcribed from the original report |

## Run the experiment

1. Read the [setup guide](docs/GETTING_STARTED.md) and provision disposable databases.
2. Import the dataset into both engines and verify the data mapping.
3. Install the required JDBC and MongoDB Java dependencies in JMeter.
4. Configure credentials and validate a small workload before running the full plan.
5. Export measurements and interpret them using the [methodology](docs/METHODOLOGY.md).

The repository contains benchmark assets rather than a turnkey environment: database backups, automated imports, schema provisioning, and driver binaries are not included.

## Take the investigation further

Validate result equivalence, standardize result consumption, compare a native document model, and introduce controlled concurrency. See the [research roadmap](docs/METHODOLOGY.md#research-roadmap).

## Author and provenance

**Mariano Carreira · 2024**
Original project: [DB-Sql_Vs_NoSQL](https://github.com/marianocarreira/DB-Sql_Vs_NoSQL)

This edition rewrites and condenses the original Spanish academic report into English documentation. It preserves the experimental scripts and historical figures while making methodological gaps explicit. It is not a page-by-page translation of the original report.

No license file was supplied by the original repository. This edition does not introduce a new license or change ownership.
