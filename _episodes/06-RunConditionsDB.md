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

## Quick look at the data

The [Conditions Database](https://dbdata0vm.fnal.gov:9443/dune_runcon_prod/B/index?namespace=pdunesp) web interface shows all the table names, of the conditions database. There, it is also possible to plot some values of certain tables as shown in the following picture. 

![Example plot in conditions database web interface](/assets/img/CondEx.png)

Alternatively to the web interface, the curl command can be used to quickly access the data in the tables
~~~
curl "{CONDB_SERVER_URL}/get?folder={folder_name}&t={key}"
~~~
where {CONDB_SERVER_URL}, {folder_name}, and {key} must be replaced with the desired information. The folder_name must be replaced with the format schema.table_name (like: pdunesp.test), and the key is usually a run number or t >= 0. ProtoDUNE tables are located in the schema pdunesp. An example that retrieves data from the table pdunesp.test to get info from run 23300 is provided below. 

~~~
curl "https://dbdata0vm.fnal.gov:9443/dune_runcon_prod/get?folder=pdunesp.test&t=23300"
~~~

## Getting started with the condb2 api
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
and they have to be filled, plus a 'user_defined' column that represents the rest of the columns that the users can add.


| channel | tv  | tr  | data_type | user_defined 
| ------  | --- | --- | --------- | ------------ 
| 0 | 1  | 30000  | TEST | other 

### Table with run number as tv_key
In this type of table is possible to store information concerning a run. 
The column **tv** is used to store the run number. The column **channel** can be left with a constant value (cero), if there are no multiple channels, or it can be used as intended with one number per channel. 
The column **tr** is filled automatically with the timestamp (unix time) when the data from the row was uploaded. And the column **data\_type** is optional and can be used as an extra key, so the user can choose to specify a string or not. 

The next table is an example of a table with run number as key. The monte carlo data has just one channel and so if the data is retrieved for those run numbers it will return just one value. Since the raw data has multiple channels, if its retrieved for run 3 it will return 3 rows of data.

| channel | tv  | tr  | data_type | user_defined 
| ------  | --- | --- | --------- | ------------ 
| 0 | 1  | 1712184741 | mc_data | other 
| 0 | 2  | 1712184742 | mc_data | other 
| 0 | 3  | 1712184743 | raw_data | other 
| 1 | 3  | 1712184744 | raw_data | other 
| 2 | 3  | 1712184745 | raw_data | other 

> ## Run numbers are interpolated
> It is important to note that when using the run number ot tv_key to extract data, the function interpolates the tv_key values. So, if there is a run that does not have any column, the 'get' function will return the row with the run number closest to the value given.
{: .callout}

### Table with timestam as tv_key
In this type of table its possible to store information that changes with time, for example slow control parameters.

The column **tv** is used to store the timestamp in unix time when the data was taken. The column **channel** can be left with a constant value, if there are no multiple channels, or it can be used as intended with one number per channel. 
The column **tr** is filled automatically with the timestamp (unix time) when the data from the row was uploaded. And the column **data\_type** is optional, so the user can choose to specify a string or not. 

The next table shows an example with timestamp as tv_key. 

| channel | tv  | tr  | data_type | user_defined 
| ------  | --- | --- | --------- | ------------ 
| 0 | 1712184001  | 1712184741 | mc_data | other 
| 0 | 1712184002  | 1712184742 | mc_data | other 
| 0 | 1712184003  | 1712184743 | raw_data | other 
| 1 | 1712184003  | 1712184744 | raw_data | other 
| 2 | 1712184003  | 1712184745 | raw_data | other 

### Example of how to create a table using the condb2 python API
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

### Uploading data to the table
Now its time to upload data to the previously created table. 

The following examples use the ucondb2 API.

#### Using the command line interface
~~~
condb2 put [options] <folder_name> <CSV file>

  Options:
      -s <server URL>             CONDB_SERVER_URL envirinment variable can be used too
      -U <username>
      -w <password>
      -d <data type>
~~~

#### Using the web access python API
In the following example it is possible to fill out all the columns of the table or specify which ones to fill
~~~
from condb2 import ConDBClient

client = ConDBClient('https://dbdata0vm.fnal.gov:9443/dune_runcon_prod')
folder = 'pdune.ExampleTable'

data = [0, 18000, "test"]   #Starting with channel, tv, 
columns = [“channel”, “tv”, "user_defined"]   #Specify columns to fill or leave blank for all
client.put_data(folder, data, columns,  data_type="pdune_hd") # data type can be used as another key in the table
~~~

### Tag a table
It is possible to **tag** a table to create different versions. The API will always returns values from the 'newer' version unless otherwise specified. The tag is related to the **tr** column of when the data was uploaded. It is also possible to leave coments when creating a new table version.

The following is an example of how to create a new version (v1.001) of the table "ExampleTable" in the namespace "pdune" using the ucondb2 API and using the web access python API 
~~~
from condb2 import ConDBClient

client = ConDBClient('https://dbdata0vm.fnal.gov:9443/dune_runcon_prod')
folder = 'pdune.ExampleTable'

client.tag_state(folder, tag='v1.001', tr=None, copy_from=None, override=False)
~~~

It is also possible to look at the tags that a table already has. For that we need to use the Direct Access Python API. In general, it is better to use the web access python API, and more information can be found in the [condb2](https://fermisda-condb2.readthedocs.io/en/latest/client_installation.html) webpage. The following is an example to look at the tags.

~~~
from condb2 import ConDB

def connect(self):
    host    =  ### 
    port    =  ###
    db_name =  ###
    user    =  ###
    passw   =  ###
    connstr = f'host={host} port={port} dbname={db_name} user={user} password={passw}'
    db = ConDB(connstr = connstr)
    return db.openFolder('pdune.ExampleTable')

folder = connect()

tags = folder.tags()
for tag in tags:
            print(tag, 'This is the tag')
~~~

### More in depth look at the data
It is possible to examine the data by appliying conditions to the table columns to filter resutls.
~~~
from condb2 import ConDBClient

client = ConDBClient('https://dbdata0vm.fnal.gov:9443/dune_runcon_prod')
folder = 'pdune.ExampleTable'

# get_data example - Get data from given run
run = 18000
columns, data = client.get_data(table, t0=run)
print("columns:", ','.join(columns))
for line in data:
    print(line)

# search_data example - Search data, or runs that comply with the following conditions
con = [("run_type","=",'PROD'),("buffer",">=",0)] # Example conditions on the data
col, data1 = client.search_data(folder, conditions=con)
print("columns:", ','.join(col))
for line in data1:
    print(line)
~~~
