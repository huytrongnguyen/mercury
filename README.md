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

## Start the Services

- Run `colima start --cpu 4 --memory 4 --disk 64 --vm-type=qemu` (for MacOS)
- Run `docker-compose down -v --remove-orphans`
- Run `docker-compose up -d --build`

This starts:
- Spark Master (accessible at http://localhost:8080)
- Spark Worker
- MinIO (accessible at http://localhost:9000 for API, http://localhost:9001 for console)
- Log in to MinIO console with admin/password and create a bucket named `mercury` (first time only).
  - Submit `spark-minio-job.py` for testing
    ```sh
    docker exec -it spark-master spark-submit \
    --master spark://spark-master:7077 \
    /opt/bitnami/spark/apps/sample/spark-minio-job.py
    ```