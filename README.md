# Full-Stack Business Intelligence (BI) Project: From Databricks Lakehouse to Power BI Reporting

--- 

## Executive Summary

This project simulates a real-world Business Intelligence (BI) initiative for a mid-sized retail organization struggling with fragmented reporting across ERP and CRM systems. Due to siloed data, leadership lacked a trusted, consistent view of KPIs across business functions.

To address this, I designed and delivered an end-to-end Full-Stack BI solution using a Databricks Lakehouse architecture and Power BI. The solution consolidates multi-source data into a governed analytical layer, applies business-aligned transformations, and enables role-based reporting for decision-makers. The project demonstrates how BI systems translate raw operational data into actionable business insights.

---

## Table of Contents

- [Problem Statement and Strategic Context](#problem-statement-and-strategic-context)
- [Business Objectives and Success Criteria](#business-objectives-and-success-criteria)
- [Solution Approach](#solution-approach)
- [Phase 1: Data Ingestion (Bronze Layer)](#phase-1-data-ingestion-bronze-layer)
- [Phase 2: Data Quality and Standardization (Silver Layer)](#phase-2-data-quality-and-standardization-silver-layer)
- [Phase 3: Business Modeling, Reporting and Decision Enablement (Gold Layer)](#phase-3-business-modeling-reporting-and-decision-enablement-gold-layer)
- [KPIs and Business Value Alignment](#kpis-and-business-value-alignment)
- [Estimated Benefits and Expected Outcomes](#estimated-benefits-and-expected-outcomes)
- [Data Quality Strategy and Key Challenges Addressed](#data-quality-strategy-and-key-challenges-addressed)
- [Limitations, Constraints and Design Trade-offs](#limitations-constraints-and-design-tradeoffs)
- [Future Enhancements Opportunities](#future-enhancements-opportunities)
- [Repository Structure](#repository-structure)
- [Key Deliverables and Navigation Links](#key-deliverables-and-navigation-links)

---

## Problem Statement and Strategic Context

The organization relied on multiple source systems for sales, customer, and product data. Leadership lacked timely visibility into critical questions such as revenue drivers, customer retention trends, and product-level profitability. Different teams interpreted metrics differently, which slowed decision-making and increased the risk of acting on incomplete or conflicting information.

From a strategic standpoint, the organization needed a centralized, trusted data foundation that could support consistent reporting, reduce manual effort, and enable stakeholders to focus on insights rather than data reconciliation.

---

## Business Objectives and Success Criteria

**Primary Objectives**
- Consolidate ERP and CRM data into a unified analytical model
- Standardize KPI definitions across sales, customer, and product teams
- Enable leadership to analyze performance drivers and trade-offs

**Success Criteria**
- Clean, reliable Silver-layer datasets with consistent definitions
- Business-ready Gold views supporting a star-schema model
- Power BI reports that answer stakeholder questions without manual work

---

## Solution Approach

The solution follows a Lakehouse + BI architecture using Databricks and Power BI, structured around three layers/ phases:
- Bronze Layer: Raw ingestion of source data
- Silver Layer: Data quality fixes and standardization
- Gold Layer: Business transformations, dimensional modeling, and reporting views.

Data Pipeline orchestration is setup and Power BI connects directly to the Gold layer, ensuring reports are always driven by governed, business-aligned data. This phased approach ensured that technical implementation remained directly tied to business outcomes, while also allowing each layer to build logically on the previous one.

[Modern Data Lakehouse Architecture Diagram](https://github.com/nitinskunigal/Full-Stack-BI---Databricks-Lakehouse-to-Power-BI-Reporting/blob/main/docs/data_lakehouse_architecture.png)

*This diagram illustrates the full Data Lakehouse Architecture and how data flows across layers in Databricks with data pipeline orchestration. The architecture shows how fragmented ERP and CRM data was transformed into a single, business-ready source of truth powering analytics and executive reporting.*

## Phase 1: Data Ingestion (Bronze Layer)

The first phase focused on establishing a reliable data foundation. Raw ERP and CRM datasets were ingested into the Bronze layer without applying business logic. This preserved source-level fidelity while creating a repeatable ingestion pattern suitable for scale.

**Key outcomes:**
- Centralized raw data from six source files across CRM and ERP
- Clear separation between ingestion and transformation responsibilities
- Foundation for traceability and downstream validation.

This phase mirrors real-world practices where raw data is preserved for auditability and future reprocessing.

## Phase 2: Data Quality and Standardization (Silver Layer)

The Silver layer focused on making data usable, not analytical. Here, I addressed common data quality issues such as inconsistent formatting, duplicates, missing values, invalid dates, and non-standard codes. Transformations were applied table-by-table, keeping logic transparent and auditable.

From a business perspective, this phase ensured that downstream analysis would not be distorted by data inconsistencies.

**Key actions:**
- Normalized customer, product, sales, and reference data
- Applied deduplication and latest-record logic where needed
- Validated correctness through targeted data quality checks.

This layer intentionally avoided aggregations or KPIs, maintaining a clean separation between data preparation and business logic.

## Phase 3: Business Modeling, Reporting and Decision Enablement (Gold Layer)

The Gold layer represents the business-ready analytical model.
Cleaned Silver datasets were integrated into dimensional views following a star schema. Business rules were applied to derive meaningful attributes and metrics aligned with stakeholder needs.

**Deliverables included:**
- Customer, Product, and Sales views designed for analytics
- Reporting views that support segmentation, lifecycle analysis, and performance trends
- A governed foundation optimized for Power BI consumption.

**Power BI dashboards were built on top of these Gold views, enabling:**
- Executive monitoring of revenue and performance drivers
- Customer segmentation and retention analysis
- Product performance comparison and pricing trade-off evaluation.

Importantly, Power BI acts as a consumption layer, not a transformation layer.

---

## KPIs and Business Value Alignment

The BI solution supports consistent, business-aligned KPIs. Each KPI is derived from the Gold layer, ensuring a single definition across all reports and stakeholders.

Key KPIs included:

- Revenue and Transaction Trends – Assess overall business performance and growth sustainability.
- Average Order Value (AOV) – Evaluate revenue quality beyond transaction volume.
- Customer Segmentation and Recency – Understand demand drivers and customer acquisition patterns.
- Product Performance and Contribution – Identify high and low margin products for pricing decisions.

---

## Estimated Benefits and Expected Outcomes

If implemented in a real organization, this solution would:

- Reduce manual reporting and reconciliation efforts
- Improve trust in leadership dashboards
- Enable faster, data-driven decision-making
- Support scalable analytics as data volumes grow

The project demonstrates how BI systems shift analytics from reactive reporting to proactive decision support.

---

## Data Quality Strategy and Key Challenges Addressed

Data quality challenges were addressed systematically rather than reactively. The Silver layer was designed as the quality assurance layer of the lakehouse, where raw operational data was cleaned, standardized, and validated before being used for analytics and decision-making. The primary objective at this stage was not aggregation or reporting, but building trust in the data.

During ingestion, both ERP and CRM source files presented several quality challenges that are common in real business systems. These included inconsistent formatting, duplicate records, missing or null values, and non-standard codes for key attributes such as gender, country, and product classifications. If left unresolved, these issues could directly impact business metrics like customer counts, revenue attribution, and profitability analysis.

To address this, I applied systematic data standardization rules across entities. For example, customer attributes such as gender and marital status were normalized into consistent, business-readable values, and country codes were aligned to avoid fragmented regional reporting. Duplicate customer records were resolved by retaining only the most recent and relevant entries, ensuring each customer was represented accurately.

Some challenges required deeper business reasoning rather than simple cleanup. Product lifecycle data, for instance, did not always include clear end dates. To ensure accurate historical reporting, I applied forward-looking logic by deriving product end dates based on future start dates. Also, NULL product end date values indicate “currently active products”. Similarly, sales records occasionally contained missing or inconsistent pricing values. In these cases, prices and sales amounts were backfilled using related fields such as quantity, ensuring financial metrics remained reliable and internally consistent.

To validate the effectiveness of these transformations, I implemented a dedicated set of data quality checks after the Silver load. These checks focused on key business expectations such as unique primary keys, valid date ranges, standardized values, and consistency between related measures like sales, price, and quantity. This step ensured that only high-quality, business-ready data progressed to the Gold layer.

Overall, the Silver layer functioned as a data quality gate, reducing downstream risk, preventing misleading insights, and establishing a reliable foundation for analytics and reporting.

---

## Limitations, Constraints and Design Trade-offs

- External source connections were not available in the Databricks Free Edition, so CSV ingestion was used
- Orchestration was simplified to focus on core BI concepts rather than enterprise tooling
- Microsoft Fabric was not used due to access constraints, though the architecture is directly transferable

These trade-offs were intentional and documented to reflect realistic project constraints.

---

## Future Enhancements Opportunities

Potential extensions include:

- Automated ingestion from live source systems
- Incremental loading and change data capture
- Role-level security and governance enhancements
- Migration to Fabric or other cloud-native orchestration tools

These enhancements would build on the same architectural principles demonstrated here.

---

## Repository Structure

```
📁 data-warehouse-and-analytics-project/
├── 📁 datasets/                              # Raw datasets used for the project (ERP and CRM data)

├── 📁 docs/                                  # Project documentation and architecture details
│   ├── 📄 data_catalog_source_system.md       # Captures essential metadata of source systems
│   ├── 📄 data_cleaning_transformation.md     # Outlines the key data cleaning and transformation techniques
│   ├── 📄 data_dictionary.md                  # Provides detailed metadata for each column in the business-ready tables
│   ├── 📄 data_flow_diagram                   # Visual representation of data flow across layers
│   ├── 📄 data_flow_tasks                     # Flow of tasks in each layer
│   ├── 📄 data_integration                    # Visual representation that depicts how Source Tables are connected
│   ├── 📄 data_lakehouse_architecture         # High-level data architecture (Bronze, Silver, Gold)
│   ├── 📄 data_layer_specifications           # Summarizes the objectives, transformations, and targets of each layer
│   ├── 📄 data_model                          # Data model design (e.g., star schema)
│   ├── 📄 power_bi_report_pages               # Screenshots of all 3 Power BI report pages
│   ├── 📄 presentation_slide_deck             # Presentation deck related to this project
│   ├── 📄 stakeholder_requirements            # Stakeholder requirements and source system understanding

├── 📁 lakehouse/                       # Scripts for building the data lakehouse in Databricks
    ├── 📁 bronze/                        # Scripts for extracting and loading (full load) raw data
    ├── 📁 silver/                        # Scripts for cleaning and transforming data
    └── 📁 gold/                          # Scipts for creating analytical models (views and data model)

├── 📁 tests/                               # QA scripts for verifying integrity and logic of gold and silver layers

├── 📄 README.md                            # Project overview and instructions

```

**Note on Historical Data**:  

While the dataset used in this project spans from 2011 to 2014, it was selected for its realistic structure and complexity. This allowed for a robust simulation of real-world business scenarios including data modeling, segmentation, and KPI analysis.  

---

## Key Deliverables and Navigation Links

- A fully functional, Databricks-based **full-stack BI solution** following industry-standard **Medallion Architecture**

- **[Lakehouse (Bronze, Silver, and Gold) SQL scripts](https://github.com/nitinskunigal/Full-Stack-BI---Databricks-Lakehouse-to-Power-BI-Reporting/tree/main/lakehouse)** 

- **[QA/ Test Scripts](https://github.com/nitinskunigal/Full-Stack-BI---Databricks-Lakehouse-to-Power-BI-Reporting/tree/main/tests)** for Silver and Gold Layers

- **[Documentation](https://github.com/nitinskunigal/Full-Stack-BI---Databricks-Lakehouse-to-Power-BI-Reporting/tree/main/docs)** for data architecture diagram, data layer specifications, data flow diagram, data model, data catalog, etc.

- **[Power BI Dashboards](https://app.powerbi.com/view?r=eyJrIjoiZTM5ZTVkMjgtYzVhZi00NzUxLThhNjMtMTU4OTU3NGE5ODY3IiwidCI6ImM2ZTU0OWIzLTVmNDUtNDAzMi1hYWU5LWQ0MjQ0ZGM1YjJjNCJ9)**, shared online through Power BI Service

- **[Video Walkthrough](https://www.youtube.com/watch?v=VtoVotsweUE)** of this Project

- **[Presentation Deck](https://github.com/nitinskunigal/Full-Stack-BI---Databricks-Lakehouse-to-Power-BI-Reporting/blob/main/docs/presentation_slide_deck.pdf)**
