---
title: Run Conditions Table
teaching: 30
exercises: 0
questions:
- What is the Run Conditions Table?
- What data is stored in this table?
- How to access the data?
objectives:
- Learn how to access and stream the data from tables in the conditions database by using the run conditions table as an example
keypoints:
- There are several APIs available to access the information in the conditions database. Which to use depends on your specific needs.
---

## The Run Conditions Table
The run conditions table is stored at the ProtoDUNE conditions database.
It contains the conditions for each run, and the data can be accessed through various methods to suit different user needs.

# Table information
As of now, the table contains the following metadata:

|          | tv (run) | tr | data_type | upload_time | start_time | stop_time | run_type | detector_id |software_version 
| -------- | -------- | ---| ----------| ------------| -----------| ----------| ---------| ------------| ----------------
| Unit | N/A | Unix | N/A | Unix | Unix | Unix | N/A | N/A | N/A | N/A | N/A | N/A | N/A | N/A | mV/fC | N/A | pA | N/A | pA | us | N/A
| Example | 25034 | 1713497099.738875 | np02_coldbox or np04_hd| 1713497099.7388604 | 1713268519.0 | 1713269109.0 | PROD | np02_coldbox or np04_hd or np02_hermes_WIB_conf | fddaq-v4.4.0-rc3-a9 
|Comment| Run number| Used for versioning | | | |  | data_stream | | 

| data_quality | ac_couple | baseline | buffering | enabled | gain | gain_match | leak | leak_10x |leak_f | peak_time | pulse_dac
| --- | -------- | -------- | ---| ----------| ------------| -----------| ----------| ---------| --------|-----| ---
| good or bad | dc_coupling or ac_coupling |2 |0 |True |14.0 | True | 500.0 | False | None | 2.0 | 0
| offline good runs | | 0 (900 mV), 1 (200 mV), 2 (200 mV collection, 900 mV induction) | 0 (no buffer), 1 (se buffer), 2 (sedc buffer) | True of FEMB should be configured and read out by WIB |Options: 14, 25, 7.8, 4.7 mV/fC | Enable pulser DAC gain matching | | Multiply leak current by 10 if true | final leak value | Channel peak time selector|Pulser DAC setting [0-63]

|strobe_delay | strobe_length | strobe_skip | test_cap | adc_test_pattern | cold | detector_type | pulser
| --- | -------- | -------- | ---| ----------| ------------| -----------| ---------
| N/A | N/A | N/A | N/A | N/A | N/A | N/A | N/A
| 255 | 255 | 255 | False | False | False | wib_default | False
| 64MHz periods to skip after 2MHz edge for strobe (pulser offset 0-255) | Length of strobe in 64MHz periods (pulser length 0-255) | 2MHz periods to skip after strobe (pulser period 0-255) | Enable the test capacitor | True if the COLDADC test pattern should be enabled | True if the front end electronics are COLD (77k) | Options: WIB default, upper APA, lower APA, CRP | True if the calibration pulser should be enabled

| beam_momentum | beam_polarity | detector_hv | wire_bias_g | wire_bias_u | wire_bias_x | lar_purity | lar_top_temp_mean | lar_bottom_temp_mean
| --- | -------- | -------- | ---| ----------| ------------| -----------| --------- | ----
| GeV/c | N/A | V |  | |  |  | |
| +5 | positive or negative | 175000 |  | |  |  | |
| indirectly calculated using magnet MBPL.022.692 current, and rounded up| looking at magnet MBPL.022.692 current | from sensorID: 47894774153498 which is NP04_DCS_01:Heinz_V |  | |  |  | |

> ## Add table
> Add the table
{: .caution}

## How to access the data

### Curl command
There are several methods available for users to access the data of the run conditions table.
The most straightforward way is to access the data with a curl command on the terminal or on the web.

> ## What you need to know
> 1. The database url. For our example: https://dbdata0vm.fnal.gov:9443/dune_runcon_prod
> 2. The table name. For our example: pdunesp.run_conditionstest
{: .checklist}

~~~
curl "https://dbdata0vm.fnal.gov:9443/dune_runcon_prod/get?folder=pdunesp.run_conditionstest&t=28650"
~~~
The above example queries the conditions of run "25034" or **the closest run to "25034"**.

> ## Remember
> 1. If the given run does not exists the previous query will return the closest run.
> 2. the column representing run number is called 'tv'.
> Add the table
{: .caution}

To get the conditions of a range of runs, for example [25100,25115] use:

~~~
curl "https://dbdata0vm.fnal.gov:9443/dune_runcon_prod/get?folder=pdunesp.run_conditionstest&t0=28650&t1=28655"
~~~

## Python API (Condb2)
Detailed documentation about the conditions database python REST API can be found in the [condb2](https://fermisda-condb2.readthedocs.io/en/latest/client_installation.html) webpage.

To use the API, it is necessary to install it, as it is explained in the webpage. Once that is done, it is recommended to use the condb web access python API, instead of the direct access. For the run conditions table, the url that must be provided is: 

1. The database url: https://dbdata0vm.fnal.gov:9443/dune_runcon_prod
2. The table/folder name: pdunesp.run_conditionstest

The web API consists of some functions to retrieve or upload data, as well as create table versions. Detailed explanation on how to use them can be found in the web access python API webpage. To use the functions get_data or search_data it is not necessary to provide username or password, nevertheless they are required to use the function put_data. To get a valid username and password contact Ana Paula Vizcaya or Norm Buchanan. They can also provide some example scripts. 

## c++ interface and art service
The run conditions table uses the c++ interface and art service of the conditions database mentioned in the [conditions database](https://wiki.dunescience.org/wiki/Conditions_Database_(ProtoDUNE)) of ProtoDUNE webpage.

It is located with the dunecalib service. The github repository is [dunecalib](https://github.com/DUNE/dunecalib). And a presentation with information on how to use the run conditions c++ interface and service can be found [here](https://indico.fnal.gov/event/63876/contributions/286958/attachments/176138/240028/DB-meeting-19March2024.pdf).

### C++ interface
To start using run conditions parameters include the following header files on your c++ file
~~~
#include "dunecalib/ConInt/RunConditionsProtoDUNE.h"
#include "nuevdb/IFDatabase/Table.h"
~~~
and set up the table to upload

~~~
condb::RunConditionsProtoDUNE* runCond = new condb::RunConditionsProtoDUNE();
runCond->SetTableURL("https://dbdata0vm.fnal.gov:9443/dune_runcon_prod/");
runCond->SetTableName("pdunesp.run_conditionstest");
runCond->SetVerbosity(0); // How much output, (0,3) - (none, more)
runCond->SetRunNumber1(0); //Change if a range of runs is desired
runCond->UpdateRN(25034); //Run Number
//runCond->SetTag(gDBTag); // If database has more than one version
runCond->LoadConditionsT();
~~~
Now you are ready to load and use the conditions parameters! The following is just one example to output some conditions on the terminal: 

~~~
condb::RunCond_t rc = runCond->GetRunConditions(run);
std::cout << "\tStart time = " << rc.start_time
            << "\n\tdata type = " << rc.data_type
            << "\n\trun Number/sofw = " << rc.run_number
  	        << "\n\tupload time = " << rc.upload_t
            << "\n\tsoftware version = " << rc.software_version
            << "\n\tstop_time = " << rc.stop_time 
            << "\n\tbuffer = " << rc.buffer
            << "\n\tac_couple = " << rc.ac_couple
            << "\n\trun type = " << rc.run_type << std::endl;

~~~

A complete example script can be found in: dunecalib/dunecalib/ConInt/getRunConditionsPDUNE.cc and that example can be run as follows: 

~~~
getRunConditionsPDUNE -r 25016
~~~
