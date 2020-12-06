# This is First Change in Git-Project

Project Summary:

A Startup called Sparkify wants to run some analytics on their new music streaming app usage, to improve its features and user adoptability. They are interested in understanding the user activity and their preferences in music attributes.
As part of data collection activity, Music Streaming app collects 'Songs' and 'User Activity' data which is stored in AWS S3 location in JSON format.

Currently, as there is no easy way to query data directly from JSON files, so the data needs to be copied from S3, process it and load into AWS Redshift database so that Analytics team from Sparkify can run their queries to get the required information.

Schema Design and ETL Pipeline:

Step 1: Get the params of the created redshift cluster:
Our first step would be to create Redshift from AWS Console and get the following details, which are required to establist redshift connection from outside AWS:
	1. redshift cluster endpoint
	2. the IAM role ARN that give access to Redshift to read from S3

update these cluster details of yours in configuration file 'dwh.cfg', endpoint assigned to Host variable, and arn to ARN variable.

Step 2: Connect to the Redshift Cluster

To pull data out of Redshift, we first need to connect to our instance. To do that we need to use a library or driver for Python to connect to Amazon Redshift. In this project we are using Psycopg2 library that is most recommended by PostgreSQL.

All the Create Table and Insert Table Statements are defined in 'sql_queries.py' file, and this is called inside 'create_table.py' file. 

From the Terminal, run create_table.py python file, doing which following actions are performed:
1. Establishes connection with Redshift DB using Psycopg2 library.
2. Creates staging tables, dimension and fact tables in redshift by execute Create SQL statements defined in 'sql_queries.py' python file.

Step 3: Import data from S3 into Redshift DB structures

Once redshift we are going to access Sparkify data which is stored in public S3 bucket, the location details are already defined in 'dwh.cfg' file.
From the Terminal, run 'etl.py' python file, doing which following actions are performed:

    1. From given S3 bucket, read Spotify event and log files, and load into redshift as staging tables (stg_events & stg_songs). There will not be any data manipulations in this process of data extraction and load, instead the staging tables will have as-is data from source.

    2. Load Star Schema Structures - dimensions and fact tables

Now that, we have all the data read from S3 and loaded into our redshift staging tables, we will be reading the required data and populated into Star Schema structures which can benifit for analytical usage.

Staging tables data is splitted into below structures:
one fact table (songsplay), four dimension tables (users, songs, artists, time). 

As the data is loaded in the above structured, the Analysts will be able to run SQL queries and get the required results.
