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
   git clone (https://github.com/HienTa2/Apache_Airflow_Astro)
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
  
![{6467E3C9-B2C8-442C-84F1-ED35A349DAA4}](https://github.com/user-attachments/assets/d54919d0-8a7e-4a1e-8a02-a5c63aa2d520)


5. **Access Airflow UI**
   - Navigate to `(http://localhost:8080)` in your browser.
   - Login with default credentials:
     - **Username**: `admin`
     - **Password**: `admin`
    
![{79396173-7584-4F64-8BA2-27C952C06547}](https://github.com/user-attachments/assets/13bb656e-4dc1-4c13-8935-c9b91d90bd0c)


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
![{4C943DF2-B8BB-4791-840D-272D1347CE47}](https://github.com/user-attachments/assets/6d20d357-1cf8-4339-a436-8ee53c565b52)


---

## Key Files

### **etl.py**
This is the main DAG script for the ETL pipeline. It contains the following tasks:

1. **Extract**: Fetches weather data from the Open-Meteo API using `HttpHook`.
2. **Transform**: Processes the API response and extracts relevant fields.
3. **Load**: Inserts the transformed data into the PostgreSQL database.



    # Define the ETL workflow
    weather_data = extract_weather_data()
    transformed_data = transform_weather_data(weather_data)
    load_weather_data(transformed_data)
```

### **docker-compose.yml**
Defines the services required for the project, including PostgreSQL. The configuration is:
```yaml
version: '3'
services:
  postgres:
    image: postgres:13
    container_name: postgres_db
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: postgres
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### **airflow_settings.yaml**
Used for configuring Airflow connections, pools, and variables during local development.
```yaml
# Configure Airflow connections and other metadata.
airflow:
  connections:
    - conn_id:
      conn_type:
      conn_host:
      conn_schema:
      conn_login:
      conn_password:
      conn_port:
      conn_extra:
        example_extra_field: example-value
  pools:
    - pool_name:
      pool_slot:
      pool_description:
  variables:
    - variable_name:
      variable_value:
```

---

## Git Commands for Workflow

### **Basic Git Commands**

1. **Clone Repository**:
   ```bash
   git clone https://github.com/HienTa2/Apache_Airflow_Astro
   cd ETL_Apache_Airflow_Astro
   ```

2. **Check Status**:
   ```bash
   git status
   ```

3. **Stage Changes**:
   ```bash
   git add <file_name>
   ```
   Or to add all changes:
   ```bash
   git add .
   ```

4. **Commit Changes**:
   ```bash
   git commit -m "Your commit message"
   ```

5. **Push Changes**:
   ```bash
   git push origin <branch_name>
   ```

6. **Pull Latest Changes**:
   ```bash
   git pull origin <branch_name>
   ```

7. **Create a New Branch**:
   ```bash
   git checkout -b <new_branch_name>
   ```

8. **Merge Branches**:
   ```bash
   git checkout <target_branch>
   git merge <source_branch>
   ```

9. **Resolve Merge Conflicts**:
   - Edit the conflicting files.
   - Mark them as resolved:
     ```bash
     git add <file_name>
     ```
   - Complete the merge:
     ```bash
     git commit
     ```

10. **Push New Branch to Remote**:
    ```bash
    git push -u origin <branch_name>
    ```

11. **Delete Branch**:
    - Locally:
      ```bash
      git branch -d <branch_name>
      ```
    - Remotely:
      ```bash
      git push origin --delete <branch_name>
      ```

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
