---
title: Introduction To The Conditions Database
teaching: 30
exercises: 0
questions:
- What is the conditions database?
- What kind of data is stored in this database?
objectives:
- Learn the basic structure and what information is stored on the conditions database
keypoints:
- There are several APIs available to access the information in the conditions database. Which to use depends on your specific needs
---


## The Conditions Database
HEP experiments take bast amounts of data with the main detectors, but they also have a lot of data, also known as metadata, coming from other sources. This metadata describes the data coming from the read-out of the primary detectors. 

> ## Conditions data
> The conditions data is a subset of the experimental metadata, specifically referring to the metadata required for offline data analysis and reconstruction.
> The conditions data of ProtoDUNE is stored in a dedicated database, **the Conditions Database.**
{: .callout}

The conditions database consists of two PostgreSQL databases.
1. **The master store of metadata (UConDB) database** is the centralized place where all the information is stored as blobs. Adding new information is straightforward, and there is no need to have a predefined database schema.
2. **The run conditions database** stores a subset of the UConDB metadata in a table. This facilitates querying the metadata with conditions on the table values, for example to get all the runs with certain characteristics, like runs with High Voltage = 175 kV.

![Conditions database architecture](/fig/dbarc.png)

or try another figure

![Conditions database architecture](fig/dbarc.png)

An illustration of both ProtoDUNE's metadata stream and the design of the conditions database. The direction of the metadata stream is shown by the arrows. Users may obtain the metadata by interacting with the conditions database APIs.

The conditions database has the following characteristics:
* It contains metadata, uploaded daily via cron jobs, from several parts of the experiment, such as: DAQ configurations, slow control parameters, beam instrumentation, data quality, and calibration parameters.
* The database allows the stored metadata to be index by time (like slow controls), or by run (like DAQ run configurations).
* The following API's facilitate the interaction of users with the conditions data: an interface with _art_ which is the framework used by the offline software to process LArTPC data from ProtoDUNE; a python rest API; a C++ API; and an interface with Metacat which is ProtoDUNE's metadata file catalog.
* There is detailed documentation on how to install and use the UconDB and the run conditions database python API's that, amongst other things, lets users upload or download data from the databases. 
* Admin users are the only ones allowed to upload data to the databases. Nevertheless all users can view and download the data. 
