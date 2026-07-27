Auto Scaling Groups
====================

By using this ASG service we can launch the new servers when traffic is increase to our application and we can terminate the instance automatically by using ASG.

Before creating ASG:
--------------------

1. We have to create one Golden AMI(Gold fish) with our own settings and tools, dependencies  we required for our application.

2. Create one empty Load balancer with empty target group.

3. create Launch Template. by crating this we can give:
  - instance-type, 
  - instance SG, 
  - AMI Created in step1, 
  - Key-pair, 
  - don't select subnets hear if you select hear all the instances will only create in that subnet.
  - no need to create volume we will get volumes from AMI because at the time of creating AMI's from the instance
    we get those volumes in a snapshot format those snap shot volumes liked with this AMI.

Now Create ASG:
----------------
1. select Launch template.
2. network settings- hear you can select VPC and subnets. advantage of selecting subnet hear is you can select more then one subnet.
3. instance type requirement: we can override instance type which we mentioned in Launch Template.
4. Load balancer -> select excising load balancer--> select target group
5. give desired count, min count, max count.
6. enable  SNS topic for notification purpose.
   
> we can edit desired, max, min count when ever you need.

>*** By using automatic scaling option we can schedule a crone job and
     we can control the desired count of the instances by using this we can reduce cost for our organization.

> In automatic scaling we have two options one is:
 ---------------------
1. Dynamic
   ---------
1.1 Target tracing scaling 
1.2. step scaling 


1.1. dynamic in dynamic this will work on Target tracking scaling policy this will work based on the  metrics we set in the configuration if you set average CPU utilization 50 % if any case the load crossed the limit then it will automatically scale up and down our infra based on the values mentioned in the max and min count.

1.2. in dynamic we have another policy called step scaling policy this will work like if you have 40% of CPU utilization it will work on 2 ec2 if the CPU utilization was increased to 60 %  then it will launch another EC2 and it will work on 3 EC2 like step by step based on the load it will launch the instanced


2. Predictable
----------------
predictable this predictable will work on our previous work load metrics means it will check last 1 or 2 weeks of data and i will scale up the infra and scale down the infra.










