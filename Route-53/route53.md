Route 53:
=========

what is Route 53 ?

* Route53 provides DNS service(Domaine Name System) provided by AWS.
* 53 is the port of route53.
* Route means when we try to access you_tube from our browser it will route to different routers and hubs and reach out to  you_tube server.

* In my case if i serch a website  myapp.com ---> this request will go to DNS service like --> Route53-->
* --> once request reached the route53 it will resolve the prticular request based on "**Name servers**" settings--->
* --> once it resolved the request will forworded to server--> this request forwording will happen with the "**A records or C-Name**"
* Based on this A or C-name records our request will served to backword resources.

* Name servers records: these records are responsible to resolve the domine name.
* A rocord: is used to server the request to backend services.


Setting of Route53:
-------------------
In this case i have a domine name with text.xyz for that domine i want to create a HostedZone

step:1  go to console and serch for route53 and click on Hostedzone.

step:2  we get form we have to fill that:

        1. Domine_Name: text.xyz
        2. description: 
        3. type: Public or Private --> select Public
        4. click on create HostedZone.

Note: Before creating a Hosted Zone we have to purchase a domine from Dome providers like godady, rout54, Google Domine.

* when i purchase a domine in Google domines at that time i will get default Name servers.
* But we don't use these default Name Servers.
* we have to use custome name servers fro our domine.
* like in step 2 i have created a hosted zone for my domine at that time i will get New "Name servers" records in aws.
* we have to copy those "Name-servers" from the Hosted Zone Provider like AWS and copy them into Domine registry website like google domine and past that into --> DNS --> UNDER custome name servers.
* Then our domine use these custome NS. so the request will forworded to HostedZone in aws. 

Now set up a "**A Records**":
-----------------------------

in this case i am using EC2 instance

* First i will create one ec2 instance.
* second we will crete a A records in Hosted zone for our ec2.
* in A records we have lot of policies
  1.simple route
  2.weighted
  3.latency
  4. failover
  and some more...
* we are using *simple_routeing* hear
* then click on "Define simple route"
* Record_Name: leave it blank it is for subdomines we are doing hear for main domine
* Recoed_type: *A record traffic to an IPV4 address and some AWS resource*
* Value: pass the EC2 public IP. and click on *define simple record*
* select the record and click in create record.

Now we can see How to setup A Load balancer as a A-record simple Routing:
--------------------------------------------------------------------------

* For this we have to create VPC-1, Subnets 1 public and 1 Private, Igw, route tables- one private route table and one public route table.
* we have to attach publich subnet to Public route table use subnet association and we have to add IGW into the Route.
* Then we have to launch the EC2 instance in the Public Subnet.
* we have to create a traget group with that Ec2 instance.
* Then we have to launch the LB and we have to attch the target group with listner With HTTP and Port 80.

Now the Route53 configration will start:
* we have click on create records
* there we can select "simple_routeing" and click on Next.
* Record_Name: leave it blank it is for subdomines we are doing hear for main domine
* Recoed_type: *A record traffic to an IPV4 address and some AWS resource*
* Value: select type: Alisa to App and classic loadbalancer --> Region --> Load balancer.
* click on *define simple record*
* select the record and click in create record.
 


  
Now we can see How to setup A two Load balancer in route 53 and how we can do weighted routing:
-------------------------------------------------------------------------------------------------


* For this we have to create VPC-2, Subnets 1 public and 1 Private, Igw, route tables- one private route table and one public route table.
* we have to attach publich subnet to Public route table use subnet association and we have to add IGW into the Route.
* Then we have to launch the EC2 instance in the Public Subnet.
* we have to create a traget group with that Ec2 instance.
* Then we have to launch the LB and we have to attch the target group with listner With HTTP and Port 80.


Now the Route53 configration will start:

* we have click on create records
* there we can select "Weighted" and click on Next
* Record_Name: leave it blank it is for subdomines we are doing hear for main domine
* Recoed_type: *A record traffic to an IPV4 address and some AWS resource*
* Weighted records to add to <domine name> --> hear we have to click on *difine Weighted routing*
* in the *difine Weighted routing* we have to choose the lb type as Alisa to App and classic loadbalancer --> Region--> Loadbalancer 
* Hear we have to give weight like 128 is 50% in 256. Hear 256 is the 100% we have to divide the 256.
* RecordID: A record for Lb1 or LB2.
* Like above we have to add another *difine Weighted routing* for another Lb with weight 128% so traffic can be distribute equally to
  Both the Load balancers.
* once both the Weighted routing's were created we have to select the both and click on create records



Now we can see How to setup A two Load balancer in route 53 and how we can do geolocation routing:
---------------------------------------------------------------------------------------------------
* By using this we can send the particular location request to specific loadbalancer.
* we can use the same two LB  setup for this geolocation routing. simply delete the Weighted records and click on create records.
* Select geolocation routing.
* Record_Name: leave it blank it is for subdomines we are doing hear for main domine.
* Recoed_type: *A record traffic to an IPV4 address and some AWS resource*
* in the field geolocation records to add to <domine name>--> click on *define geolocation record*
* in the *difine geolocation record* we have to choose the lb type as Alisa to App and classic loadbalancer --> Region--> Loadbalancer.
* Location: any of the required location from where we need to make the request
* RecordID: request from the region.
* like above we have to add another *define geolocation record* in there we can selection another region.
* once both the geolocation record's  were created we have to select the both and click on create records.


Now we can see How to setup A two Load balancer in route 53 and how we can do geolocation routing:
--------------------------------------------------------------------------------------------------

* By using this we will route the traffic to secondrey load balncer when primary load balncer is fail to serves the request.
* we can use the same two LB  setup for this Filover record. simply delete the geolocation records and click on create records.
* Select on Failover
* Record_Name: leave it blank it is for subdomines we are doing hear for main domine.
* Recoed_type: *A record traffic to an IPV4 address and some AWS resource*
* in the field Failover records to add to <domine name>--> click on *define Failover record*
* in the *difine Failover record* we have to choose the lb type as Alisa to App and classic loadbalancer --> Region--> Loadbalancer.
* Filover type: Primary for LB-1
* like above we have to add another *define Failover record* in there we can selection another LB-2 and give failover type is secondary.
* once both the Failover record's  were created we have to select the both and click on create records.

  


