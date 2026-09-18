What is AWS S3:
----------------

      Definition: Amazon S3 is a simple storage service offering durable, highly available, and scalable data storage infrastructure at low costs.
      Object Storage: It is a key-based object store.
      File Upload: Allows uploading files ranging from 0 bytes to 5TB each.
      Scalability: Offers unlimited storage capacity.
      Bucket Storage: Files are stored in containers called buckets.
      Data Accessibility: Enables storing and retrieving any amount of data from anywhere on the internet.
      Global Unique Names: Operates with a universal namespace; bucket names must be globally unique.
      DNS Name: Each bucket has a DNS name for access (e.g., https://s3-eu-east-2.amazonaws.com/bucket_name).

Real World use cases:
-----------------------

* In my project we have mainly used s3 bucket to store the logs like:
  1. vpc flow logs
  2. waf logs
  3. cloud trail logs
  4. Application logs
  5. Application access logs
  6. Application load balancer logs

* in s3 we have two things
  1. Bucket
  2. Object


Use case in my project: once used was generated the report's for the sample set those reports will be stored in s3 bucket when even they requesred these
                        reports comes from s3 bucket.

                        How it will work ?
                        app team they will use AWS SDK for s3 to store the report in s3 bucket



Bucket:
--------
      Object Container: Buckets are containers for storing objects.
      Folder Simulation: Users can create folders within buckets using object key names.
      URL Access: Bucket names are part of the URL for accessing stored objects.
      Region Specific: Each S3 bucket is specific to a region.
      Global Name Uniqueness: Names must be globally unique due to S3's universal namespace.
      Logging: Supports enabling logging to track access and usage.

Objects in Bucket:
------------------
    Definition: Objects are individual items stored in an S3 bucket.
    Characteristics: Each object has a unique key and can store data up to 5TB, along with optional metadata.
    Access: Accessed via URLs incorporating bucket names and object keys (e.g., https://s3.amazonaws.com/bucket_name/object_key).
    Operations: Common operations include upload, download, copy, delete, and list, managed via AWS SDKs, CLI, or AWS Management Console.


Key point:

         we create bucket at region level but bucket name is global level.
         we only use general purpose buckets only.

Types of Storage Classes in AWS S3
---------------------------------

       Standard: Provides high durability, availability, and performance for frequently accessed data.
        Intelligent-Tiering: Automatically moves objects between two access tiers based on changing access patterns.
        Standard-IA (Infrequent Access): For data that is accessed less frequently but requires rapid access when needed.
        One Zone-IA: Lower-cost option for infrequently accessed data that doesn't require multiple Availability Zone resilience.
        Glacier and Glacier Deep Archive: For archival data with retrieval times ranging from minutes to hours.

Use case in my project: we store our vpc flow logs, waf logs, cloud trail logs,  ..ect in Standerd -IA class.
                        Application logs, Application access logs and we have to check with dev team how frequently app access these logs based up on that we can set class.
                        
                         after 30 days vpc flow logs, waf logs, cloud trail logs,  we move from Standerd-ai to galcier.
                         because vpc flow logs, waf logs, cloud trail logs we have to maintain atleast 18 months.
                         after 10 months old logs were deleted.

Now we can see the bucket creation:
------------------------------------

Go to -- > s3 --> click on create bucket.

Region: we have to select.

Bucket Type: General/Directry -- > select General

Bucket Name: unique bucket name with small letters

Copy settings from the exisiting buket: if you have any existing buket we can select hear those bucket setting will apply to this bucket.

Object Ownership: ACL disable / ACL enable --> always select ACL disable

                  ACL: access control list
                  if you want to provide access to other AWS accounts we have to enable this acl.
                  if you don't want we can disable it.

Block public access settings for this bucket: always disable it

                  if you enable it every one who have our bucket URL will download the objects.

Bucket Versioning: always enable

Default encryption: SSE-S3 , SSE-AWS kms, Dual SSE + AWS KMS ---> max we use SSE-S3


Bucket Key: if you're selecting AWS-KMS in above hear we will anable it.

object Locks: disable


Click on create Bucket

* After creating the bucket we see few things:

Objects Tab: hear we will see uploaded objects to that s3 bucket.

Properties Tab: bucket region, Bucket arn, created date and Bucket version with edit options, Tags with edit option, defult encryption with edit option.
                *server access logs* -- if you enble this we can capture S3 bucket logs like uploading, deleting. it's chargable
                *AWS Cloud trail logs* 
                *Event notification*: we can send notifications based up on the events. we can apply this events to specific path(called prefix)
                                      we have destination option like lambda, SNS and SQS.

Permissions Tab: public access with edit option, bucket policy with edit option.

Bucket Metric: bucket size, no of objects

Managment: life cycle rules.



Use case in my project: From AWS console are we able to download the folder like file ? 
                        No we can't download the folder from the aws console like file.
                        and we can't download the munltile objects as well from the console a time like selecting few files

                        If you want to download folders or multile objects we can do it with the help of AWS CLI.



Versioing : By enabling this bucket versioning to s3 if we are uploading the same file agaian and again with the modified data the old file will be saved under versioning 
            means we see latest file in objects tab. if you want to see old file we can see under versioning.

Use case in my project: for terrafrom state file.
            
                         







