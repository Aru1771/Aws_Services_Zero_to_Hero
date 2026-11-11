Sticky Sessions in AWS ALB:
=============================

* Sticky Sessions (Session Affinity) ensure that requests from the same client are consistently forwarded to the same backend target for a specified duration.

* The main purpose is to maintain session continuity between a client and an application server.

* Understand with a real-world example

Suppose you have an application running on three EC2 instances behind an ALB.

<img width="768" height="631" alt="image" src="https://github.com/user-attachments/assets/71bf03db-6e01-4c69-8e68-f0941192e626" />

<img width="937" height="752" alt="image" src="https://github.com/user-attachments/assets/66b10baf-56b4-4ef9-a8ad-2df85dac7a51" />

How does ALB identify the same client?
---------------------------------------

ALB uses cookies to maintain stickiness.

Example:

    1. Client sends the first request.
    2. ALB selects EC2-1.
    3. ALB sends a cookie to the client's browser.
    4. Browser stores the cookie.
    5. On the next request, browser sends that cookie back.
    6. ALB reads the cookie and forwards the request to EC2-1.

Example response:

    HTTP/1.1 200 OK
    Set-Cookie: AWSALB=encrypted-value


The browser sends it in subsequent requests:

    Cookie: AWSALB=encrypted-value


Types of Sticky Sessions in ALB:
---------------------------------

AWS ALB supports two types.

This is how ALB remembers the selected target.

<img width="955" height="472" alt="image" src="https://github.com/user-attachments/assets/a4b9d41c-2e74-4cd7-86a2-631a6779c27c" />

<img width="952" height="500" alt="image" src="https://github.com/user-attachments/assets/bf4b5bb3-d2d2-4787-aeaf-ae52cf09a9af" />


How to enable Sticky Sessions:
------------------------------

* You enable it at the Target Group level, not directly as a general ALB-wide setting.

Steps:

      1. Open AWS Console.
      2. Go to EC2.
      3. Select Target Groups.
      4. Select your target group.
      5. Open Attributes.
      6. Click Edit.
      7. Enable Stickiness.
      8. Select the stickiness type.
      9. Configure the duration.
      10. Save changes.

What happens if EC2-1 becomes unhealthy?
----------------------------------------

Suppose:

    - User is connected to EC2-1.
    - EC2-1 becomes unhealthy.
    - The user's cookie still points to EC2-1.
Will ALB continue sending traffic to EC2-1?

No. ALB selects another healthy target, such as EC2-2. The session becomes associated with the new target.

Disadvantages of Sticky Sessions

Sticky sessions are useful, but they introduce some production problems.

Problem	Explanation

    Uneven traffic -->	One EC2 instance may receive more requests
    Scaling	--> Newly added instances might receive less traffic from existing sticky clients
    Instance failure --> 	Client gets moved to another healthy instance
    Session dependency -->	Application may depend on local server memory
This is important for production troubleshooting.

My DevOps recommendation:

For production applications, prefer storing session information in centralized storage such as:

    - Redis / Amazon ElastiCache
    - Amazon DynamoDB
    - A shared database
This allows any healthy application instance to retrieve the user's session.

Important EKS interview scenario:
----------------------------------

Suppose you deploy an application in EKS:

    - 3 Kubernetes pods
    - ALB Ingress Controller (AWS Load Balancer Controller)
    - ALB with sticky sessions enabled
A user initially reaches Pod-1.

Subsequent requests continue reaching Pod-1 while stickiness is valid.

If Pod-1 becomes unhealthy, traffic can move to another healthy pod.

For EKS, remember that ALB stickiness is configured through target group attributes. With AWS Load Balancer Controller, you can configure it using Ingress annotations.
