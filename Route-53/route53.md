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

step:2  we get form we have to fill that
        1. Domine_Name: text.xyz
        2. description: 
        3. type: Public or Private --> select Public
        4. click on create HostedZone.

Note: Before creating a Hosted Zone we have to purchase a domine from Dome providers like godady, rout54, Google Domine.

* when i purchase a domine in Google domines at that time i will get default Name servers.
* But we don't use these default Name Servers.
* we have to use custome name servers fro our domine.
* like in step 2 i have created a hosted zone for my domine at that time i will get New Name servers records in aws.
* we have to copy those Name-servers from the Hosted Zone Provider like AWS and copy them into Domine registry website like google domine and past that into --> DNS --> UNDER custome name servers.
* Then our domine use these custome NS. so the request will forworded to HostedZone in aws. 

Now set up a "**A Records**":
-----------------------------

in this case i am using EC2 instance

* First i will create one ec2 instance.
* second we will crete a A records in Hosted zone for our ec2.
* in A records we have lot of policies
  1.simple route
  2.weighted
  3. latency
  and some more...
* we are using simple route hear
* Third click on "Define simple route"
* subdomine: leave it empty because we are pointing to our main domine.
* record type: route traffic to an IPV4 Address and some aws resources
* Value: pass the EC2 public IP.

Now we can see How to setup A Load balancer as a A-record Weighted Routing:
--------------------------------------------------------------------------




