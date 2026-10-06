# 🎬 Producer's Edge — Movie Analytics & Decision Support System

[![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=flat&logo=mysql&logoColor=white)](https://www.mysql.com/)
[![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Processing-150458?style=flat&logo=pandas&logoColor=white)](https://pandas.pydata.org/)

> **Producer's Edge** is an end-to-end relational database and analytics platform tailored for film producers and studio executives. By normalizing large-scale Kaggle movie datasets into an optimized MySQL architecture, the system provides high-speed analytics for concept validation, talent casting, and market trend discovery.
<img width="2000" height="2588" alt="image" src="https://github.com/user-attachments/assets/0d953a8e-5e62-487a-974b-a698929f5f71" />

---

## 📌 Key Modules & Features

The system exposes three core modules via a graphical decision-support interface:

### 1. 🔍 Concept & Title Analysis
- **Plot Concept Discovery:** Utilizes MySQL FULLTEXT natural-language searching across plot overviews to retrieve financially comparable films, displaying Budget, Revenue, and calculated ROI Ratio.
- **Competitor Title Search:** Matches title keywords to assess competitor reception, voter engagement, and historical popularity metrics.

### 2. 🌟 Talent & Casting Intelligence
- **High-Performing Actor Pairs ("Power Couples"):** Analyzes co-star chemistry by running self-joins on cast records (cast_order < 10), computing collaborative average ratings filtered by minimum shared projects.
- **Top-Grossing Directors:** Evaluates lifetime box-office performance by aggregating worldwide movie revenues per director.

### 3. 📈 Market Trends & Genre Analytics
- **Lucrative Genre Mashups:** Detects high-performing multi-genre combinations (e.g., Adventure + Fantasy) through self-joining junction tables, dynamic revenue threshold filtering, and average box-office aggregation.

---

## 🏛️ Database Architecture & Design

### Relational Schema (3NF)
Raw nested JSON structures (cast, crew, genres, tags) were normalized into a clean relational schema to ensure referential integrity, eliminate anomalies, and enable high-performance indexing:

    movies ---< movie_genres >--- genres
      |
      |---< movie_cast >--- people
      |
      |---< movie_crew >--- people
      |
      |---< movie_keywords >--- keywords
      |
      |---< movie_ratings_summary (1:1)

### Entities & Relationships
- **Core Entities:** `movies`, `genres`, `people`, `keywords`, `movie_ratings_summary`.
- **Junction Tables:** `movie_genres`, `movie_cast`, `movie_crew`, `movie_keywords` with composite primary keys ensuring unique relationship mappings and fast associative lookups.
- **Referential Integrity:** Enforced via `FOREIGN KEY` constraints configured with `ON DELETE CASCADE` and `ON UPDATE CASCADE`.

---

## ⚡ Performance Optimizations & Indexing

To support fast complex joins and analytical aggregations:
- **Clustered Indexes:** Primary keys on every base and junction table.
- **Full-Text Search Indexes:**
  - `FULLTEXT idx_ft_title (title)` for instant competitor searches.
  - `FULLTEXT idx_ft_overview (overview)` for natural language plot queries.
- **B-Tree & Composite Indexes:**
  - `INDEX idx_revenue (revenue)` on `movies` for sorting box-office queries and ROI calculations.
  - `INDEX idx_movie_cast_order (movie_id, cast_order, person_id)` on `movie_cast` to accelerate self-joins and early filtering.
  - `INDEX idx_job (job)` on `movie_crew` to quickly filter director roles.

---

## 📂 Repository Structure

    ├── src/
    │   ├── dataSets/               # Cleaned CSV data sources (Kaggle Movies Dataset)
    │   ├── create_db_script.py     # Schema creation DDL scripts (in dependency order)
    │   ├── api_data_retrieve.py    # Batched CSV insertion logic (1000 rows/batch)
    │   ├── queries_db_script.py    # Parameterized SQL query routines (SQL-injection safe)
    │   └── queries_execution.py    # Example executions & benchmarks
    ├── config.py                   # Database connection configuration
    ├── system_docs.md              # In-depth architectural & SQL query specifications
    ├── user_manual.pdf             # Application GUI walkthrough & manual
    └── requirements.txt            # Python dependencies

---

## 🚀 Getting Started

### Prerequisites
- Python 3.9+
- MySQL Server 8.0+

### Installation & Setup

1. **Clone the repository:**
   git clone [https://github.com/yuvalnamirbarr/FIANL-PROJECT-DBMS.git](https://github.com/yuvalnamirbarr/FIANL-PROJECT-DBMS.git)
   cd FIANL-PROJECT-DBMS

2. **Install dependencies:**
   pip install -r requirements.txt

3. **Configure Database Credentials:**
   Update `config.py` with your MySQL connection parameters.

4. **Initialize Schema & Load Data:**
   python src/create_db_script.py
   python src/api_data_retrieve.py

5. **Run Queries / Dashboard:**
   python src/queries_execution.py
