# Mercury

## Components

- **Airflow**: Orchestrates the pipeline with `CeleryExecutor`, PostgreSQL metadata database, and Redis for scalability.
- **PySpark with Iceberg**: Processes raw data (e.g., deduplication, filtering) and writes to an Iceberg table in MinIO for analytics
- **MinIO**: Stores raw CSV and Iceberg table data in an S3-compatible bucket.
- **Docker**: Assumes Airflow, PySpark with Iceberg, and MinIO are running in separate Docker containers.

## Project Structure

```
mercury/
├── airflow/
|   └── dags/
|       └── sample_bi.py              # Generates DAGs per app_id
├── scripts/
│   ├── crawler.py                # Script to craw data from API and save to MinIO
│   └── spark_processing.py       # PySpark script to process data into Iceberg/MinIO
├── jars/
│   └── (Iceberg JAR downloaded)  # Iceberg runtime JAR
├── logs/                         # Airflow logs
├── plugins/                      # Airflow plugins (optional)
├── Dockerfile.crawler            # Dockerfile for crawler
├── docker-compose.yml            # Docker Compose configuration
└── README.md                     # Setup instructions
```

## How to start on MacOS

- Run `colima start --vm-type=vz`
- Run `docker-compose down -v --remove-orphans`
- Run `docker-compose up -d --build`