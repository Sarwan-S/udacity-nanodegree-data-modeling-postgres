# Description
This application performs ETL to create a Postgres relational database for optimising queries on song plays analysis for Sparkify. Sparkify would like to understand their users' usage patterns on their music streaming app.

Data collected by Sparkify include songs data files as well as log data files, all of which are in JSON format. This application transforms and loads these files into relational ables for easy querying via SQL.

# Purpose of Database
The Postgres database created by this ETL is called **sparkifydb**. This relational database would allow the Sparkify analytics team to perform queries to find the following type insights, among others:

1. Most active users based on 
2. Most popular songs based on numer of times the song is played
3. Most popular artists based on number of times their songs are played
4. Songs that specific users like listening to
5. Popular songs in specific geographies

As this is a relational database, the analytics team would be able to write flexible simplfied queries depending on the types of insights they would like to retrieve. This database also provides the following importances:

1. A standard data model is enforced.
2. Updates and insertions can be performed as more data is collected
3. Data integrity is maintained during insertions/modifications of the data
4. Data has an intuitive and simple organisation for flexible SQL queries

# Database Schema Design
A STAR schema design has been implemented for sparfifydb. This simplifies the queries the analytics team will write through it's denormalised design. It also allows for faster aggregations as relevant information is captured in a denormalised structure.

This database consists of one fact table and five dimension tables. This design allows for the analytics team to run quick queries on the fact table to understand business process metrics on song plays by users in sessions. It also allows for queries to answer when, where and what type of questions (when were the songs played? which location were they played from? what songs are most popular?) using queries that combine the fact table with the dimension tables.

## Fact Table
### songplays
A fact table named **songplays** captures information from the log data files about which songs were played by which user during a session.

The following attributes are captured in this fact table:

| Column      | Data Type |
|-------------|-----------|
| songplay_id | serial    |
| start_time  | timestamp |
| user_id     | int       |
| level       | varchar   |
| song_id     | varchar   |
| artist_id   | varchar   |
| session_id  | int       |
| location    | varchar   |
| user_agent  | text      |

## Dimension Tables
### users
The **users** dimension table captures information from the log data files about the users of the music streaming app.

The following attributes are captured in this dimension table:

| Column     | Data Type |
|------------|-----------|
| user_id    | int       |
| first_name | varchar   |
| last_name  | varchar   |
| gender     | char(1)   |
| level      | varchar   |

If a user changes his level from free to paid or vice versa, the database record is updated accordingly.

### songs
The **songs** dimension table captures information from the songs data files about songs in the music database.

The following attributes are captured in this dimension table:

| Column    | Data Type |
|-----------|-----------|
| song_id   | varchar   |
| title     | varchar   |
| artist_id | varchar   |
| year      | int       |
| duration  | numeric   |

### artists
The **artists** dimension table captures information from the songs data files about artists in the music database.

The following attributes are captured in this dimension table:

| Column    | Data Type |
|-----------|-----------|
| artist_id | varchar   |
| name      | varchar   |
| location  | varchar   |
| latitude  | numeric   |
| longitude | numeric   |

### time
The **time** dimension table captures information from the log data files about timestamps of records in **songplays** broken down into specific time units.

The following attributes are captured in this dimension table:

| Column     | Data Type |
|------------|-----------|
| start_time | timestamp |
| hour       | int       |
| day        | int       |
| week       | int       |
| month      | int       |
| year       | int       |
| weekday    | int       |

# ETL Pipeline
## Files
The following files are present in this project:

| File/Folder      | Description                                                                                 |
|------------------|---------------------------------------------------------------------------------------------|
| data             | Folder containing files for songs data and log data in JSON format                          |
| create_tables.py | Python file which creates the database, fact and dimension tables                           |
| etl.ipynb        | Notebook file which reads and processes a single file and loads the data into the tables    |
| etl.py           | Python file which reads and processes all files and loads them into your tables             |
| sql_queries.py   | Python file which contains all sql queries, and is imported into the last three files above |
| README.md        | A read me file which provides discussion on this ETL application                            |

## Usage

1. Run the create_tables.py file in a terminal using the following command:
```python create_tables.py```

2. Run the etl.py file in a terminal using the following command:
```python etl.py```

> NOTE: You will not be able to run test.ipynb, etl.ipynb, or etl.py until you have run create_tables.py at least once to create the sparkifydb database, which these other files connect to
