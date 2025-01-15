# Weather ETL Pipeline with Apache Airflow

This project demonstrates an ETL pipeline built using **Apache Airflow** to extract weather data from the Open-Meteo API, transform it, and load it into a PostgreSQL database. The pipeline is orchestrated using Docker and monitored with Airflow’s web interface.

---

## Project Structure
```
ETL_Apache_Airflow_Astro/
│
├── .astro/                   # Astro project metadata
├── dags/                     # Airflow DAGs folder
│   ├── etl.py                # Main ETL pipeline script
│   ├── exampledag.py         # Example DAG provided by Astro
│   └── .airflowignore        # Ignore unnecessary files in Airflow
├── include/                  # Optional additional scripts
├── plugins/                  # Custom Airflow plugins
├── tests/                    # Test scripts
├── Dockerfile                # Defines Airflow container
├── docker-compose.yml        # Docker Compose configuration
├── airflow_settings.yaml     # Airflow metadata settings
├── requirements.txt          # Python dependencies
└── README.md                 # Project documentation (this file)
```

---

## Setup Instructions

### Prerequisites
1. Install **Docker Desktop** and ensure it’s running.
2. Install **Astro CLI** (`npm install -g @astro/cli` or use the Astro installer).
3. Ensure Python is installed on your system.

---

### Steps to Run the Project

1. **Clone the Repository**
   ```bash
   git clone <repository_url>
   cd ETL_Apache_Airflow_Astro
   ```

2. **Initialize Astro Project**
   ```bash
   astro dev init
   ```

3. **Add Required Dependencies**
   Update `requirements.txt` with the following:
   ```
   apache-airflow
   apache-airflow-providers-http
   apache-airflow-providers-postgres
   requests
   ```

4. **Start the Astro Project**
   ```bash
   astro dev start
   ```
   - This will start the Airflow scheduler, webserver, and PostgreSQL database as Docker containers.

5. **Access Airflow UI**
   - Navigate to `http://localhost:8080` in your browser.
   - Login with default credentials:
     - **Username**: `admin`
     - **Password**: `admin`

6. **Set Up Connections in Airflow**
   - **PostgreSQL Connection** (`postgres_default`):
     - Type: `Postgres`
     - Host: `postgres`
     - Port: `5432`
     - Login: `postgres`
     - Password: `postgres`
   - **API Connection** (`open_meteo_api`):
     - Type: `HTTP`
     - Base URL: `https://api.open-meteo.com`

7. **Place Your DAG**
   - Copy `etl.py` into the `dags/` folder.

8. **Trigger the DAG**
   - Activate and trigger the DAG from the Airflow UI.

9. **Verify the Data**
   - Use a PostgreSQL client (e.g., **DBeaver**) to connect to the database and query the `weather_data` table:
     ```sql
     SELECT * FROM weather_data;
     ```

---

## Technical Highlights

### Tools Used
- **Apache Airflow**: Orchestrates the ETL pipeline.
- **PostgreSQL**: Stores the transformed weather data.
- **Open-Meteo API**: Provides weather data via HTTP.
- **Docker**: Manages containerized services for Airflow and PostgreSQL.

### ETL Pipeline Workflow
1. **Extract**: Fetches weather data using the Open-Meteo API.
2. **Transform**: Cleans and prepares the data for storage.
3. **Load**: Inserts data into the PostgreSQL database.

---

## Troubleshooting

### Merge Issues
If you encounter `fatal: refusing to merge unrelated histories`, resolve it as follows:
1. Allow unrelated histories:
   ```bash
   git merge origin/main --allow-unrelated-histories
   ```
2. Push changes to the correct branch:
   ```bash
   git push -u origin main
   ```

### DAG Not Showing in Airflow
- Ensure `etl.py` is in the `dags/` directory.
- Restart Astro to reload DAGs:
  ```bash
  astro dev restart
  ```

---

## How to Extend the Project
- Add new data sources or APIs to the pipeline.
- Visualize the weather data using tools like Tableau or Power BI.
- Add more transformations, such as data normalization or aggregation.
- Schedule the pipeline for other cities or regions.
