What is Amazon EC2?
--------------------
* Amazon EC2 (Elastic Compute Cloud) is a web service that provides resizable compute capacity in the cloud.
* It allows you to obtain and configure virtual servers (known as instances) to run your applications, scalable depending on your needs.

      Scalability: Easily scale compute resources up or down as needed.
      Variety of Instances: Choose from various instance types optimized for different use cases (e.g., compute-optimized, memory-optimized).
      Security: Secure instances with built-in security groups and key pairs.
      Flexible Pricing: Pay only for the compute capacity you actually use.
      Integration: Easily integrate with other AWS services and third-party tools.

Key Points:

            Instance Sizes: Each instance type is available in different sizes, offering varying amounts of CPU, memory, storage, and networking capacity.
            Customizable: Users can select the instance type and size that best suits their specific workload requirements.


1. general purpose --> instances provides a balanced mix of compute, memory, and network resources. they are ideal for diverse workloads, like web services, code repositories, and when workload performance is uncertain.
  
2. compute optimized --> instances are ideal for compute-intensive tasks, such as gaming servers, high performance computing(hpc), machine learning, and scientific modeling.

3. memory optimized --> instances are used for memory-intensive tasks like processing large datasets, data analytics and databases. they provides fast performance for memory-heavy work loads.

4. accelerated computing --> instances use hardware accelerator, like graphics processing units(gpu's), to efficiently handle tasks, such as floating- calculations, graphics processing, and machine learning.
 
5. storage optimized --> instances are designed for workloads that requires high performance for local storage, such as databases, data warehousing, and I/O-intensive applications.

cost optimization on EC2:
=========================

Now we can see and understand the diff ec2 pricing options, so we can take decisions and optimized your cost based on your specific user needs.

1. on-demand instances --> pay only for the compute capacity you consume with no upfront payments or long-term commitments required.
You pay only for what you use (per second or per hour).

No advance payment.

No long-term contract.

Instances run continuously until you stop them.

No risk of interruption by AWS.




2. reserved instances --> get a saving of up to 75% by committing to a 1-year or 3-year term for predictable workloads using specific instances families and aws region.

You reserve EC2 capacity for 1 year or 3 years.

You choose:

Instance type (family, size)

Region

OS and tenancy

In return, AWS gives up to 75% discount compared to On-Demand.

Best for steady, predictable workloads.





3. spot instances --> bid on spare compute capacity at up to 90% off the on-demand price, with the flexibility to be interrupted when AWS reclaiming the instance.

AWS has many servers. Some are not always in use.

They sell this extra capacity very cheaply (up to 90% cheaper than On-Demand).

You can bid/use this capacity at a low price.

But AWS can take it back anytime (with ~2 minutes notice) if they need it.

So your instance can be interrupted/stopped/terminated suddenly.





*4. savings plans --> save up to 72% across a variety of instances and services by committing to a consistent use age level for 1 to 3 years.

You commit to a fixed spend per hour (for example: $10/hour) for 1 or 3 years.

AWS gives up to 72% discount.

The discount automatically applies to:

EC2

Fargate

Lambda

Across multiple instance families and regions (depending on plan type).






5. dedicated hosts --> reserve an entire physical server for you exclusive use. this option offers full control and is ideal for workloads with strict security or licensing needs.

You are not sharing the hardware with any other AWS customer.

You get full control over the physical host.

You can place your own EC2 instances on that host.

Useful when you have:

Strict security/compliance

License-bound software (like Oracle, Windows Server with BYOL, etc.)






6. dedicated instances --> pay for instance running in hardware dedicated solely to your account. this option provides isolation from other AWS customers.

Your EC2 instances run on dedicated physical servers.

No other AWS customer’s instances will be on the same hardware.

AWS manages the hardware, you only manage the instances.

Provides strong isolation for security and compliance.




Use SSH to connect to your Linux server:
---------------------------------------

1. Open Terminal (Linux/Mac) or PuTTY (Windows): Launch your SSH client.
2. SSH Command: Use the SSH command with the instance's public IP address or DNS name and your private key file:

        ssh -i /path/to/private-key.pem ec2-user@public-ip-address
* Replace /path/to/private-key.pem with the path to your PEM file, ec2-user with your instance's username (may vary by AMI), and public-ip-address with your instance's public IP address or DNS.

Access Windows Instance:
--------------------------

* Once the instance is launched, note the public IP address or DNS name of your instance from the EC2 dashboard.

Connect to Windows Instance:

    Use Remote Desktop Protocol (RDP) to connect to your Windows instance. On your local machine:
    Search for "Remote Desktop Connection" in the Start menu (Windows).
    Enter the public IP address or DNS name of your instance. Click "Connect."
    Enter the credentials (username and password) specified during instance launch.

Start Using Windows Instance:

    You are now connected to your Windows EC2 instance. Start configuring Windows settings, installing applications, and exploring the capabilities of Windows on AWS.

