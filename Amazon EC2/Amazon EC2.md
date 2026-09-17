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

