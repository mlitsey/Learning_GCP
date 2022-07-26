# GCP Data Engineer Course

## What is Google Cloud Platform?
1. [Google Cloud learning path](https://learn.acloud.guru/learning-path/gcp-architecture)
2. [Introduction to Google Cloud course](https://acloud.guru/overview/gcp-101)
3. [Google Certified Associate Cloud Engineer course](https://acloud.guru/overview/gcp-certified-associate-cloud-engineer)
4. [Google Certified Professional Cloud Architect course](https://acloud.guru/overview/gcp-certified-professional-cloud-architect)
5. [Stay up to date on GCP news with GCP This Month](https://acloud.guru/series/gcp-this-month)
6. [Get GCP service reviews with GCP Service Spotlight](https://acloud.guru/series/gcp-service-spotlight)
7. [Google Cloud locations](https://cloud.google.com/about/locations)

## VPC (virtual private cloud)
- [ ] Create the VPC
1. From Google Cloud console's main navigation, choose VPC Network.
2. Click Create VPC Network.
3. Give your VPC a name.
4. On Subnets, click Custom. Give a unique name to your subnet.
5. Set Region to US-Central-1.
6. Set you IP Address range to 10.0.1.0/24.
7. Click Done.
8. Click Create.
- [ ] Verify the VPC Has Been Created
1. Click VPC Network.
2. You should see the VPC name that you have created.

## Create an Instance
- [ ] Create the General Purpose Instance
In the lab, the correct project is already selected. In the real world, if you have multiple projects, you would want to choose the appropriate project before starting.
1. From Google Cloud console's main navigation, choose Compute Engine.
2. Click Create.
3. Give your instance a name.
4. Make sure your instance is in the US-Central-1 region; the zone does not matter.
5. For Machine Family, click General-purpose and choose e2-medium.
6. Under Firewall, we are going to choose HTTP and HTTPS.
7. Click Create.
- [ ] Verify the Instance Was Created
From the Instance dashboard, verify the instance is up and running.

## Cloud Storage
- Bucket Names must be globally unique

### Storage Classes
1. Standard
	- best for data that is frequently accessed and stored for only brief periods of time
2. Nearline
	- low cost highly durable storage service for storing infrequently accessed data
3. Coldline
	- very low cost, highly durable storage similar to near line, access approximately once a quarter
4. Archive
	- lowest cost highly durable storage for archiving, backup, and disaster recovery. Access approximately once a year

### Create a Bucket
In the lab, the correct project is already selected; but in the real world, if you have multiple projects, you would want to choose the appropriate project before starting.
1. From Google Cloud console's main navigation, choose Storage.
2. Click Create a Bucket.
3. Give your bucket a name—it has to be globally unique—and click Continue.
4. Select Region as the location type and US-Central-1 as the region, then click Continue.
5. Select Standard as the storage class, then click Continue.
6. Under Access Control, select fine-grained, then click Continue.
7. Under Protection tools, leave the option None selected.
8. Expand DATA ENCRYPTION; we will leave the Use a customer-managed encryption key (CMEK) box unchecked.
9. Click Create.

Create a Folder, Upload an Object, and Make the Bucket Public
1. Once inside the bucket, click CREATE FOLDER.
2. Give your folder a name, then click Create.
3. Click on your folder name hyperlink to access the folder.
4. Select UPLOAD FILES and upload a sample file into your folder.
5. Click on Bucket details to move back into the bucket level and out of your folder.
6. Highlight your bucket and click on the three dots on the right of your bucket, then click Edit access.
7. Select ADD PRINCIPAL.
8. Under New principals, type in allUsers.
9. Select the Cloud Storage role, and then select Storage Object Viewer.
10. Click Save, then click Allow Public Access.

## Database Services

### Relational Databases
1. Cloud SQL
	- MySQL, Postgres, SQL server engines
2. Cloud Spanner
	- globally distributed database service with unlimited scaling
### Non-Relational Databases
1. BigTable
    - large analytical and operation workloads
2. Firestore
    - fully managed document database for developing applications
3. Memory Store
    - highly available in-memory service for Redis and Memcached, 

### Create a Cloud SQL Instance
1. From Google Cloud console's main navigation, under Storage, choose SQL.
2. Click Create Instance, and then click Choose MySQL.
3. Give your instance a name such as test-instance, and set the root password to acloudguru.
4. For Zone, choose us-central1-b.
5. Under Database version, click the MySQL 5.7 engine for the instance.
6. Expand the Show configuration options menu.
7. For Machine type and storage:
    * Click Change.
    * Select the shared core machine type
    * Zonal availability: Single Zone
8. Under Storage type, Storage capacity, and Encryption, leave the settings as they are as SSD, 10GB, and Google-managed key respectively (as you do not need much space for this lab).
9. Scroll down, and leave all other settings as their defaults.
10. Under Labels, set Key to name and Value to Test.
11. Click Create.
Note: It may take up to 5 minutes for the instance to be created.

## Big Query
`
SELECT
  name, gender,
  SUM(number) AS total
FROM
  `bigquery-public-data.usa_names.usa_1910_2013`
GROUP BY
  name, gender
ORDER BY
  total DESC
LIMIT
  10
`

## Google Dataflow
Google Dataflow is a fully managed servicefor executing Apache Beam pipelines within the Google Cloud Platform ecosystem.
Apache Beam is an open-source unified programming model to define and execute data processing pipelines. 
If you look here at the diagram, you'll see a pipeline, you'll see a data source, PTransform, PCollection, and output. 
The data source can be, let's say, a database like Cloud SQL, and the PTransform, it represents a data processing operation or a step in your pipeline. 
PCollection represents distributed dataset that your Beam pipeline operates on.
The dataset can be bounded, meaning if it comes from a fixed source like a file, or unbounded, meaning it comes from a continuously updating source via subscription, like Pub/Sub or another mechanism.
Dataflow automatically partitions your data and distributes your worker code to Compute Engine instances for parallel processing.
![GCP Dataflow](/Users/mlitsey/GCP_Data_Engineer/GCP_Dataflow.jpg)

## Pub/Sub
Pub/Sub is a fully-managed real-time messaging service that allows you to send and receive messages between independent applications. 
You can use Pub/Sub as messaging-oriented middleware or event ingestion and delivery for streaming analytics pipelines. 

> Your team wants a setup in their environment where they would like to receive notifications on events and even possibly create some automation to respond to those event notifications. You've heard a lot about Pub/Sub, so you want to try to set up and test a topic and subscription to see how it works.

### Creating a Topic and Subscription with Pub/Sub in Google Cloud
- [ ] Creating a Topic and Subscription
1. From Google Cloud console's main navigation, choose **Pub/Sub**.
2. Click **Create Topic**.
3. Give your topic a name, click **Create Topic**.
4. From the Topics dashboard, head over to the Pub/Sub menu and select **Subscriptions**.
5. Select **Create a Subscription**.
6. Name your subscription and then select your topic under **Select a Cloud Pub/Sub Topic**.
7. For delivery type, you will leave this as Pull.
8. Click **Create**.
- [ ] Publishing a Message
1. From the dashboard, head over to the Pub/Sub menu and select **Topics**.
2. Once inside the Topics dashboard, click on the topic you have created.
3. Click **Publish a Message** then **Publish a single message**.
4. For publish type, leave it as **Single**.
5. In the message body, you will type in the message you will like to push.
6. Click **Publish**.
- [ ] Pulling the Message
1. From the dashboard, head over to the Pub/Sub menu and select **Subscriptions**.
2. Click on your subscription.
3. Select **View Messages**.
4. Under Messages, select **Pull**.

## Docker
`docker ps` - command to see docker containers running

`docker run <container-name>` - command to run a specific docker container

`docer run -it ubuntu bash` - runs an interactive ubuntu container with bash prompt

## Vagrant
`vagrant init centos/7` - command to initialize a centos7 instance

`vagrant up` - starts virtual machine

`vagrant ssh` - connect to virtual machine

`vagrant halt` - shutdown 

`vagrant destroy` - removes virtual machine

# Cost Control on GCP

- can export to BigQuery.
- setup billing alerts
- right size VM's, automatic but have to restart VM
- sustained use discounts, automatic 25% of the month
- commited use discount, 1-3 year commitment
- preemptible VMs, can be shutdown by GCP and only 24 hours at most

