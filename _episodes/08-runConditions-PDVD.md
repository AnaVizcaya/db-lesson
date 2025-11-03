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

| data_stream | data_quality | ac_couple | baseline | buffering | enabled | gain | gain_match | leak | leak_10x |leak_f | peak_time | pulse_dac
| --- | --- | -------- | -------- | ---| ----------| ------------| -----------| ----------| ---------| --------|-----| ---
| N/A | N/A | N/A | N/A | N/A | N/A | mV/fC | N/A | pA | N/A | pA | us | N/A
| cosmic, physics or calibration | good or bad | dc_coupling or ac_coupling |2 |0 |True |14.0 | True | 500.0 | False | None | 2.0 | 0
|   | offline good runs | | 0 (900 mV), 1 (200 mV), 2 (200 mV collection, 900 mV induction) | 0 (no buffer), 1 (se buffer), 2 (sedc buffer) | True of FEMB should be configured and read out by WIB |Options: 14, 25, 7.8, 4.7 mV/fC | Enable pulser DAC gain matching | | Multiply leak current by 10 if true | final leak value | Channel peak time selector|Pulser DAC setting [0-63]

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
