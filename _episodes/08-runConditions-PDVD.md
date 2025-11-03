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
