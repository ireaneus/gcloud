# TerramEarth Case Study

## Company Overview
TerramEarth manufactures heavy equipment for the mining and agricultural industries. About 80% of their business is from mining and 20% from agriculture. They currently have over 500 dealers and service centers in 100 countries. Their mission is to build products that make their customers more productive.

## Solution Concept
There are 20 million TerramEarth vehicles in operation that collect 120 fields of data per second. Data is stored locally on the vehicle and can be accessed for analysis when a vehicle is serviced. The data is downloaded via a maintenance port. This same port can be used to adjust operational parameters, allowing the vehicles to be upgraded in the field with new computing modules.

Approximately 200,000 vehicles are connected to a cellular network, allowing TerramEarth to collect data directly. At a rate of 120 fields of data per second, with 22 hours of operation per day, TerramEarth collects a total of about 9 TB/day from these connected vehicles.

## Existing Technical Environment
TerramEarth’s existing architecture is composed of Linux based systems that reside in a data center. These systems gzip CSV ¡les from the field and upload via FTP, transform and aggregate them, and place the data in their data warehouse. Because this process takes time, aggregated reports are based on data that is 3 weeks old.

With this data, TerramEarth has been able to preemptively stock replacement parts and reduce unplanned downtime of their vehicles by 60%. However, because the data is stale, some customers are without their vehicles for up to 4 weeks while they wait for replacement parts.

## Business Requirements
- Decrease unplanned vehicle downtime to less than one week
- Support the dealer network with more data on how their customers use their equipment to better position new products and services
- Have the ability to partner with different companies—especially with seed and fertilizer suppliers in the fast-growing agricultural business—to create compelling joint offerings for their customers

## Technical Requirements:
- Expand beyond a single data center to decrease latency to the American Midwest and East Coast
- Create a backup strategy
- Increase security of data transfer from equipment to the data center
- Improve data in the data warehouse
- Use customer and equipment data to anticipate customer needs

## Application 1: Data Ingest:
*A custom Python application reads uploaded data files from a single server, writes to the data warehouse*

### Compute:

- Windows Server 2008 R2
    - 16 CPUs
    - 128 GB of RAM
    - 10 TB local HDD storage

## Application 2: Reporting
*An off-the-shelf application that business analysts use to run a daily report to see what equipment needs repair. Only two analysts of a team of 10 (five West Coast, five East Coast) can connect to the reporting application at a time.*

### Compute

- Off-the-shelf application. License tied to number of physical CPUs
    - Windows Server 2008 R2
    - 16 CPUs
    - 32 GB of RAM
    - 500 GB HDD

### Data warehouse

- A single PostgreSQL server
    - RedHat Linux
    - 64 CPUs
    - 128 GB of RAM
    - 4x 6TB HDD in RAID 0
 
## Executive statement:
>Our competitive advantage has always been in our manufacturing process, with our ability to build better vehicles for lower cost than our competitors. However, new products with different approaches are constantly being developed, and I’m concerned that we lack the skills to undergo the next wave of transformations in our industry. My goals are to build our skills while addressing immediate market needs through incremental innovations.