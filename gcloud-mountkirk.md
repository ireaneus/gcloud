# Mountkirk Games Case Study

## Company Overview
>Mountkirk Games makes online, session-based, multiplayer games for mobile platforms. They build all of their games using some server-side integration. Historically, they have used cloud providers to lease physical servers.

>Due to the unexpected popularity of some of their games, they have had problems scaling their global audience, application servers, MySQL databases, and analytics tools.

>Their current model is to write game statistics to files and send them through an ETL tool that loads them into a centralized MySQL database for reporting.

## Solution Concept:
>Mountkirk Games is building a new game, which they expect to be very popular. They plan to deploy the game’s backend on Google Compute Engine so they can capture streaming metrics, run intensive analytics, and take advantage of its autoscaling server environment and integrate with a managed NoSQL database.

## Business Requirements:
- Increase to a global footprint
- Improve uptime—downtime is loss of players
- Increase efficiency of the cloud resources we use
- Reduce latency to all customers

## Technical Requirements
*Requirements for Game Backend Platform*
1. Dynamically scale up or down based on game activity
2. Connect to a transactional database service to manage user profiles and game state
3. Store game activity in a timeseries database service for future analysis
4. As the system scales, ensure that data is not lost due to processing backlogs
5. Run hardened Linux distro

*Requirements for Game Analytics Platform*
1. Dynamically scale up or down based on game activity
2. Process incoming data on the fly directly from the game servers
3. Process data that arrives late because of slow mobile networks
4. Allow queries to access at least 10 TB of historical data
5. Process files that are regularly uploaded by users’ mobile devices

## Executive Statement
>Our last successful game did not scale well with our previous cloud provider, resulting in lower user adoption and affecting the game’s reputation. Our investors want more key performance indicators (KPIs) to evaluate the speed and stability of the game, as well as other metrics that provide deeper insight into usage patterns so we can adapt the game to target users. Additionally, our current technology stack cannot provide the scale we need, so we want to replace MySQL and move to an environment that provides autoscaling and low latency load balancing and frees us up from managing physical servers.

### What are their goals (Solution Concept)
- building new game, no migration necessary
- environments
	- Create game backed on GCE
		- Hardened Linux distro
		- NoSQL transactional database - Datastore (Firestore)
	- Separate analytics setup 
	- Different storage service for each

### What do they care about
- Scaling
- Measuring performance - increase efficiency of solutions - Stackdriver Monitoring/logging
- Reliable experience for users - no downtime
- Managed services
- Analytics - usage patterns
- Global footprint

### Business requirements
- Increase to a global footprint
	- multiple regional *instance group* backends
		- Single global *HTTP load balancer*
	- multi-regional ingest/storage options
		- Pub/Sub, Datastore, bigquery, cloud storage

### Exam information 
> **Cloud Datastore (Firestore) - NoSQL transactional database** game user profiles and game states
> **Store in BigQuery**
- BigQuery vs Bigtable
	- Bigtable = millisecond response time
	- BigQuery = response measured in seconds, scales
	- BigQuery reading from Bigtable also a valid answer, but not best answer

> Process incoming data streaming  - Pub/Sub process with Dataflow
> Process data that arrives late because of slow mobile networks
	- Pub/Sub *scales and buffers messages*
	- Dataflow *accounts for late/out of order data

