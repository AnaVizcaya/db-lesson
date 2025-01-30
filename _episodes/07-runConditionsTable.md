---
title: Run Conditions Table
teaching: 30
exercises: 0
questions:
- What is the Run Conditions Table?
- What data is stored in the table?
- How to access the data
objectives:
- Learn how to access and stream the data from tables in the conditions database by using the run conditions table as an example
keypoints:
- There are several APIs available to access the information in the conditions database. Which to use depends on your specific needs
---

## The Run Conditions Table
The run conditions table is stored at the ProtoDUNE conditions database.
It contains the conditions of each run, and the data can be retrieved by several methods to accommodate user needs. 

# Table information
As of now, the table contains the following metadata. 

> `Add table`
>
> Add the table
{: .caution}

## How to access the data

There are several methods available for users to access the data of the run conditions table.

The most straightforward way is to access the data with a curl command on the terminal or on the web:

~~~
curl "https://dbdata0vm.fnal.gov:9443/dune_runcon_prod/get?folder=pdunesp.run_conditionstest&t=28650"
~~~
The above example queries the conditions of run "25034" or **the closest run to "25034"**.

> `Remember`
> 1. If the given run does not exists the previous query will return the closest run.
> 2. the column representing run number is called 'tv'.
> Add the table
{: .caution}

To get the conditions of a range of runs, for example [25100,25115] use:
