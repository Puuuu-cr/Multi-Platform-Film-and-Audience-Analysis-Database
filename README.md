# Multi-Platform-Film-and-Audience-Analysis-Database
This project is an end-to-end data pipeline and relational database system designed for film and audience analysis. It automatically ingests real-world movie data from the TMDB (The Movie Database) API and stores it in a highly optimized PostgreSQL database. The system supports advanced features like user reviews, watchlists, automated statistical updates via database triggers, and performance-tuned analytical queries.

✨ Project Highlights
Automated Data Pipeline: A robust Python script that fetches and paginates through the TMDB API, dynamically handling inserts and updates (Upserts) for movies and genres.

Optimized Relational Schema: A 3NF-compliant PostgreSQL database separating static metadata (movies, genres) from dynamic interaction data (reviews, stats, watchlists).

Automated Aggregation: Utilizes PostgreSQL Triggers (update_local_movie_stats) to automatically calculate and update a movie's average rating and total review count in real-time whenever a user submits a review.

Performance Tuned: Implements specific B-Tree Indexes to drastically reduce query planning and execution time for complex analytical queries (e.g., filtering by genre and release date).

Data Integrity: Enforces strict Foreign Key constraints with ON DELETE CASCADE to prevent orphan data, along with composite primary keys to prevent duplicate watchlist entries.

🛠️ Tech Stack
Database: PostgreSQL (with PL/pgSQL for triggers/functions)

Language: Python 3.x

Libraries: requests (API calls), psycopg2 (PostgreSQL adapter)

External API: TMDB API

🗄️ Database Schema Overview
The database consists of 7 core tables and 2 analytical views:

Core Tables
genres: Dictionary of movie genres.

movies: Core entity storing static TMDB movie metadata (titles, overview, dates, TMDB ratings, poster URLs).

movie_genres: Many-to-many mapping between movies and genres.

users: Independent entity for user accounts (includes masked password hashes).

user_reviews: User interactions containing local 0-10 ratings and review text.

movie_stats: Vertically partitioned table storing real-time aggregated local stats (total reviews, average rating) to relieve read/write pressure on the main movies table.

watchlists: Tracks movies users want to watch later (composite primary key prevents duplicate adds).

Views
v_movie_full_details: A comprehensive view joining movies, aggregated genres (comma-separated), and local stats for easy frontend display (e.g., Streamlit).

v_public_user_info: A secure view exposing only non-sensitive user profile data.

🚀 Setup & Installation
1. Prerequisites
Install PostgreSQL and ensure the server is running.

Install Python 3.7+.

Obtain a free API key from TMDB.

2. Database Initialization
Open your PostgreSQL client (e.g., DBeaver, pgAdmin, or psql).

Create a new database named Movie_Project (or your preferred name).

Execute the provided schema.sql file to generate all tables, views, functions, triggers, and indexes.

3. Python Environment Setup
Install the required Python packages:

Bash
pip install requests psycopg2-binary
4. Configuration
Open the data_pipeline.py script and update the core configuration variables to match your local setup:

Python
TMDB_API_KEY = "your_tmdb_api_key_here"
DB_CONFIG = {
    "host": "localhost",
    "database": "Movie_Project",
    "user": "your_postgres_username",
    "password": "your_postgres_password",
    "port": "5432"
}
5. Run the Pipeline
Execute the Python script to start fetching data. By default, it syncs the genre dictionary and then fetches the top 500 pages (10,000 movies) of popular films.

Bash
python data_pipeline.py
You will see a real-time console log tracking the insertion of each page.

⚡ Query Optimization
To handle heavy analytical workloads, the database includes pre-configured indexes on frequently queried columns:

SQL
CREATE INDEX idx_genres_name ON genres(genre_name);
CREATE INDEX idx_movies_release_date ON movies(release_date);
CREATE INDEX idx_movie_stats_avg_rating ON movie_stats(average_rating);
Note: You can use the provided EXPLAIN ANALYZE statement in the SQL script to benchmark query performance before and after applying these indexes.
