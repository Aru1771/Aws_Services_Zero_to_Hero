What is AWS Cloud watch ?
-----------------------------
* it will watch services in the aws. like if you uploading any file to S3 if you integrate Cloud watch to S3 then it will watch S3 activities.
* it is acts like a Gatekeeper to aws. which was helping us in monitoring, alerting, reporting, logging.
* it will play key role in infrastructure monitoring.
* Bu using Cloud watch we can track metrics, monitoring logs and set alarms.
* By setting alarms we can take action before it causes the issues.
* we will key eye on resource utilizations and if required we can reduce the resource limits to do cost optimization.
* we can use logs to troubleshoot and resolve the issues quickly.
* Could watch is much expensive so we long integrate this could watch to prod related resources.


Create a Log group in cloud watch:
-----------------------------------

* in log group we can store any type of logs like "VPC FLOW_LOGS", Application logs, infra_logs, cloud_trail_logs.

* To create a log groups we have to Go to CloudWatch service.
* in left side we have *Logs* --> *Log_Groups*--> click on create a *log group*
* In log Groups we have two types:
  1. standerd
  2. infrequent
 
* Standerd: give all futures like live trail, metrics extension, alarams, data protection, log patterns to provide real time visability into application health.
* infrequent: it will give less futures only select for lower env not recomended fro higher env.
  
Log group creation:
-------------------

Name: log group name
Retention settings: hear we can select after how many days the logs will be delete: eg: 12 months --> means 12 months logs only available.
                    if you change the retention period time i will reflect after 73 hours.

Log class: standerd

KMS-keys: Optinal

Tags: 

* Click on create a log group

* once we created the log group we can see details like Log class, metrics filtes, retention, storage bytes.

* we can also export the logs to s3 bucket. 

*Now we can see How to send VPC flow logs to log group:*
------------------------------------------------------

* First we have to go to VPC --> Flow_Logs--> click on create flow logs

  Name: name of the flow logs
  Filter: accept/reject/ all --> recomended is all
  Maximum aggregation intervel: 10 min
  Destination: send to cloudwatch
  Destination log groups: select the log group which we have created above.
  IAM role: Select the below created iam role

           to know about permissions--> click in info -- > VPC Flow Logs -- > Publish Flow logs to CW --> IAM role for Publish Flow logs to CW

           we have to create a role with the policied available in the above page.

           first create a policy then create a role with that policy

           at the time creating the role.

           Trusted entry--> aws services ---> use case--> EC2 ---> select the above permission policy and we have to attch the trush policy which is
            mentioned in the same policy tab for this iam role.

Note:
  
         At the time of creating the role trust policy will pointing Ec2 only after creating the role we will edit and replace the service as per the docs.


Log record formate: we can select default or customised



Note: 


      we cant' modify the flow_logs once it was created


When the flow logs will capture the logs.
-----------------------------------------

when ever we create any resouce in the vpc it will generate the logs.

For eg: if i created ec2 instace automatically one network interface was created and automatically in log group we can see the log steam was created there we can
        find ourlogs 



           
*Now we can see How to setup alarms to EC2 instances:*
---------------------------------------------------------

Eg: you can take a perticular instance is reached to high CPU or Memory utilization.


* we can set the alaram in two way.
  1. from cloud watch alarms
  2. from ec2 instance actions--> monitoring and troubleshoot --> manage cloud watch alarms

Alarm setup for Ec2:
--------------------

* click on create alarm
* Alaram notification: for this we have to use SNS service--> select the below created topic.

         for SNS topic creation we have to go to SNS service ---> select topic --> create topic

          Type:  standerd
          Name: name of the topic
          Dispaly name: Email notification

          Subscribe Topic:

           Chose: lambda, email, sms ----> select email---> office email

           we will recieave an email we have to confirm the subscription.

* Alaem Action: Recover, restart, terminate, stop--> based on requirment choose one

* Alarm thershold: we have specify the metrics
     
  
  
Note:
------
cloud watch will not check the memory metrics for that we have create custom metrics.

    1. Monitoring is the first thing and advantage in aws Cloud watch for infra monitoring.
    
    2. Real life metrics it is noting we can say like how much cup was utilized last 30 min. this will helps to understand the situation.
    
    3. Alarms: It will send notification based on the metrics then we will easy take actions before making the issue.
    
    4. log insights: with the help of this log insights we can know which service is accessing the other service in AWS that records means logs will record in Logs.
    
    5. Custom metrics: if any matrices we need to send the cloud watch then we will use custom metrics like CPU memory. by using this custom metrics we can monitor our application health as well.
    
    6. Cost optimization: we will discus in future.


Now we can see the CloudWatch.
============================== 

first we will see log groups :
------------------------------

log groups means cloud watch automatically create a group for our logs.
eg: if I create any project in code build it will create one log group in log groups in cloud watch.


if you create another project in code build it will create another log group.

what we can see in the log group ?

we can see last build logs for last 1 hour, it will tell docker file stages logs. it will tell errors.

second log insights:
--------------------

by giving simple quarry we get log groups data.


Third Metrix:
---------------

by using Metrix cloud watch automatically information of aws services.

default metrics: 1036 it will collect all the matrix in our account from all the services.












