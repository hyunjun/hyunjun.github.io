---
layout: post
title: import csv file to MySQL, load, mysqlimport
---

# Situation
* Necessary to import CSV file into MySQL

# LOAD DATA INFILE

# mysqlimport
* Usage

  ```
  # use csv
  $ mysqlimport -h x.y.z.w -u <user> -p<pwd> --local --fields-terminated-by=, <database> <table name>.csv

  # use tab delimeter
  $ mysqlimport -h x.y.z.w -u <user> -p<pwd> --local --fields-terminated-by='   ' <database> <table name>.csv
  mysqlimport: [Warning] Using a password on the command line interface can be insecure.
  <database>.<table name>: Records: 938986  Deleted: 0  Skipped: 6215  Warnings: 1884187
  ```
  * `--local` is important
  * If there are four columns in <table name>, e.g. id, name, date, school
  * Let's say id and name is NOT NULL, date and school can be NULL
  * <table name>.csv could be like below

    ```
    $ head <table name>.csv
    0<tab>John
    1<tab>Micheal
    ...
    ```
  * After importing csv file, table looks like below

    ```
    mysql> select * from <table name>;
    +----+---------+------+--------+
    | id | name    | date | school |
    +----+---------+------+--------+
    | 0  | John    | NULL |   NULL |
    | 1  | Micheal | NULL |   NULL |
    ...
    ```
* ref
  * [How to Import a Large CSV file to MySQL](http://chriseiffel.com/everything-linux/how-to-import-a-large-csv-file-to-mysql/)
