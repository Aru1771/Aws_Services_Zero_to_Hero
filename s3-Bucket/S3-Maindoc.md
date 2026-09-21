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

Use case in my project: 

     we store our vpc flow logs, waf logs, cloud trail logs,  ..ect in Standerd -IA class.
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

Properties Tab: 
  
               bucket region, Bucket arn, created date and Bucket version with edit options, Tags with edit option, defult encryption with edit option.
                *server access logs* -- if you enble this we can capture S3 bucket logs like uploading, deleting. it's chargable
                *AWS Cloud trail logs* 
                *Event notification*: we can send notifications based up on the events. we can apply this events to specific path(called prefix)
                                      we have destination option like lambda, SNS and SQS.

Permissions Tab: public access with edit option, bucket policy with edit option.

Bucket Metric: bucket size, no of objects

Managment: life cycle rules.



Use case in my project: 

                       From AWS console are we able to download the folder like file ? 
                        No we can't download the folder from the aws console like file.
                        and we can't download the munltile objects as well from the console a time like selecting few files

                        If you want to download folders or multile objects we can do it with the help of AWS CLI.



Versioing :
  
       By enabling this bucket versioning to s3 if we are uploading the same file agaian and again with 
        the modified data the old file will be saved under versioning means we see latest file in objects tab. 
        if you want to see old file we can see underversioning.

Use case in my project:

      for terrafrom state file.
            


✅ Bucket Policy in Amazon S3
-------------------------------

* if you want to restrict the access you can enable the policy hear. even if you have admin access but the bucket policy is denied we can't access the bucket.

* so if user have access to put the objects to s3 at account level but at resource level policy if we are restricting it the user will not upload any files.

* in user level policy user don't have access to upload the files to s3 bucket but at bucket level we provided the access to that user then he will able to
  to upload the files to s3.

* Final Point: Resource level policy have high Priority.


Use case in my project: 


       By using this we can only provide access to specific user in the orginizaation to access and perform the actions on that bucket.
       Rest all users even the Admin users also can't access the bucket.


✅ S3 Bucket Lifecycle
-------------------------

* By using this life cycle rule's we can move our bucket objects from one class to another class.

* For that we have to create life cycle rule in bucket manegment tab.

Go to --> Management Tab --> click on create rule:

               Life cycle rule_name:
               Chosee rule scope: always choose a folder in the bucket to apply this rule.
                                  path like folder1/aws_logs/region/* ---> after a path we have to specify the region.
                                  after region if any file or folder was created those will move to another class after 30 days.
                                  /region will come for only vpc flow logs.
              Object size: if you want we can specify

              * Life cycle rule actions: select--> move current version of objects b/w storage class.

              Transfer: chosse storage class and mentione the time frame
              
              Life cycle rule actions: if you want old versions of the file's --> select --> move nonconcurrent option for this option we have create another
                                       Transfer like below.

              Transfer: chosse storage class and mentione the time frame.
              
              * Life cycle rule actions: if you select --> expire current version of object.
                                                         we have to specify the days then after that specied days the current file will be deleted.
               Life cycle rule actions: if you select -- > Perminently delete non concurrnent the old versions will delete after the mentioned time frame

                Life cycle rule actions: if you select --> delete incomple uploads --> we can delete those files after the mentioned time.

* we mainly use this for vpc flow logs, app load balancer logs, aws waf logs, cloud trail logs. to reduce the cost.

* This life cycle rules will apply every day 12:00 AM UTC time.



✅ How to capture and upload the ALB logs to s3 bucket:
-------------------------------------------------------

Why we need this ALB logs ?

* if an application is throwing an error meesage like 500 error and 404 errors at that time we have to check the ALB logs.
* 500 error means health check.
* at that point we have to check the application load balancer logs.


Note: 
   
      1. we upload the ALB logs to S3 Bukcet we have to create a bucket at the same region.
      2. we can able to captute logs for only ALB and Classic Load balancer because they are supporting HTTP and HTTPS.
      3. We can't capture the logs for network load balancer because it will work on tcp Protocol. if you want the NLB logs we have to chck the vpc logs.


* To setup a logs for ALM

      Click on LB --> Actions --> Edit load balancer attributes --> under monitoring--> Access logs: Enble --> give s3 bucket ARN--> save and change it.

* But before to do this we have update a polict at the bucket level to allow the alb to store his logs.

       For that: Go to google and search for How to upload ALB logs to S3.

       find the aws page and copy the policy and update it at bucket level

        in polict edit the elb account id by refering the same docs.

        after configure --in s3 --awslogs--account_num--testfile.


Use case in my project: 

       we always enable the ALB logs and store in s3.

       How we can troble shoot who is storing in s3 ?

       By using athena aws service.

       we will store multiple lb logs in same s3.


✅ Presigned URLs in AWS S3
----------------------------

* By useing this we can provide access to specify object in the bucket for the specified time to user.


✅ Server-side encryption for Amazon S3 buckets
-----------------------------------------------

* Real time we wll use SSE only defalt s3 key.


✅ AWS cli CMD:
-----------------

* We can copy the objects to bucket by using cp
* if you want to upload a folder to s3 we have to use --recursive in the end.
* after deleting the bucket we can create the bucket with deleted bucket name imedeatly in the same region.
* But if you want to use that deleted bucket name in another region we have to wait one hour.
  
                                  
       


  





