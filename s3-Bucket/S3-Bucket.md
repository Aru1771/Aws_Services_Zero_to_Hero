Aws -S3- SSS(simple storage service)
====================================

* In s3 we can store any amount of data (scalability) (charged by the data you have uploaded)

* There is no data lose hear. AWS will maintain 3 copies of our data. (most availability)

* we can access data from any where in the internet.

* All the data will store in SSD disks.

* AWS s3 is global service no need to selecta region

Day:1
-----

Topics:
-------
1. how to crate a S3 bucket.
2. how to upload a files to S3.
3. how to download a files from s3.

Day:2
======

Topics:
-------

1. how to make an object public.
2. how to configure s3 bucket as static website.
3. static website demo.


How to make an object public.
--------------------------------

* if you upload any object to s3 it will store in private. to access the object publicly we have to make that object Public.

      Go to object -- >object actions ---> Make public

* how to make bucket publicly access ?

       Go to Permisson block -- > edit block public access--- > uncheck and save 
  

* why we are maintain our bucker name unique ?

A) After creating our bucket the aws will create a domine with the unique bucket name.

* that domine URL will create like:

* https://[bucket_name].s3.[bucket_region].amazon.com/[object]

* object URL we will get with https.

how to configure s3 bucket as static website.
-------------------------------------------------


1. if we have any front end code we have to upload the files and folders to s3 bucket first.
2. once upload was completed make all the object related to code make them public.
3. we can access the app by using index.html default URL. if you click the object you can see it.
4. but I want to access application with separate static URL which we get for our bucket.
For bucket Static URL:

        Go to properties-- > static website hosting [Edit]  --> Enable -->Host a static website--> index document [index.html] file name --> save 
5. we get the URL in the same properties tab under static website hostname.

        http://[bucket_name].s3-website.[bucket_region].amazonaws.com
6. static website URL we will get with http

Note:  
    
      along with that always check the bucket have public access or not in permission tab.


Day:3
=====

AWS Global regions.

AWS divide the regions based on out continents.

    1. North America
    2. South America
    3. Asian Pacific
    4. middle east


* so as of now we have 25 regions globally.

* total we have 80 available zone globally.

* in every region we have >= 3 availability zones.

* in a region for every availability zone will maintain min 100km distance.

* Now we can see if we upload a file or object in S3 bucket. how the file will store in the region and how many copies it will store ?
A) i created one s3-bucket my-bucket in Mumbai region. so i have uploaded one file in the bucket. so bucket data will store in all the available zones in the region.

* if i make a download request for file from s3 bucket. the request will go to top level domine resolver and see file address spaces and then the request will go to that AZ.
  if any of the AZ will down the request will go to AZ-2.

* How is telling AWS S3 to maintain 3 copies for our file ?

A) it will work based on the "storage class"

Totally we have 7 storage classes:
------------------------------------
these 7 will categorized into 4 types:

    1.frequent
    2.infrequent
    3.archived
    4.intelegence


Day-4
======


Storage Classes:
----------------

frequent
-----------
under frequent access we have:
1. standard: data will store >=3 AZ, pay as you go, no min charges
2. Reduces redundant storage (RRS) --> not recommended to use by AWS.

infrequent access
----------------------
under infrequent access we have:

1. Standard infrequently access:
   data will store >=3 AZ, if you want to download any of the object from this class we have to pay retrieval pay to AWS. it will charge for 28 days. min charges

2. one zone infrequently access:
   data will store only 1 AZ-- not recommended,if you want to download any of the object from this class we have to pay retrieval pay to AWS. it will charge for 28 days.min charges
   
Archival
-------------

under Archival storage we have:

1. Glacier:
   data will store >=3 AZ, file will store in zip format.
   if you want to download any of the object from this class we have to pay more retrieval pay to AWS.
   this retrieval pay will be based on the time of the download min 5min to 12hr of time to download.
   it will charge for 90 days.min charges




3. Glacier Deep archival:
   data will store >=3 AZ, file will store in zip format but more compressed,
   if you want to download any of the object from this class we have to pay more retrieval pay to AWS.
   hear we have to wait 12 hours to download a file. it will charge for 180 days.min charges


intelligent:
-----------------
   data will store >=3 AZ, pay as you go. No min charges


* in the storage classes main diff is data storing format, cost, Retravel fee, min charges, AZ,  for the above store classes please refer s3 Storage classes PDF.




