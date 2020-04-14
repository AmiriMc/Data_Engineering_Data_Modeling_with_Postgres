# Project: Data Modeling with Postgres

## Introduction

This project was provided as part of Udacity's Data Engineering Nanodegree program.

A startup called *Sparkify* wants to analyze the data they've been colelcting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. Currently, they don't have an easy way to query their data, which resides in a directory of __JSON logs__ on user activity on the app, as well as a directory with __JSON metadata__ on the songs in their app. 

They'd like a __data engineer__ to create a __Postgress database__ with tables designed to optimize queries on song play analysis, and bring you on the project. Your role is to create a database schema and __ETL pipeline__ for this analysis. You'll be able to test your database and ETL pipeline by running queries given to you by the analytics team from Sparkify and compare your results with their expected results.

## Project Description

The goal of this project was to model user activity data to create a database and ETL pipeline in Postgres using __Python__. I had to define __Fact and Dimension tables__ for a star schema for a particular analytic focus, and write an ETL pipeline that transfers data from files in two local directories into these tables in Postgres using Python and __SQL__.

### To run the Python scripts, follow instructions below:

1. Unzip the Data folder. Make sure the unzipped Data folder is in the same directory as the scripts.
2. Load Docker image using the instructions below.
3. In a terminal, run the command `python create_tables.py` to run the create_tables.py script.
4. Run all cells in etl.ipynb to observe how the ETL process was developed for each table.
5. Run all cells in test.ipynb to test whether all of the tables were loaded correctly.
6. Close the test.ipynb notebook by restarting the kernel.
7. Close the etl.ipynb by restarting the kernel, or by running `conn.close()` from inside the notebook.
8. In a terminal, run the command `python etl.py` script to read and process all of the files from `song_data` and `log_data` and load them into the tables.
9. Rerun test.ipynb to test whether all of the tables were loaded correctly.

## Schema Design for the Database

The __Star schema__ design was used to create this database. The design includes 1 Fact table (songplays) and 4 Dimension tables (users, songs, artists, and time). The _sql_queries.py_ file contains all of the PostgreSQL queries such as `CREATE TABLE` , `DROP table IF EXISTS` , `INSERT INTO` , and `SELECT`. The _create_tables.py_ file is used to create the sparkifydb database, and all of the required tables that are defined in the _sql_queries.py_ script.

![](https://github.com/AmiriMc/Data_Engineering_Data_Modeling_with_Postgres/blob/master/StarSchema.png?raw=t)

## ETL Pipeline
The _etl.py_ script sets up the ETL pipeline. ETL (Extract, Transform, and Load) methods were used to populate the _songs_ and _artists_ tables from the data within the JSON song files (`data/song_data/`) and to populate the _users_ and _time_ tables from the JSON log files (`data/log_data/`). A `SELECT` query gathers the `song_id` and `artist_id` information based on the title, artist name, and song duration from the log file.

## Example Queries
Some useful example queries tested in Jupyter:
* Get total number of users: `SELECT COUNT(user_id) FROM users`
* Get total number of female 'F' (or male 'M') users: `SELECT COUNT(gender) FROM users WHERE gender = 'F'`
* Get year of oldest/newest ('MIN' or 'MAX') activity : `SELECT MIN(year) FROM time`

### Docker Image (contributed by Ken Hanscombe)

A Docker image was used so that I could develop this project on my local machine, rather than on Udacity's internal system. Thank you to Ken Hanscombe for creating this stable, easy to implement image.

The Docker image __postgres-student-image__ is on the Docker hub, from which you can run a container with user 'student', password 'student', and database studentdb (the starting point for this project). You do not need to install Postgress (it runs in the container).

To download the image, install [Docker](https://docs.docker.com/), which requires you to create a username and password. In a terminal, log into Docker Hub (you will be prompted for your Docker username and password).


Run the following commands in a terminal:
```
docker login docker.io
```

Pull the image:
```
docker pull onekenken/postgres-student-image
```
Run the container:
```
docker run -d --name postgres-student-container -p 5432:5432 onekenken/postgres-student-image
```
The create_tables.py pre-defined connection `conn = psycopg2.connect("host=127.0.0.1 dbname=studentdb user=student password=student")` will now connect to the container.

To stop and remove the container after the exercise:
```
docker stop postgres-student-container
docker rm postgres-student-container
```
