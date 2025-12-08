---
title: Run Conditions Table of ProtoDUNE-VD
teaching: 30
exercises: 0
questions:
- What is the ProtoDUNE-VD Run Conditions Table?
- What data is stored in this table?
- How to access the data?
objectives:
- Learn how to access and stream the data from tables in the conditions database by using the run conditions table as an example
keypoints:
- There are several APIs available to access the information in the conditions database. Which to use depends on your specific needs.
---

## The ProtoDUNE-VD Run Conditions Table
The ProtoDUNE-VD run conditions table is stored at the ProtoDUNE conditions database.
It contains the conditions for each run, and the data can be accessed through various methods to suit different user needs.

# Table information
As of now, the table contains the following metadata:

|          | tv (run) | tr | data_type | upload_time | start_time | stop_time | run_type | detector_id |software_version 
| -------- | -------- | ---| ----------| ------------| -----------| ----------| ---------| ------------| ----------------
| Unit | N/A | Unix | N/A | Unix | Unix | Unix | N/A | N/A | N/A 
| Example | 25034 | 1713497099.738875 | np02_coldbox or np04_hd| 1713497099.7388604 | 1713268519.0 | 1713269109.0 | PROD | np02_coldbox or np04_hd or np02_hermes_WIB_conf | fddaq-v4.4.0-rc3-a9 
|Comment| Run number| Used for versioning | | | |  | data_stream | | 

| data_stream | data_quality | ac_couple | baseline | gain | peak_time | pulser | apas | test_cap
| --- | --- | -------- | -------- | --------|-----| --- | --- | ---
| N/A | N/A | N/A | N/A | mV/fC | us | N/A | N/A | N/A
| cosmic, physics or calibration | good or bad | dc_coupling or ac_coupling |2 |14.0 | 2.0 | False | 2,3 | False
|   | offline good runs | | 0 (900 mV), 1 (200 mV), 2 (200 mV collection, 900 mV induction) |Options: 14, 25, 7.8, 4.7 mV/fC | Channel peak time selector| True if the calibration pulser should be enabled | list of APAs | Enable the test capacitor


| beam_momentum_mean | beam_momentum_std | beam_momentum_set | beam_polarity | detector_hv_mean | detector_hv_std | detector_hv_set  | lar_top_temp_mean | lar_bottom_temp_mean
| --- | -------- | -------- | -------- | -----|  ------| -------| ----- | ----
| GeV/c | GeV/c | GeV/c | N/A | V | V | V |   |
| +5.352 | 0.042 | +5 | positive or negative | 176371.3461 | 10.1325 | 176000.0 |   |
| indirectly calculated using magnet current | | Set value of beam momenutm of the run | looking at magnet current | from sensorID | | Set hv for the run|   |

## How to access the data

### Curl command
There are several methods available for users to access the data of the run conditions table.
The most straightforward way is to access the data with a curl command on the terminal or on the web.

> ## What you need to know
> 1. The database url. For the protoDUNE-VD run conditions table: **https://dbdata0vm.fnal.gov:9443/dune_runcon_prod**
> 2. The table name. For the protoDUNE-VD run conditions table: **pdunesp.run_conditions_vd**
{: .checklist}

~~~
curl "https://dbdata0vm.fnal.gov:9443/dune_runcon_prod/get?folder=pdunesp.run_conditions_vd&t=39252"
~~~
The above example queries the conditions of run "39252" or **the closest run to "25034"**.

> ## Remember
> 1. If the given run does not exists the previous query will return the closest run.
> 2. The column representing **run number** is called **tv**.
{: .caution}

To get the conditions of a range of runs, for example [25100,25115] use the following query:
~~~
curl "https://dbdata0vm.fnal.gov:9443/dune_runcon_prod/get?folder=pdunesp.run_conditions_vd&t0=39252&t1=39260"
~~~

## Python API (Condb2)
Detailed documentation about the conditions database python REST API can be found in the [condb2](https://fermisda-condb2.readthedocs.io/en/latest/client_installation.html) webpage.

To use the API, it is necessary to install it, as it is explained in the webpage. Once that is done, it is recommended to use the condb2  *web access python API*, instead of the direct access. For the rprotoDUNE-VD run conditions table, the url that must be provided is: 

1. The database url: **https://dbdata0vm.fnal.gov:9443/dune_runcon_prod**
2. The table/folder name: **pdunesp.run_conditions_vd**

The  *web access python API* includes functions for retrieving and uploading data, as well as creating table and table versions. Detailed documentation on how to use these functions can be found in the *web access python API* webpage. To use the functions get_data or search_data it is not necessary to provide username or password, nevertheless they are required to use the function put_data. To get a valid username and password contact Ana Paula Vizcaya or Norm Buchanan from the database team.

## c++ interface and art service
The run conditions table uses the c++ interface and art service of the conditions database mentioned in the [conditions database](https://wiki.dunescience.org/wiki/Conditions_Database_(ProtoDUNE)) of ProtoDUNE webpage.

It is located with the dunecalib service. The github repository is [dunecalib](https://github.com/DUNE/dunecalib). A presentation with information on how to use the run conditions c++ interface and service can be found [here](https://indico.fnal.gov/event/63876/contributions/286958/attachments/176138/240028/DB-meeting-19March2024.pdf).

### C++ interface
To start using run conditions parameters include the following header files on your c++ file
~~~
#include "dunecalib/ConInt/RunConditionsProtoDUNE.h"
#include "nuevdb/IFDatabase/Table.h"
~~~
and set up the table to upload:

~~~
condb::RunConditionsProtoDUNE* runCond = new condb::RunConditionsProtoDUNE();
runCond->SetTableURL("https://dbdata0vm.fnal.gov:9443/dune_runcon_prod/");
runCond->SetTableName("pdunesp.run_conditions_vd");
runCond->SetVerbosity(0); // How much output, (0,3) - (none, more)
runCond->SetRunNumber1(0); //Change if a range of runs is desired
runCond->UpdateRN(39000); //Run Number
//runCond->SetTag(gDBTag); // If database has more than one version
runCond->LoadConditionsT();
~~~
Now you are ready to load and use the conditions parameters! The following is just one example to output some conditions on the terminal:
