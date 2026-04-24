# 🎬 TMDB Movie Database & Analytics Platform

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![API](https://img.shields.io/badge/TMDB_API-01B4E4?style=for-the-badge&logo=themoviedb&logoColor=white)

> An end-to-end data pipeline and relational database system designed for film and audience analysis. It automatically ingests real-world movie data from the TMDB API and stores it in a highly optimized PostgreSQL database. 

---

## ✨ Project Highlights

- **🤖 Automated Data Pipeline:** A robust Python script that fetches and paginates through the TMDB API, dynamically handling inserts and updates (Upserts) for movies and genres.
- **🏗️ Optimized Relational Schema:** A 3NF-compliant PostgreSQL database separating static metadata (movies, genres) from dynamic interaction data (reviews, stats, watchlists).
- **⚡ Automated Aggregation:** Utilizes PostgreSQL Triggers (`update_local_movie_stats`) to automatically calculate and update a movie's average rating and total review count in real-time.
- **🚀 Performance Tuned:** Implements specific B-Tree Indexes to drastically reduce query planning and execution time for complex analytical queries.
- **🛡️ Data Integrity:** Enforces strict Foreign Key constraints with `ON DELETE CASCADE` to prevent orphan data, along with composite primary keys to prevent duplicates.

---

## 🗄️ Database Schema Overview

The database consists of **7 core tables** and **2 analytical views**:

### Core Tables

| Table Name | Type | Description |
| :--- | :--- | :--- |
| `genres` | Dictionary | Standardized list of movie genres. |
| `movies` | Core Entity | Static TMDB movie metadata (titles, overview, dates, ratings, posters). |
| `movie_genres` | Mapping | Many-to-many relationship between movies and genres. |
| `users` | Core Entity | Independent entity for user accounts (includes masked password hashes). |
| `user_reviews` | Interaction | User interactions containing local 0-10 ratings and review text. |
| `movie_stats` | Partitioned | Vertically partitioned table storing real-time aggregated local stats to relieve read/write pressure on the `movies` table. |
| `watchlists` | Interaction | Tracks movies users want to watch later (composite PK prevents duplicates). |

### Analytical Views

* **`v_movie_full_details`**: A comprehensive view joining movies, aggregated genres (comma-separated), and local stats for easy frontend display (e.g., Streamlit).
* **`v_public_user_info`**: A secure view exposing only non-sensitive user profile data.

---

## 🛠️ Setup & Installation

### 1. Prerequisites
* Install **PostgreSQL** and ensure the server is running.
* Install **Python 3.7+**.
* Obtain a free API key from TMDB.

### 2. Database Initialization
Open your PostgreSQL client (e.g., DBeaver, pgAdmin, or `psql`) and create a new database named `Movie_Project`. Execute the provided `schema.sql` file to generate all tables, views, functions, triggers, and indexes.

### 3. Python Environment Setup
Install the required Python packages via terminal:
```bash
pip install requests psycopg2-binary
