# Smart City Real-Time Data Platform

An end-to-end, enterprise-grade data engineering platform designed to ingest, process, store, and analyze high-velocity IoT and environmental streaming data for smart city optimization. This containerized platform handles vehicle telemetry, localized weather tracking, real-time traffic updates, and critical road anomaly events using a unified Lambda architecture (combining real-time streaming and robust batch workflows).

<img width="1408" height="768" alt="preview" src="https://github.com/user-attachments/assets/c28c9bd0-5c3c-45ef-b37c-03052dbf0e8c" />


---

## 🏗️ System Architecture & Workflow

The platform leverages modern data technologies organized into a structured Medallion architecture orchestrated entirely via containerized environments:

1. **Data Generation & Ingestion:**
   * Custom Python simulators continuously emit vehicle telemetry data alongside real-time integrations with the **OpenWeatherMap API** and **TomTom Traffic API**.
   * **Apache Kafka** serves as the central message broker, handling high-throughput, low-latency ingestion across dedicated topics: `vehicle-telemetry`, `weather-data`, `traffic-events`, `road-events`, and `alerts`.

2. **Stream & Batch Processing:**
   * **Apache Spark (Structured Streaming)** consumes Kafka feeds concurrently via distributed processing jobs:
     * `bronze_writer.py`: Ingests and serializes raw event payloads directly.
     * `silver_cleaner.py`: Performs schema enforcement, type validation, deduplication, and parsing.
     * `gold_aggregator.py`: Computes multi-dimensional aggregations, dynamic traffic metrics, and business-level KPIs.
     * `alert_detector.py`: Scans live streams to surface real-time anomaly alerts (e.g., congestion, vehicle overspeeding).

3. **Storage & Data Lakehouse:**
   * Managed storage is split across an **AWS S3 Data Lake** utilizing the **Medallion Architecture**:
     * **Bronze Layer:** Raw Parquet formatted data streams.
     * **Silver Layer:** Cleaned, validated, and structural datasets.
     * **Gold Layer:** Aggregated operational metrics and business intelligence snapshots.
   * Curated relational layers are pushed directly to a **Snowflake Data Warehouse** structured into a Star Schema with explicit dimensions (`dim_vehicle`, `dim_location`, `dim_weather`, `dim_time`) mapping back to centralized fact tables (`fact_vehicle_events`).

4. **Orchestration & Data Transformation:**
   * **Apache Airflow** acts as the centralized control plane, scheduling complex workflow dependencies, coordinating batch workflows, and automating structural transformations.
   * **dbt (data build tool)** processes and validates downstream metrics inside Snowflake, handling staging models, data quality testing, and materializing analytical data marts.

5. **Analytics, Monitoring & Consumer Layer:**
   * **Grafana Live Dashboards** interface with the low-latency streaming layers to display real-time traffic trends, active geofenced alerts, and live congestion heatmaps.
   * Historical Business Intelligence views monitor long-term fuel consumption, environmental KPIs, and urban infrastructure planning analytics.
   * **Prometheus** provides comprehensive infrastructure-level observability, monitoring cluster health, container constraints, and vital Kafka/Spark metrics.

---

## 🛠️ Tech Stack

* **Ingestion & Messaging:** Apache Kafka
* **Distributed Processing:** Apache Spark (Structured Streaming & Core Batch)
* **Orchestration:** Apache Airflow
* **Data Storage & Warehousing:** AWS S3, Snowflake Data Warehouse
* **Modeling & Data Quality:** dbt (data build tool)
* **Visualization:** Grafana
* **Infrastructure Observability:** Prometheus, Docker Containerization
* **Development Language:** Python, SQL (T-SQL / Snowflake SQL)

---

## 🌟 Key Engineering Accomplishments

* **Scalable Architecture:** Designed a comprehensive Lambda/Medallion processing engine handling continuous high-frequency streams with strict schema enforcement.
* **Modular Pipeline Scripts:** Authored structured, performant processing scripts (`bronze_writer.py`, `silver_cleaner.py`, and `gold_aggregator.py`) optimizing data transformation boundaries.
* **Optimized Data Warehousing:** Modeled an analytical Star Schema inside Snowflake to streamline analytical query overhead for downstream consumer layers.
* **Automated Data Quality:** Implemented automated dbt data quality checks and test boundaries to ensure cross-layer consistency throughout pipeline iterations.
