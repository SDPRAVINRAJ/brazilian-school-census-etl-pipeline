# 🇧🇷 Brazilian School Census ETL Pipeline

An end-to-end data engineering pipeline that processes **12 years** of Brazilian National Basic Education Census (Censo Escolar) data using **Apache Spark**, transforming raw CSV files into an analytical **Star Schema** in **PostgreSQL**, with interactive dashboards powered by **Metabase**.

---

## 📊 Dashboard Preview

| Metric | Value |
|---|---|
| 📚 Total Records Processed | 2,792,984 |
| 📅 Years Covered | 2010 – 2021 |
| 💾 Raw Data Size | ~2.5 GB |
| 📉 File Size Reduction (Parquet) | ~80% |
| 🏫 Total Enrolments in 2021 | 46,668,401 |
| 🌐 Schools with Internet (2021) | 67.5% |

---

## 🏗️ Pipeline Architecture

```
Raw CSV Files (INEP Open Data Portal)
        ↓
download_censo.py      — Download 12 years of ZIP files
        ↓
extract_censo.py       — Extract CSVs from ZIP archives
        ↓
convert_to_parquet.py  — Convert CSV → Parquet (Apache Spark)
        ↓
build_star_schema.py   — Build Star Schema → PostgreSQL (Spark + JDBC)
        ↓
Metabase Dashboard     — Business Intelligence & Visualisation
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **Apache Spark 3.5.1** | Distributed data processing |
| **PySpark** | Python API for Spark |
| **Python 3.11** | Programming language |
| **PostgreSQL 15** | Data warehouse |
| **Docker & Docker Compose** | Container orchestration |
| **Metabase** | BI dashboard & visualisation |
| **Adminer** | Database administration |
| **Parquet** | Columnar storage format |

---

## ⭐ Star Schema Design

```
         dim_local
              |
dim_tp_dependencia ──────── fact_censo_escolar ──────── dim_in_internet
              |                     |
      dim_tp_localizacao     dim_in_biblioteca
                                    |
                            dim_in_agua_potavel
                            dim_in_banheiro
                            dim_in_computador
                            dim_in_energia_inexistente
                            dim_in_esgoto_inexistente
                            dim_in_equip_nenhum
                            dim_in_refeitorio
```

**Fact Table:** `fact_censo_escolar` — 2.7M+ rows of school enrolment data

**Dimension Tables (10):**
- `dim_local` — State and municipality information
- `dim_tp_dependencia` — School dependency type (Federal/State/Municipal/Private)
- `dim_tp_localizacao` — Urban or Rural classification
- `dim_in_internet` — Internet access indicator
- `dim_in_biblioteca` — Library availability
- `dim_in_agua_potavel` — Potable water availability
- `dim_in_banheiro` — Bathroom facilities
- `dim_in_computador` — Computer availability
- `dim_in_energia_inexistente` — Electricity absence indicator
- `dim_in_refeitorio` — Canteen availability

---

## 🚀 Getting Started

### Prerequisites

- Python 3.11
- Java JDK 17
- Apache Spark 3.5.1
- Docker Desktop
- Git

### Installation

**1. Clone the repository**
```bash
git clone https://github.com/yourusername/brazilian-school-census-etl-pipeline.git
cd brazilian-school-census-etl-pipeline
```

**2. Create virtual environment**
```bash
py -3.11 -m venv venv311
venv311\Scripts\activate      # Windows
source venv311/bin/activate   # Mac/Linux
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Start Docker services**
```bash
docker-compose up -d
```

**5. Set environment variables (Windows PowerShell)**
```powershell
$env:JAVA_HOME = "C:\Program Files\Eclipse Adoptium\jdk-17.0.14.7-hotspot"
$env:SPARK_HOME = "C:\spark\spark-3.5.1-bin-hadoop3"
$env:HADOOP_HOME = "C:\hadoop"
$env:PYSPARK_PYTHON = "path\to\venv311\Scripts\python.exe"
$env:PYSPARK_DRIVER_PYTHON = "path\to\venv311\Scripts\python.exe"
$env:SPARK_LOCAL_HOSTNAME = "localhost"
```

---

## ▶️ Running the Pipeline

Run each script in order:

```bash
# Step 1 — Download raw data
python scripts/download_censo.py

# Step 2 — Extract CSVs from ZIP files
python scripts/extract_censo.py

# Step 3 — Convert CSV to Parquet
python scripts/convert_to_parquet.py

# Step 4 — Build Star Schema in PostgreSQL
python scripts/build_star_schema.py
```

---

## 📁 Project Structure

```
brazilian-school-census-etl-pipeline/
│
├── scripts/
│   ├── download_censo.py        # Download raw ZIP files from INEP
│   ├── extract_censo.py         # Extract CSVs from ZIP archives
│   ├── convert_to_parquet.py    # Convert CSV → Parquet using Spark
│   └── build_star_schema.py     # Build Star Schema → PostgreSQL
│
├── data/                        # Raw CSV files (not tracked in git)
├── output/                      # Parquet output files (not tracked in git)
├── extracted/                   # Extracted ZIP contents (not tracked in git)
├── raw_zip/                     # Downloaded ZIP files (not tracked in git)
│
├── docker-compose.yml           # Docker services configuration
├── postgresql.jar               # PostgreSQL JDBC driver
├── requirements.txt             # Python dependencies
├── test_spark.py                # Test Spark setup
├── test_postgres_connection.py  # Test PostgreSQL connection
```

---

## 🌐 Accessing Services

| Service | URL | Credentials |
|---|---|---|
| **Metabase** | http://localhost:3000 | Set during first login |
| **Adminer** | http://localhost:8081 | Server: spark_postgres, DB: censo_escolar, User: censo |
| **PostgreSQL** | localhost:5432 | User: censo, DB: censo_escolar |

---

## 📈 Key Insights

- 📉 Brazilian school enrolment **declined from 52M (2010) to 46.7M (2021)**
- 🏙️ **São Paulo** leads with 2,674,792 students enrolled in 2021
- 🌐 **32.5% of schools** still lacked internet access in 2021
- 📊 **SP, MG, BA** are the top 3 states by total enrolment

---

## 📦 Requirements

```
pyspark==3.5.1
py4j==0.10.9.7
```

---

## ⚠️ Notes

- Python **3.11** is required — PySpark 3.5.1 is not compatible with Python 3.12
- Windows users need `winutils.exe` and `hadoop.dll` for Hadoop 3.3.5
- Data files are excluded from git due to size (~2.5GB) — run `download_censo.py` to fetch them
- PostgreSQL JDBC driver (`postgresql.jar`) must be downloaded separately

---

## 📄 Data Source

Data sourced from **INEP (Instituto Nacional de Estudos e Pesquisas Educacionais Anísio Teixeira)**
- Brazilian Ministry of Education open data portal
- Annual Censo Escolar publications 2010–2021

---

## 📝 License

This project is for educational purposes. Data sourced from Brazil's open government data portal.

---

