# Real-Time Stocks Market Data Pipeline

An end-to-end real-time data engineering project that ingests, processes, and analyzes live stock market data, turning it into actionable insights for analytics and reporting.

---

## ✅ Key Features

- Live stock data ingestion from Finnhub API (real-time, not simulated)
- Real-time streaming with Apache Kafka  
- Automated ETL orchestration with Airflow 
- Scalable cloud data warehouse powered by Snowflake  
- Medallion Architecture (Bronze → Silver → Gold)  
 
---

## 📂 Project Structure

```text
├── producer/
│   └── producer.py
├── consumer/
│   └── consumer.py
├── infra/
│   ├── docker-compose.yml
│   └── dags/
│       └── minio_to_snowflake.py
├── dbt_stocks/
│   └── dbt_stocks/
│       ├── models/
│       │   ├── bronze/
│       │   ├── silver/
│       │   └── gold/
│       └── dbt_project.yml
└── requirements.txt
```
---
## 🛠 Infrastructure Setup

### 1. Clone the Repository
```bash
git clone <repo-url>
cd <repo-folder>
```
### 2. Start Services (Kafka, Airflow, MinIO)
Starts all required services like Kafka, Airflow, and MinIO using Docker.

```bash
cd infra
docker-compose up -d
```
### 3. Verify Services

* Airflow UI http://localhost:8080
* Kafdrop (Kafka UI) http://localhost:9000
* MinIO UI http://localhost:9001

### 🏗 Running the Pipeline
#### Step 1: Start Producer
Fetches live stock data from the API and sends it to Kafka.
```bash
python producer/producer.py
```
#### Step 2: Start Consumer
Reads data from Kafka and stores raw data into MinIO.
```bash
python consumer/consumer.py
```
#### Step 3: Enable Airflow DAG
- Used to schedule and monitor the pipeline that loads data into Snowflake.
1. Open the Airflow UI.
2. Unpause/Enable the DAG: minio_to_snowflake.
3. This process moves raw data from MinIO to the Snowflake Bronze layer.

#### Step 4: Snowflake Setup
- Creates storage and compute resources where pipeline data will be stored and processed.

```txt
Ensure the following objects are created in your Snowflake account before proceeding:
1. Database
2. Schema
3. Warehouse
```

#### Step 5: dbt Transformations
Run dbt to transforms raw Snowflake data into clean and business-ready models.
```bash
cd dbt_stocks/dbt_stocks
dbt run
dbt test
```
#### Outcome:
- Bronze: Raw ingested data.
- Silver: Cleaned and validated data.
- Gold: Business-level metrics and aggregates.
```txt
Silver models are built from Bronze data.
Gold models are built from Silver tables.
Tests are executed to ensure data quality.
```
