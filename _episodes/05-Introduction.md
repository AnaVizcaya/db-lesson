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


# The Conditions Database
HEP experiments take bast amounts of data with the main detectors, but they also have a lot of data, also known as metadata, coming from other sources. This metadata describes the data coming from the read-out of the primary detectors. 

> ## Conditions data
> The conditions data is a subset of the experimental metadata, specifically referring to the metadata required for offline data analysis and reconstruction.
> The conditions data of ProtoDUNE is stored in a dedicated database, **the Conditions Database.**
{: .callout}

The conditions database consists of two PostgreSQL databases.
1. **The master store of metadata (UConDB) database** is the centralized place where all the information is stored as blobs. Adding new information is straightforward, and there is no need to have a predefined database schema.
2. **The run conditions database** stores a subset of the UConDB metadata in a table. This facilitates querying the metadata with conditions on the table values, for example to get all the runs with certain characteristics, like runs with High Voltage = 175 kV.

![Conditions database architecture](/assets/img/dbarc.png)

An illustration of both ProtoDUNE's metadata stream and the design of the conditions database. The direction of the metadata stream is shown by the arrows. Users may obtain the metadata by interacting with the conditions database APIs.

The conditions database has the following characteristics:
* It contains metadata, uploaded daily via cron jobs, from several parts of the experiment, such as: DAQ configurations, slow control parameters, beam instrumentation, data quality, and calibration parameters.
* The database allows the stored metadata to be index by time (like slow controls), or by run (like DAQ run configurations).
* The following API's facilitate the interaction of users with the conditions data: an interface with _art_ which is the framework used by the offline software to process LArTPC data from ProtoDUNE; a python rest API; a C++ API; and an interface with Metacat which is ProtoDUNE's metadata file catalog.
* There is detailed documentation on how to install and use the UconDB and the run conditions database python API's that, amongst other things, lets users upload or download data from the databases. 
* Admin users are the only ones allowed to upload data to the databases. Nevertheless all users can view and download the data. 


## The master store of metadata (UConDB) database
ProtoDUNE UConDB has a collection of **folders** which in turn can have a collection of **objects**. Each object has a unique **key** that should be used to retrieve the information. The **key** can be the run number or a timestamp.

The ucondb will store all ProtoDUNE metadata in the folder **protodune_conditions**. Each kind of metadata will have it's own object, for example: 
* daq_config_v2 - the configuration of the DAQ processes in each run and the configuration of the front-end electronics
* ifbeam_imagnet - the current of the magnet "" coming from the IFbeam DB
* ifbeam_momentum - the beam central momentum calculated using the magnet current
* np04_hv_setting - the setting of the high voltage comming from the slow cotrols DB

### Getting started 
There are different ways to access and upload/download the contents of the ucondb. 

A REST API was created by Igor V Mandrichenko. It also contains a command line interface. Instructions on how to install it and how to use it can be found in the [UConDB documentation](https://ucondb.readthedocs.io/en/latest/).

The UConDB URL server must be provided, for ProtoDUNE data use: 
~~~
export UCONDB_SERVER_URL='https://dbdata0vm.fnal.gov:9443/protodune_ucon_prod/app'
~~~
The ProtoDUNE folder should also be provided which is: protodune_conditions 

### Quick Access
Alternatively to the REST API, the curl command can be used to retrieve the data:
~~~
curl -o output.file "{UCONDB_SERVER_URL}/get?folder={folder_name}&object={object_name}&tv={key}"
~~~
where {UCONDB_SERVER_URL}, {folder_name}, {object_name}, and {key} must be replaced with the desired information. An example that retrieves data from the folder named test, the object named test, usesing the run key of 12008 is provided below. 
~~~
curl -o output.file "https://dbdata0vm.fnal.gov:9443/protodune_ucon_prod/app/get?folder=test&object=test&tv=12008"
~~~
For ProtoDUNE data the folder name protodune_conditions should be used. 
> ## Remember
> ProtoDUNE data is stored in the folder: protodune_conditions 
{: .callout}

### Upload data to the database
UConDB implements strong client authentication for all requests which modify the state of the database. A username and a password must be used to upload content to the DB. Contact Ana Paula Vizcaya or Norm Buchanan from the database group to get them. 

The command line interface of the python web API can be used to upload data, as explained in it's documentation webpage.

Alternatively, the curl command can also be used:
~~~
curl -T data.file --digest -u user:password -X PUT "{UCONDB_SERVER_URL}/data/{folder_name}/{object_name}/key={key}"
~~~
where {UCONDB_SERVER_URL}, {folder_name}, {object_name}, and {key} must be replaced with the desired information. The key can be a float, usually corresponding to the run number or timestamp.

