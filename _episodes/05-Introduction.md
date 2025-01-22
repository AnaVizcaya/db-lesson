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
An illustration of both ProtoDUNE's metadata stream and the design of the conditions database. The direction of the metadata stream is shown by the arrows. Users may obtain the metadata by interacting with the conditions database APIs.

## Do local setup for local rendering (optional)

Follow the instructions [https://carpentries.github.io/lesson-example/setup.html#setup-for-local-rendering-of-the-lessons-optional](https://carpentries.github.io/lesson-example/setup.html#setup-for-local-rendering-of-the-lessons-optional) for setup on your local machine - in principle this is optional but in practice it is really helpful.  You are going to need ruby and pyYAML.  I used conda on a mac but they have instructions for Windows, Mac and UNIX. 

> ## Alert 
> At this point you should stop following their instructions and start using our github template to avoid overwriting DUNE specific items. 
{: .caution}
