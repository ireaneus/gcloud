# Sample Case Study: Dress4win

>Dress4Win is a web-based company that helps their users organize and manage their personal wardrobe using a web app and mobile application. The company also cultivates an active social network that connects their users with designers and retailers. They monetize their services through advertising, ecommerce, referrals, and a freemium app model. The application has grown from a few servers in the founder’s garage to several hundred servers and appliances in a colocated data center. However, the capacity of their infrastructure is now insufficient for the application’s rapid growth. Because of this growth and the company’s desire to innovate faster, Dress4Win is committing to a full migration to a public cloud.

## Solution Concept
>For the first phase of their migration to the cloud, Dress4Win is moving their development and test environments. They are also building a disaster recovery site, because their current infrastructure is at a single location. They are not sure which components of their architecture they can migrate as is and which components they need to change before migrating them.

## Existing technical environment
*The Dress4win application is served out of a single datacenter location*

### Databases:
- MySQL - user data, inventory, static data
    - Redis - metadata, social graph, caching
    - MySQL 5.7
    - 8 core CPUs
    - 128 GB of RAM
    - 2x 5 TB HDD (RAID 1)

### Compute:
- 40 web application servers providing micro-services based APIs and static content
    - Tomcat - Java
    - Nginx
    - Four core CPUs
    - 32 GB of RAM
- 20 Apache Hadoop/Spark servers:
    - Data analysis
    - Real-time trending calculations
    - Eight core CPUs
    - 128 GB of RAM
    - 4x 5 TB HDD (RAID 1)
- Three RabbitMQ servers for messaging, social notifications, and events:
    - Eight core CPUs
    - 32GB of RAM
- Miscellaneous servers:
    - Jenkins, monitoring, bastion hosts, security scanners
    - Eight core CPUs
    - 32GB of RAM

### Storage appliances:

- iSCSI for VM hosts
- Fibre channel SAN - MySQL databases
    - 1 PB total storage; 400 TB available
- NAS - image storage, logs, backups
    - 100 TB total storage; 35 TB available


## Business Requirements
- Build a reliable and reproducible environment with scaled parity of production.
  - *Deployment manager*

- Improve security by defining and adhering to a set of security and Identity and Access Management (IAM) best practices for cloud.

- Improve business agility and speed of innovation through rapid provisioning of new resources.
  - *Managed instance groups, Autoscaling through load balancers*

- Analyze and optimize architecture for performance in the cloud.
  - *app-engine - kubernetes*


## Technical Requirements
- Easily create non-production environments in the cloud
- Implement an automation framework for provisioning resources in cloud
- Implement a continuous deployment process for deploying applications to the on-premises data center or cloud
- Support failover of the production environment to cloud during an emergency
- Encrypt data on the wire and at rest
- Support multiple private connections between the production data center and cloud environment.

## Executive statement
>Our investors are concerned about our ability to scale and contain costs with our current infrastructure. They are also concerned that a competitor could use a public cloud platform to offset their up-front investment and free them to focus on developing better features. Our traffic patterns are highest in the mornings and weekend evenings; during other times, 80% of our capacity is sitting idle.

>Our capital expenditure is now exceeding our quarterly projections. Migrating to the cloud will likely cause an initial increase in spending, but we expect to fully transition before our next hardware refresh cycle. Our total cost of ownership (TCO) analysis over the next five years for a public cloud strategy achieves a cost reduction between 30% and 50% over our current model.

#### Case Study review  #####

### Application servers:
- Tomcat - Java micro-services
  - ### App Engine
- Nginx - static content
  - ### Storage
- Apache Beam - Batch processing
  - ### Data Flow

### Storage appliances:
- iSCSI for VM hosts
- Fiber channel SAN - MySQL databases
- NAS - image storage, logs, backups

### Apache Hadoop/Spark servers:
- Data analysis
- Real-time trending calculations
### MQ servers:
- Messaging
- Social notifications
- Events

### Miscellaneous servers:
- Deployment manager Jenkins, Stackdriver monitoring, Compute engine and private network hosts bastion hosts, Cloud Security scanners - App engine scanners. security scanners

## What are their goals (Solution Concept)
- Migrate dev/test environments
- Build DR - Hybrid networks (GCP connect to on-premises)
- prefer managed services

## What do they care about
- Manage cost - scaling down capacity during off-peak times
- Business agility - resource provisioning
- Secure environment - IAM Customer supplied encryption firewall rules