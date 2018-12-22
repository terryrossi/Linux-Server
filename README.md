# **Linux Server Project.**

 The purpose of this project is to take a baseline installation of a Linux server and prepare it to host a web applications. We will secure our server from a number of attack vectors, install and configure a database server, and deploy one of our existing web applications onto it.

 To complete this project, we'll use a Linux server instance on AWS (Amazon Lightsail).

## **The Script**

The script is written in Python 3.

##### To execute the script, run: `python logdb.py`

## **The Environment**

### Ubuntu Linux server instance on Amazon Lightsail:

  - A new User has been created for the grader: `grader`.
  - To access the server using key pairs, please check:
      [AWS SSH Access](https://aws.amazon.com/premiumsupport/knowledge-center/new-user-accounts-linux-instance/)




### PostgreSQL Database

  - [Download the data here](https://d17h27t6h515a5.cloudfront.net/topher/2016/August/57b5f748_newsdata/newsdata.zip). You will need to unzip this file after downloading it. The file inside is called newsdata.sql. Put this file into the vagrant directory, which is shared with your virtual machine.

  - You'll need to load the site's data into your local database. Review how to use the psql command in this lesson: [(FSND version)](https://classroom.udacity.com/nanodegrees/nd004-mena/parts/a8609286-c119-4bc5-b9c9-2a3828080114/modules/56f0f4c7-d611-4949-b8d5-e1b9df12d95f/lessons/4cff95e1-3f1c-435a-bc6c-40fcf0d8f884/concepts/0b4079f5-6e64-4dd8-aee9-5c3a0db39840) (Access Required)

  - To load the data, `cd` into the vagrant directory and use the command `psql -d news -f newsdata.sql`.

      Here's what this command does:

      - psql — the PostgreSQL command line program
      - -d news — connect to the database named news which has been set up for you
      - -f newsdata.sql — run the SQL statements in the file newsdata.sql

        Running this command will connect to your installed database server and execute the SQL commands in the downloaded file, creating tables and populating them with data.

        #### Structure of the Database:

          - Database Name: `news`

            The database includes three tables:

            - The `authors` table includes information about the authors of articles.
            - The `articles` table includes the articles themselves.
            - The `log` table includes one entry for each time a user has accessed the site.

        #### The connection to the Database:

          - You'll want to use the psycopg2 Python module to connect your program to the database.

            For instance:  `db=psycopg2.connect("dbname=news")`

        #### Additional Views:

        I've created 3 views to facilitate the process:

          1. `CREATE OR REPLACE VIEW vlog (log_id, art_title, art_id, art_authorid, log_time, log_status)
            AS select log.id, articles.title, articles.id,articles.author, log.time, log.status
            from log , articles WHERE log.path = '/article/' || articles.slug`
             _This view to correlate rows from TB Log and TB articles based on the Unique key Slug from
             article and the path from log in concatenating "/article/" with articles.slug_

          2. `CREATE VIEW vstatusgood AS select date(time) as date,count(*) as num from log group by date order by num desc;`
              _This view to aggregate the number of Requests per Date_

          3. `CREATE VIEW vstatusbad AS select date(time) as date,count(*) as num from log where status not like '2%' group by date order by num desc;`
              _This view to aggregate the number of erroneous Requests per Date_


## The Report:

  Here are the questions the reporting tool should answer. The example answers given aren't the right ones, though!

  1. What are the most popular three articles of all time? Which articles have been accessed the most? Present this information as a sorted list with the most popular article at the top.

  Example:
      ```
      "Princess Shellfish Marries Prince Handsome" — 1201 views
      "Baltimore Ravens Defeat Rhode Island Shoggoths" — 915 views
      "Political Scandal Ends In Political Scandal" — 553 views
      ```    
  2. Who are the most popular article authors of all time? That is, when you sum up all of the articles each author has written, which authors get the most page views? Present this as a sorted list with the most popular author at the top.

  Example:
```
      Ursula La Multa — 2304 views
      Rudolf von Treppenwitz — 1985 views
      Markoff Chaney — 1723 views
      Anonymous Contributor — 1023 views
```
  3. On which days did more than 1% of requests lead to errors? The log table includes a column status that indicates the HTTP status code that the news site sent to the user's browser. (Refer to this lesson for more information about the idea of HTTP status codes.)

  Example:

      `July 29, 2016 — 2.5% errors`

The PGM will print the results and also create a text file named: `LogAnalysis.txt`

You can look at the file in the working directory to check the results.




## Code Quality

The code quality has been validated using The PEP8 style guide. You can do a quick check using the pep8 command-line tool.

I have also tried to document the process as much as possible using comment inside logDB.py

Please check it out and don't hesitate to let me know if these's anything I could have done better.

Regards.

Terry

## NEW VERSION: Code Reuse. This version is enhanced with the use of functions called by If __name__ == '__main__': ...  Based on Mentor suggestion.
