Object Storage

- before you can use a storage, you will need to create a bucket.
- search for storage on the search bar.
- bucket name should be unique globally.
- choose location to store your bucket. they come in regional, dual-regional and multi-regional.
- choose default storage class which can be standard, nearline, coldline, archive.
- you can drag and drop your data from your pc to your bucket.
- files are objects and folders are just folders.

Cloud Storage

- it is popular, flexible and inexpensive storage service.
- it is multipurpose.
- it is serverless, auto scaling and infinite.
- you can store large object using a key value pair
- treats an entire object as a unit. partial updates are not allowed.
- you can have access control at object level.
- provides REST API to access and modify the object.
- also provide a cli which is gsutil & client libraries like java, python, nodejs
- used to store all different file types eg media files, archives, packages and logs, db backups, storage devices, staging data during on-premise to cloud database migration.

Cloud Storage - Objects & Buckets Structure.

- first create a bucket then store objects in the form of key and value pairs.
- objects are stored in buckets with bucket names globally unique.
- bucket names are used as part of the object URLs and can only contain lower case letters, numbers, hyphens, underscore, and periods.
- you can have 3 to 63 characters and should not start with 'goog' or contain the word 'google'
- you can have unlimited objects in a bucket.
- each object is identified by unique key
- maximum object size is 5TB but you can store unlimited number of such objects.

Cloud Storage - Storage Classes
Introduction;

- different kinds of data can be stored in Cloud Storage eg media files, archives, packages, logs, backups and long term archives.
- dues to this, there is huge variations in access patterns.
- storage classes helps you to optimize your costs needs based on your access needs.
- it is designed for durability of 99.999%.

Types of Storage Classes

1. Standard
   Name: Standard
   Min Storage Duration: None
   Monthly Availability: 99.999% in multi region and dual region, and regions.
   Use Case: Data that is frequently used data for short period of time.

2. Nearline Storage
   Name: Nearline.
   Min Storage Duration: 30 days.
   Monthly Availability: 99.95% in multi region & dual region, 99.9% in regions.
   Use Case: Data that is read or modify once a month on average.

3. Coldline Storage
   Name: Coldline
   Min Storage Duration: 90 days.
   Monthly Availability: 99.95% in multi and dual region, 99.9% in regions
   Use Case: data is read or modified once a quarter.

4. Archive Storage
   Name: Archive
   Min Storage Duration: 365 days
   Montly Availability: 99.95% in multi region and dual region, 99.9% in regions.
   Use Case: data that is used less than once a year.

Storage Class Features

- high durability (99.99999% annual durability)
- low latency (first byte typically in tens of milliseconds)
- unlimited storage, auto scaling(no configurations needed), no minimum object size
- uses same API across all storage classes.
- committed SLA is 99.95% for multi region and 99.9% for single region for Standard, Nearline, and Coldline storage classes.
- there is no commited sla for archive storage.

Cloud Storage - Upload & Download Objects

1. Simple Upload
   Scenario: Small files that can be re uploaded in case of failures and NO object metadata.
2. Multipart Upload
   Scenario: Small files that can be re uploaded in case of failures + object metadata
3. Resumable Upload
   Scenario: Large files, RECOMMENDED for most use cases (even for small files - costs on addititonal HTTP request)
4. Streaming Transfers
   Scenario: Upload an object of unknown size
5. Parallel Compsite Uploads
   Scenario: File is divided up to 32 chunks and uploaded in parallel. Significantly faster if network and disk speed are not limiting factor.

6. Simple Download
   Scenario: Downloading objects to destination
7. Streaming Download
   Scenario: Downloading data to a process
8. Sliced Object Download
   Scenario: Slice the objects into smaller parts and download them.
