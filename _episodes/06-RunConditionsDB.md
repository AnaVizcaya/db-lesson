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

## The Run Conditions Database

Is a PostgreSQL relational database that records and keeps track of the conditions parameters of ProtoDUNE.
It is based on the [conditions database at FNAL](https://cdcvs.fnal.gov/redmine/projects/condb/wiki/Conditions_Database_at_Fermilab)

It stores a subset of the ProtoDUNE metadata, which is kept organized in tables.

## Installing the python API
There are two methos for installing the condb2 python API used to interact with the database.
1. Follow the instructions on their wepabge [condb2](https://fermisda-condb2.readthedocs.io/en/latest/client_installation.html)

## How to upload data
