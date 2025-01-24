---
title: Run Conditions Database (Condb2)
teaching: 30
exercises: 0
questions:
- What is the Run Conditions Database (Ucondb2)?
- How to access it?
objectives:
- Learn how to upload and download data from the Run Conditions Database
keypoints:
- There are several APIs available to access the information in the conditions database. Which to use depends on your specific needs
---

## The Run Conditions Database (Condb2)

Is a PostgreSQL relational database that records and keeps track of the conditions parameters of ProtoDUNE.
It is based on the [conditions database at FNAL](https://cdcvs.fnal.gov/redmine/projects/condb/wiki/Conditions_Database_at_Fermilab)

It stores a subset of the ProtoDUNE metadata, which is kept organized in tables. Some examples are:
* Run Conditions table
* Calibration tables

## Getting started
There are two methos for installing the condb2 python API used to interact with the database.
1. Follow the instructions on their wepabge [condb2](https://fermisda-condb2.readthedocs.io/en/latest/client_installation.html)
   * Where the ConDB URL server for ProtoDUNE data is:
   ~~~
   CONDB_SERVER_URL='https://dbdata0vm.fnal.gov:9443/dune_runcon_prod'
   ~~~
3. Intall it using Spack 
    - On a dunegpvm machine on Alma9 run the following comands: 
    ~~~
    source /cvmfs/larsoft.opensciencegrid.org/spack-packages/setup-env.sh
    spack load py-condb2@2.1.4
    condb2 
    ~~~
    and it should give the following output
   
    ~~~
     condb 
       create [options] <database name> <folder_name> <column>:<type> [...]
       write  [options] <database name> <folder_name> < <CSV file>
       read   [options] <database name> <folder_name>
       put    [options] <folder_name> < <CSV file>
       get    [options] <folder_name>
       tag    [options] <folder_name> <tag name>
   ~~~
   {: .output}

## Quick look at the data
Alternatively to the REST API, the curl command can be used to quickly access the tables
~~~
curl "{CONDB_SERVER_URL}/get?folder={folder_name}&t={key}"
~~~
where {CONDB_SERVER_URL}, {folder_name}, and {key} must be replaced with the desired information. The folder_name must be replaced with the format schema.table_name (like: pdunesp.test), and the key is usually a run number or t >= 0. ProtoDUNE tables are located in the schema pdunesp. An example that retrieves data from the table pdunesp.test to get info from run 23300 is provided below. 

~~~
curl "https://dbdata0vm.fnal.gov:9443/dune_runcon_prod/get?folder=pdunesp.test&t=23300"
~~~
   
## How to upload data
> ## Alert
> Condb2 implements strong client authentication for all requests which modify the state of the database. A username and a password must be provided to create a table and upload content to the DB. Contact Ana Paula Vizcaya or Norm Buchanan to get them. 
{: .caution}



Depending on what information the user wants to store, there are different ways to create and fill out the tables.

### Creating tables
When creating a table, the condb2 API will automatically create the following columns:
1. **channel** number used to give the table another dimension.
2. **tv** key of the table, its data type is a float. There are different possibilities of how to use it and I will go into more detail later on, but they usually are: run numbers, timestamp of when the row was uploaded, or the same value for all rows.
3. **tr** timestamp in unix time of when the column data was uploaded, this is used to create the tags, or versions pointers of the tables
4. **data\_type** optional key of the table. With it, is possible to specify an extra condition, for example detector type, or data vs montecarlo

The next table shows the four columns of all conditions tables in the order in which they are created
and they have to be filled.


| channel | tv   | tr  | data_type | user_defined |
| ------  | ---  | --- | --------- | ------------ |
| 0 | 1   | 30000  | TEST | other |


The following is an example of how to create a table using the condb2 api

~~~
import os, subprocess, re

# Table information
table_name = 'pdunesp.test_prueba' #schema.table_name
host = ###
port = ###
user = ####
passw = ####
r_permission = ### # DB users to grant read permissions to
w_permission = ### #DB users to grant write permissions to
database = ###

payloads = 'start_time:float run_type:text extra_condition:hstore'
com = f'condb create -h {host} -p {port} -U {user} -w {passw} -s -R {r_permission} -W {w_permission} {database} {table_name} {payloads}'

comm = re.split(' ', com)
try:
    subprocess.run(comm)
except:
    print(f'something didnt work with the creation of the table')
~~~

