# Google Cloud Certified Professional Cloud Architect
[Back](README.md) to README.md

Table of Contents
- [Exam info](#exam-information)
- [Core Management Services](#core-management-services)
- [Cloud IAM](#cloud-iam)
- [Billing](#billing)

# Exam Information
Exam tested on:
- Exam is 50 questions - pass or fail
- multiple choice 

You are tested on your:
- Knowledge of what Google’s cloud services are and their use cases
- Understanding of how individual services work
- Understanding of how Google’s services are intended to fit together
- Concepts wide range of topics
- Business requirements - HA reduce costs - most correct answer
- 3 possible case studies
- Hands on experience - create experiment break it fix it

The exam asks you to make the best choice
- The right choice is not always obvious
- Look for keywords that give you a hint

Study areas 
* Deployment manager
* Cloud Storage
* Cloud endpoint
* Best practices and concepts
* CI/CD
* Tensorflow
* Automate deployments using Google App Engine
* Deploy hybrid cloud applications using Container Engine and Kubernetes
* Build microservices using Google Cloud Functions
* Persistent Volumes, using persistent disks in GCE, survive instance and container restarts.
* Deprecated - Works warning
* Obsolete - new users cannot use it
* Deleted - all users cannot use it
* Windows Server requires scripts in GCS to be public https://cloud.google.com/compute/docs/startupscript
* Endpoints Frees developers from writing wrapper to access App Engine resources from a mobile or web client
* Kubernetes change the number of node in an existing cluster - gcloud container clusters resize new target number
* If asked if changing machine type in kubernetes while cluster is running - migrate to new cluster
* Continuous delivery is a DevOps software development practice where code changes are automatically built, tested, and prepared for a release to production.
* With continuous deployment, revisions are deployed to a production environment automatically without explicit approval from a developer, making the entire software release process automated
* Before you can make requests to Storage Transfer Service, you must make sure the Cloud Storage Transfer Service API is enabled for your project, and that your application is set up for authorization, using the OAuth 2.0 protocol.

# Core Management Services
### Resource Hierarchy
- Core principles
  - Each child object has only one parent
  - Permissions is inherited from top down
  - Child nodes inherit parent permissions
  - More permissive parent policy always overrules more restrictive child policy

### GC Resource Hierarchy
*Organization Resource*
- Represents an organization
- IAM Access control policies applied to the Organization resource are applied throughout the entire hierarchy
- Can grant access to different people in org.
- Not applicable to personal gmail accounts
- Organization - Key roles
  - Organization admin: Full power to edit all permissions
  - Organization owner: Reserved for G Suite/Cloud Identity super admin
*Folders*
- Additional (optional) grouping and isolation boundary between projects
- Collection of projects and other folders
- IAM roles applied to folder apply to all projects inside
- Useful for grouping by departments
- Useful for delegating admin rights
- Beware: Removing projects from folder will remove folder-applied IAM roles
*Projects*
- CORE organizational component of GCP
- Basis for creating, enabling, using, and paying for GCP services (i.e., everything)
- Exam tip: Become VERY familiar working with and managing projects
- Identifiers:
  - Project ID (must be globally unique)
  - Project number (automatically generated)
  - Project name (friendly name)
    - Good practice to have identical Project name and ID
### Resources
Everything that is created and used on GCP
- instances
- services
- APIs
- Cloud Storage buckets
- Managed services
- IAM policies
### Labels
- Key-value pair
- Key: unique identifier, cannot be empty, up to 64 labels per resource

```sh
[user@linuxserver] gcloud compute instances create [instance-name] --labels key=value,key=value --zone us-west1-a
[user@linuxserver] gcloud compute instances describe instance-2 --zone us-west1-a --format 'default(labels)'
[user@linuxserver] gcloud compute instances update instance-2 --zone us-west1-a --update-labels owner=user2,state=readyfordeletion
[user@linuxserver] gcloud compute instances update instance-2 --zone us-west1-a --remove-labels key1,key2
```

### Quotas
- Caps on resources on a per project basis
- Quota can be increased via support ticket

# Cloud IAM
### IAM
- Use more restricted rights at the higher levels
- Resources added at lower levels will inherit those restrictions
- Privileges can be added at lower levels when necessary
- Use service accounts and roles to secure machines and applications
- Manage users in groups when possible
- Audit your logs
- Use roles to enforce the principle of least privilege
- Use projects for separate duties
- Cloud Platform supports SAML 2.0-based SSO, which provides seamless SSO against Cloud Platform Console, web- and command-line-based SSH, and OAuth authorization prompts, the user is prompted for their username but not their password.

### Service Accounts
- Usually a member is a human
- Service accounts are special accounts that identify machines or applications
- Service accounts are created in the form of an email [user@email.com] [googleservice@gservice.com]
- Service account is created when creating an API
- Service accounts can be assigned one or more roles
- A computer or application can run using a service account identity that machine will only be allowed to perform actions allowed by its roles
- gservice generated account uses access scopes, whereas user created service accounts have scopes predefined
- user accounts will need service account user role in order for them to ssh into compute instances

```sh
[admin@instance-1]$ gcloud config list
[core]
account = 2345665412-compute@developer.gserviceaccount.com
disable_usage_reporting = True
project = PROJECT_ID

Your active configuration is: [default]
```

### Roles
*Primitive Roles*
- Are project-wide roles
- Project viewer can see everything in a project
- Project editor can change everything in a project
- Project owner has all rights of editor and can add members
- The primitive roles do not provide fine-grained control over what members can do

*Predefined Roles*
- One role can’t remove permissions granted by another role
- For example, if you make someone a Project Owner and a Storage Viewer, they have read-write access to storage
- App Engine Admin, BigQuery User, or DataStore Viewer are examples of predefined roles
- Many predefined roles exist
- Principle of least privilege

### Members - Person = Google Account
- [Admin console](https://admin.google.com/AdminHome)
  - Users
  - Billing
- Google account always associated with email address
- Google Account Types: Personal, GSuite Domain, Cloud Identity Domain, Google Group, allAuthenticatedUsers, allUsers

### IAM Policies
### Organization Level
Must be the Administrator account for the organization
- IAM Policy - default menu option
  - Shows Members and Roles
    - Organization Administrator - All powerful
    - Select top-level organization
      - add member - user@email.com - Roles: Organization viewer, Resource Manager role - Project creator, Billing account user  --  Create resources in project
### Project Level
- Select Project 
  - IAM & Admin web menu
    - Shows Members and Roles
    - Add user@email.com - Role project viewer
### Command line

```
[root@cloudserver]# gcloud projects get-iam-policy PROJECT_ID > filename.yml

[root@cloudserver]# gcloud projects set-iam-policy PROJECT_ID filename.yml

[root@cloudserver]# gcloud projects add-iam-policy PROJECT_ID --member user:(user@email.com) --role roles/editor

[filename.yml]
bindings:
- members:
  - user:user@email.com
  role: roles/editor
etag: BwWBQh7328=
version: 1
```

### IAM Best practices
- Principle of least privilege
- predefined roles over primative roles
- grant roles at smallest scope necessary
  - compute instance admin vs compute admin
- editor role better than owner
- name service keys to reflect use and permissions
- Export audit logs to cloud storage
- use google groups
- more than 1 organization administrator
- separate production and development environments into separate projects

# Billing
### Billing + Cloud IAM
- Assigned mostly at organization level or within billing account
- Billing roles within Cloud IAM
  - Billing account creator - Org level only
  - Billing account administrator
  - Billing account user
  - Billing account Viewer
  - Project billing manager
- Export to Cloud storage and bigquery
- set budgets and alerts

# Global resources
### Sometimes Google Cloud services seem interchangeable
- BigTable or DataStore
- BigQuery or Cloud SQL
- App Engine or Container Engine

> Global resources are accessible by any resource in any zone within the same project. When you create a global resource, you do not need to provide a scope specification. 

### Global resources include 
  - Firewall rules
  - Snapshots
  - Networks
  - Routes
  - etc.

    ● Can only be used with managed instance groups 
    ● Can include some or all configuration of a regular instance 
    ● Instance and disk names will be generated from a configurable base name 
    ● Ephemeral external IP addresses will be automatically assigned 
    ● Single-instance properties limit creation to 1 instance/group Properties.disks[].source properties.disks[].initializeParams.diskName properties.networkInterfaces.accessConfigs.natIP

### Multi-regional resources
- Cloud storage
- Cloud Datastore
- Cloud BigQuery

# IaaS
### Google Cloud Compute
- Infrastructure as a Service (IaaS)
- On-demand pricing, Sustained use discount, Committed use discount, Preemptible VM
- Disks: Standard, SSD, Local SSD - Pay for allocation
- Snapshots: mv disks different zones and regions, resize.
- Custom applications
- Lift and Shift
- N1-standard systems have CPU # in the name with 4x# for RAM

# GCP Networking
## VPC networking
- You can create up to five networks per project
- Auto mode VPC networks have a single, automatically created subnet in each region of the VPC network
- For custom mode VPC networks, create a network, then create the subnets that you want within a region
- 7000 VMs per VPC network
- Subnets are bound to regions
- Each subnet defines a private IP address range
- Subnets are used for resource management
- Networks can span multiple regions around the world
- Shared VPC allows a network to be shared across multiple projects in a single organization
- VPN tunnel can be used to connect 2 separate organizations
- Machines in different networks can only communicate with public IPs
- Machines without a public IP can only be accessed within a network

## Firewalls
- Single firewall for entire VPC - implied deny all ingress, implied allow all egres
- To access load-balanced servers, a firewall rule must be created for the appropriate networks
- Best practice is to create firewall rules allowing traffic only from GCP Load Balancer networks

## Load Balancers
- Can be created for HTTP, TCP, or UDP
- Can span multiple regions or balance traffic to machines in multiple zones within a single region
- Can send traffic to multiple machines managed by one or more instance groups forming a back-end service
- Can also send requests to static content in Cloud Storage buckets
- Allows HTTP(S) traffic to Compute Engine instances
- Instances can be further secured by having no external IP addresses
- Load balancing uses internal IP addresses
- Leave a bastion host by which you can manage these instances

### Network load balancing distributes incoming traffic across multiple instances 
    ○ Supports non-HTTP(S) protocols (TCP/UDP) 
    ○ Can be used for HTTPS traffic when you want to terminate connection on your instances (not at HTTPS load balancer) 
    ● Supports autoscaling with managed instance groups

### Global Forwarding Rules 
    ● A global forwarding rule provides a single global IP address for an application 
    ● The rule routes traffic by IP address, port, and protocol to an HTTP or HTTPS target proxy 
    ● A global forwarding rule can only forward to a single port ○ Supporting HTTP and HTTPS requires two rules 
    ● Global forwarding rules can only be used by an HTTP(S) load balancer

### Cloud SSL proxy advantages (vs. network load balancing with termination of SSL connections on the instances) 
    ○ Intelligent routing 
    ○ Reduced CPI load on instances 
    ○ Certificate management 
    ○ Security patching

Target proxies are referenced by one or more global forwarding rules and route the incoming HTTP or HTTPS requests to a URL map. You can manage target proxies by using either the gcloud command-line tool or the REST API methods.

## Cloud DNS
- Allows you to manage your domains right from GCP
- Map domains and subdomains to load balancers, cloud storage buckets, or App Engine applications
- 100% SLA uptime

## Cloud CDN
- Configured when setting up an HTTP(S) load balancer
- CDN is cheaper for network egres traffic

## Cloud VPN
- Cloud VPN allows secure access to VPC networks 
- Can use a VPN to connect a GCP network to your local network using direct connect or interconnect see slide 54/55
- Can use a VPN to connect different GCP networks

# PaaS 
## Google Container Engine
- Used to deploy containerized applications
- Applications deployed into a cluster of Compute Engine virtual machines
- Application configuration is defined using Kubernetes open-source, orchestration framework
- Kubernetes supports other clouds and on-premises deployments
- GKE is more flexible than App Engine, but also less automated
- Especially useful for hybrid deployments

## Google App Engine Standard
- Each service is deployed independently
- Services scale independently
- Different languages can be used for different services
- Can deploy multiple version simultaneously
- Use traffic splitting to control which version(s) requests are sent to
- Uses Google-specific containers for deploying services
- Supports only Python, Java, PHP, and Go
- Scales very quickly; containers can start in a couple hundred milliseconds
- Will scale to zero instances when there is no traffic
- free tier

## Google App Engine Flex
- Uses Docker containers for deploying services
- Supports any language that will run in a Docker container
- Google provides preconfigured Docker containers for Python, Java, PHP, Go, Ruby, .NET, and NodeJS
- Docker containers run in a Compute Engine virtual machine
- Cannot scale to zero instances, so there is no free tier
- Autoscaling enabled
- App Engine Flexible supports custom images

## Google Cloud functions
- Serverless environments for running Node.js functions
- Just upload the code
- Microservice architecture where each function performs one job
- Can respond to Storage changes, Pub/Sub messages, or Web requests
- Incredibly fast and scalable
- Very inexpensive (first 2 million requests per month are free)

## Cloud Storage
- A scalable, fully-managed, highly reliable, and cost-efficient object / blob store.
- Images, pictures, and videos, Objects and blobs, Unstructured data
- Storing and streaming multimedia
- Storage for custom data analytics pipelines
- Archive, backup, and disaster recovery
### Nearline
- Cost per GB is less than regional, but there is a cost for accessing data and a 1-month minimum charge
- Use for data that is accessed infrequently (photos might be an example)

### Coldline
- Cost per GB is less than Nearline, but there is a higher cost for accessing data and a 3-month minimum charge
- Use for data archiving, backups, and disaster recovery

### Cloud Data Transfer replication 
- Transfer google cloud buckets
- Transfer amazon s3 buckets
- 1GB per stream

## Cloud SQL
- A fully-managed MySQL and PostgreSQL database service that is built on the strength and reliability of Google’s infrastructure.
- Cloud SQL delivers high performance and scalability with up to 10TB of storage capacity, 25,000 IOPS, and 208GB of RAM per instance.
- Second Generation instances support MySQL 5.6 or 5.7, and provide up to 208 GB of RAM and 10 TB data storage, with the option to automatically increase the storage size as needed.
- First Generation instances support MySQL 5.5 or 5.6, and provide up to 16 GB of RAM and 500 GB data storage.
- ACID
- No duplication
- Online transaction processing (OLTP)
- IOPS increases with increase disk space, does not require hardware modification

## Cloud Spanner
- Cloud Spanner is a fully managed, mission-critical, relational database service that offers transactional consistency at global scale, schemas, SQL (ANSI 2011 with extensions), and automatic, synchronous replication for high availability.
- Gain horizontal scaling without migration from relational to NoSQL databases

## Cloud Datastore
- A scalable, fully-managed NoSQL document database for your web and mobile applications.
- Semi-structured application data
- Hierarchical data
- Durable key-value data
- Product catalogs that provide real-time inventory and product details for a retailer.
- User profiles that deliver a customized experience based on the user’s past activities and preferences.
- Transactions based on ACID properties, for example, transferring funds from one bank account to another.
- 1GB per month free tier

## Cloud BigTable
- Cloud Bigtable is ideal for applications that need very high throughput and scalability for non-structured key/value data, where each value is typically no larger than 10 MB. Cloud Bigtable also excels as a storage engine for batch MapReduce operations, stream processing/analytics, and machine-learning applications.
- Cloud Bigtable is Google's NoSQL Big Data database service. It's the same database that powers many core Google services, including Search, Analytics, Maps, and Gmail. Bigtable is designed to handle massive workloads at consistent low latency and high throughput, so it\'s a great choice for both operational and analytical applications, including IoT, user analytics, and financial data analysis.
- Wide-column NoSQL datastore similar to Cassandra or HBase
- Requires a cluster of machines be created
- Each row in a BigTable table must have a unique key
- Go, Java
- Datasets starting at 1TB
- BigTable has full ETL capability. In this case also no need for autoscaling

## Cloud BigQuery
- A scalable, fully-managed Enterprise Data Warehouse (EDW) with SQL and fast response times.
- Interactive querying online analytical processing (OLAP) workloads up to petabyte-scale
- Data warehousing and data analysis service
- Extremely inexpensive storage
- 2 cents/GB/month for the first 3 months, then 1 cent/GB/month after
- Extremely fast: jobs split across many machines 
- Easy to use: no indexes required, simple schemas
- NoOps: no need to provision anything

## Stackdriver suite best practices
- Create a single project for stackdriver Monitoring
- Single pane of glass for all projects activities
- Determine monitoring needs in advance
- IAM controls are separate for stackdriver

## StackDriver Logging
- Stackdriver Logging is part of the Stackdriver suite of products in Google Cloud Platform (GCP). It includes storage for logs, a user interface called the Logs Viewer, and an API to manage logs programmatically. Stackdriver Logging lets you read and write log entries, search and filter your logs, export your logs, and create logs-based metrics.
- Real-time log management and analysis

## StackDriver Monitoring
- Stackdriver Monitoring collects metrics, events, and metadata from Google Cloud Platform, Amazon Web Services (AWS), hosted uptime probes, application instrumentation, and a variety of common application components including Cassandra, Nginx, Apache Web Server, Elasticsearch and many others. Stackdriver ingests that data and generates insights via dashboards, charts, and alerts.
- Stackdriver Monitoring is available in two service tiers, Basic and Premium
- Basic 7 days of logging 
- Premium 30 days of logging admin.
- 400 days of non-admin logs

## StackDriver Error reporting
- Stackdriver Error Reporting aggregates and displays errors produced in your running cloud services.
- Supported languages are Java, Python, JavaScript, Ruby, C#, PHP, and Go. To report errors from Android and iOS client applications, we recommend setting up Firebase Crash Reporting.
- Stackdriver Error Reporting is Generally Available for Google App Engine standard environment and is a Beta feature for Google App Engine flexible environment, Google Compute Engine, and AWS EC2.
- Reporting errors from your application can be achieved by logging application errors to Google Stackdriver Logging or by calling an API endpoint. The setup process depends on your platform; please refer to the setup guides.
- Information in Stackdriver Error Reporting is retained for 30 days.
- Error Reporting displays errors for the currently-selected Cloud Platform Console project. It does not support Stackdriver accounts.

## StackDriver Trace
- Stackdriver Trace is a distributed tracing system for Google Cloud Platform that collects latency data from Google App Engine, Google HTTP(S) load balancers, and applications instrumented with the Stackdriver Trace SDKs, and displays it in near real time in the Google Cloud Platform Console.
- Quickly view a snapshot of last-day latency data for your application in the trace overview.
- Generate custom analysis reports that show an overview of latency data for all or a subset or requests, and allow you to compare two different sets of latency data.
- Traces are stored for 30 days.

## GCP Dataflow
- Cloud Dataflow is a fully-managed service for transforming and enriching data in stream (real time) and batch (historical) modes with equal reliability and expressiveness -- no more complex workarounds or compromises needed. And with its serverless approach to resource provisioning and management, you have access to virtually limitless capacity to solve your biggest data processing challenges, while paying only for what you use.
- Data Flow Fully-managed data processing service, supporting both stream and batch execution of pipelines
Apache Beam
- ETL - The process of extracting data from source systems and bringing it into the data warehouse is commonly called ETL, which stands for extraction, transformation, and loading
- Dataflow is a unified programming model and a managed service for developing and executing a wide range of data processing patterns including ETL, batch computation, and continuous computation. Cloud Dataflow frees you from operational tasks like resource management and performance optimization.

## Cloud Pub/Sub
- Publish/Subscribe service
- Cloud Pub/Sub is a simple, reliable, scalable foundation for stream analytics and event-driven computing systems. As part of Google Cloud’s stream analytics solution, the service ingests event streams and delivers them to Cloud Dataflow for processing and BigQuery for fanalysis as a data warehousing solution.
- Can be used to queue up and store messages up to 7 days
- Balancing workloads in network clusters Implementing asynchronous workflows Distributing event notifications Refreshing distributed caches Logging to multiple systems Data streaming from various processes or devices Reliability improvement
- Does not work with synchronous workflows

## Cloud Dataproc
- Google Cloud Dataproc lets you provision Apache Hadoop clusters and connect to underlying analytic data stores.
- Cloud Dataproc is a managed framework that runs on the Google Cloud Platform and ties together several popular tools for processing data, including Apache Hadoop, Spark, Hive, and Pig. Cloud Dataproc has a set of control and integration mechanisms that coordinate the lifecycle, management, and coordination of clusters. Cloud Dataproc is integrated with the YARN application manager to make managing and using your clusters easier.
- It can setup HDFS on cloud storage buckets

## Deployment manager
- Declarative parallel processing schema files
- yaml Declarative language
- Reliable reproducable - deployment Manager
- Cloud Launcher

#### All operations problems are treated as software problems through
- Automate repetitive tasks
- Fix problems with code

## Blue/Green Deployment
- Allow new revisions to be deployed with less risk and no downtime
- There are two copies of the production environment
- Blue environment is taking requests
- Green environment is idle
#### When deploying a new version:
- Update new version to green environment leaving the blue environment in place
- Test the green environment
- When testing is complete, move the workload to the green environment
- Green is now blue; can turn the old environment off
- Blue-Green deployments also make rolling back to old versions easier
- App Engine completely automates blue-green deployments
- Multiple versions of App Engine apps can exist simultaneously
- Can easily request any version by including the version in the URL
- Testing can be done on new versions before migrating the load to them

## A/B Testing
- A/B testing allows multiple versions of a program to run at the same time
- Test the new version with a portion of the users 
- Compare usage and errors of the two versions
- Allows developers to get feedback and testing from real users prior to fully committing to a new version
- In App Engine, use traffic-splitting for A/B testing

## Canary Release
- A new version of a service is put into production alongside old versions
- A small subset of select traffic is routed to the canary release
- Canary releases help developers know how a new version will perform when put into production
- Canary releases are easy to pull back if they fail their testing
- Use a microservice architecture in App Engine
- Each small service can be deployed separately from the other services in the application

## Autoscaling
- Graceful exit on autoscaling shrink event by running shutdown script to redirect incoming traffic at load balancer, and flush cache prior to exit.
- The autoscaler will collect information based on the policy, compare it to your desired target utilization, and determine if it needs to perform scaling
- Autoscaler controls managed instance group

## Instance grouping
- Regional managed instance groups improve your application availability by spreading your instances across three zones. 
- Managed Instance group - instance group updater apply rolling updates - canary update rollback zero downtime
- All unmanaged groups are used for is load balancing across dissimilar instances

## Connection draining
You can enable connection draining on backend services to ensure minimal interruption to your users when an instance is removed from an instance group, either manually or by an autoscaler. To enable connection draining, you set a timeout duration during which the backend service preserves existing sessions being handled by endpoints on an instance that will be removed. The backend service preserves these sessions until the timeout duration has elapsed, allowing user sessions to gracefully terminate, but preventing new connections to the instance. After the timeout duration is reached, the instance is terminated and all remaining connections are forcibly closed. You can set a timeout duration between 1 to 3600 seconds.

- doug@roitraining.com
- wwww.roitraining.com
- linkedin:drehnstrom

| Service | Description |
| --- | --- |
| Cloud Dataflow | # Serverless stream and batch data processing service |
| BigQuery | # Serverless data warehousing services that help you to deploy advanced cloud data warehousing solutions for your enterprise |
| Cloud ML Engine | # Serverless machine learning services that automatically scales built on custom Google hardware (Tensor Processing Units) |
| Mobile apps | # Firebase |
| Web clients | # Firebase |
| Web backend | # App Engine -> Datastore |
| Microservices | # Cloud Functions -> Datastore |
| Bots | # Cloud Functions |
| IoT device messages | # Cloud Pub/Sub -> Dataflow |
| NoSQL database | # Cloud Datastore |
| ETL | # Cloud Dataflow -> BigQuery |
| Blob file storage | # Cloud Storage |
| Analytics warehouse (SQL) | # BigQuery |
| Personalization | # Cloud Machine Learning Engine |

## Tensorflow
TensorFlow™ is an open source software library for numerical computation using data flow graphs. Nodes in the graph represent mathematical operations, while the graph edges represent the multidimensional data arrays (tensors) communicated between them
