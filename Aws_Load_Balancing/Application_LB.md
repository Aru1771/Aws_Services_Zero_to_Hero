Application Load Balancer:
--------------------------

* AWS Application Load Balancer (ALB) is a Layer 7 load balancer that distributes incoming HTTP and HTTPS traffic across multiple targets,
   such as EC2 instances, containers, and IP addresses, based on application-level information like URL paths, hostnames, and HTTP headers.

<img width="800" height="542" alt="image" src="https://github.com/user-attachments/assets/253d98e5-2197-445f-aa71-9300b21efb0b" />

<img width="577" height="720" alt="image" src="https://github.com/user-attachments/assets/421c17ca-3306-4be5-b8ea-7953c4594f75" />

* First we will see the simple flow:

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/13d4c22e-066f-4121-be94-b4427a66195c" />

Now:

    1. Users send HTTP/HTTPS requests to ALB.
    2. ALB receives the requests.
    3. ALB evaluates its listener rules.
    4. ALB forwards requests to the appropriate target group.
    5. A healthy backend instance processes the request.
    6. The response returns to the user through ALB.

Important: ALB does not normally distribute requests using a simple round-robin algorithm. It uses its configured routing rules and target-selection behavior.

<img width="727" height="542" alt="image" src="https://github.com/user-attachments/assets/12fdf02f-0841-4147-90a8-14a32a18ea86" />

<img width="976" height="615" alt="image" src="https://github.com/user-attachments/assets/a9d4824d-2a2d-4595-9eaa-e7c8967f4104" />


Module 3: ALB architecture and components
------------------------------------------

* This is the most important module. You must understand these four components:

<img width="2134" height="1162" alt="image" src="https://github.com/user-attachments/assets/5a3d2146-c57e-44bc-b236-1e2c07156ba1" />

<img width="726" height="282" alt="image" src="https://github.com/user-attachments/assets/2f3f85c1-d3e6-41eb-9c42-98c6eeb42fcc" />

# 3.1 Load Balancer

The load balancer is the entry point for your application.

Example:

      DNS: app.example.com
              |
              v
      Application Load Balancer
              |
              v
      Backend applications
      

ALB supports two main schemes:

1. Internet-facing ALB

        - Accepts traffic through publicly reachable load balancer addresses.
        - Usually deployed in public subnets.
        - Used for applications accessed over the internet.
   
2. Internal ALB

          - Has private IP addresses.
          - Used for internal applications.
          - Accessible from connected VPC networks or other permitted private networks.

Production example:

    - Public ALB → Customer-facing website.
    - Internal ALB → Internal employee application.

An ALB should normally be deployed across at least two Availability Zones for high availability.

# 3.2 Listener

A listener checks incoming connection requests using a configured protocol and port.

Example:

    ALB
     |
     |-- Listener 80 (HTTP)
     |
     |-- Listener 443 (HTTPS)


Common configurations:
Listener	Purpose

    HTTP : 80	Accept HTTP traffic
    HTTPS : 443	Accept encrypted HTTPS traffic

A listener needs a default action. You can also configure additional rules.

Real-world example:

A user accesses:

    https://app.company.com


The HTTPS listener on port 443 receives the request and evaluates its rules.


# 3.3 Listener Rules

This is where ALB becomes powerful.

Listener rules decide where to send incoming requests based on conditions.

Common conditions:

    - Host header
    - Path pattern
    - HTTP header
    - HTTP request method
    - Query string
    - Source IP address
Let's understand with examples.

<img width="878" height="410" alt="image" src="https://github.com/user-attachments/assets/85b81fdc-aa21-42b1-afd2-eb27294e15a4" />

Module 5: Security Groups – Production knowledge
-------------------------------------------------

<img width="806" height="727" alt="image" src="https://github.com/user-attachments/assets/9e94ab58-ae4a-4df6-bab9-37079999087a" />

<img width="907" height="262" alt="image" src="https://github.com/user-attachments/assets/0c7e5f0e-396c-499d-84a6-30a06e9aa700" />

<img width="461" height="451" alt="image" src="https://github.com/user-attachments/assets/2a1907eb-6956-4ee1-b3cd-9746e74da218" />

This provides an additional security layer.

Interview question: Why do we reference the ALB security group in the EC2 security group?

Answer: To allow backend traffic only from the load balancer, preventing direct access to the application port from unauthorized sources.

Remember that the ALB security group must allow outbound traffic to the targets on both application and health-check ports.


Module 6: SSL/TLS Termination
-----------------------------

Suppose your users access:

    https://company.com


How does ALB handle HTTPS?

You configure:

    - ACM certificate.
    - HTTPS listener on port 443.
    - SSL/TLS security policy.

<img width="4024" height="2154" alt="image" src="https://github.com/user-attachments/assets/de0d40f7-98a9-4da1-9658-e7075f035606" />

Traffic flow:

    Client
      |
      | HTTPS : 443
      |
      v
    ALB
      |
      | TLS termination
      |
      | HTTP : 8080
      |
      v
    EC2 Application

What is SSL termination?

    SSL/TLS termination means the ALB handles the incoming HTTPS encryption and decryption.

* The backend application can receive normal HTTP traffic.

* Alternatively, you can configure HTTPS between ALB and the backend as well.

Production recommendation:

Use HTTPS between the client and ALB. Where required by your security policy, also encrypt traffic between ALB and backend targets.

What is ACM?
    
    AWS Certificate Manager manages SSL/TLS certificates.

Typical workflow:

    1. Request or import a certificate in ACM.
    2. Validate domain ownership.
    3. Attach the certificate to the ALB HTTPS listener.
    4. Configure an HTTP listener to redirect port 80 traffic to HTTPS 443.

Example:

    http://company.com
           |
           v
    ALB : 80
           |
           v
    Redirect to HTTPS
           |
           v
    https://company.com


ALB supports multiple certificates on HTTPS listeners using Server Name Indication (SNI). 


Module 7: Advanced ALB routing
-------------------------------

# 7.1 Weighted routing – Canary deployment

Suppose you have two application versions:

    - Version V1: Stable application.
    - Version V2: New application.
You want to test V2 with only 10% of traffic.

<img width="1999" height="833" alt="image" src="https://github.com/user-attachments/assets/ba0b745d-7f41-424f-870c-72fd512b27ae" />

<img width="1000" height="473" alt="image" src="https://github.com/user-attachments/assets/222c8e21-1ef1-4db4-b277-b4e9a9151760" />

Now all traffic goes to V2.

Important: Weighted routing does not automatically detect whether V2 is performing better and adjust the weights. Your deployment process or monitoring decisions must manage that.

# 7.2 Redirect

You can redirect:

    HTTP → HTTPS


You can also redirect one URL to another.

Example:

    /company-old
         |
         v
    /company-new

# 7.3 Fixed response
Suppose your application is under maintenance.

You can configure a listener rule to return:

    HTTP 503
    Service temporarily unavailable


The ALB can return this response without forwarding the request to a backend.


Module 8: ALB with Auto Scaling Group
---------------------------------------
* Imagine your application experiences increasing traffic.

* Auto Scaling Group (ASG) can increase the number of EC2 instances.

      Example:
      Initial:
      EC2-1
      EC2-2
      
      High traffic:
      EC2-1
      EC2-2
      EC2-3
      EC2-4


How does ALB know about new EC2 instances?

    1. You associate the target group with the Auto Scaling Group.
    2. ASG launches a new EC2 instance.
    3. ASG automatically registers the instance with the target group.
    4. ALB performs health checks.
    5. After the initial health check succeeds, ALB starts routing traffic to the instance.
When an instance is removed, it is deregistered from the target group.

What is deregistration delay?

It gives existing in-flight requests time to complete before the target is fully removed from service. This helps avoid interrupting active requests.
This is a common production architecture.


ALB with EKS:
--------------

Now connect this to your Kubernetes knowledge.

In EKS, you can use AWS Load Balancer Controller to provision and manage ALBs from Kubernetes Ingress resources.

Traffic flow:

      Internet
         |
         v
      ALB
         |
         v
      Target Group
         |
         v
      Kubernetes Pods


<img width="867" height="347" alt="image" src="https://github.com/user-attachments/assets/02e3de0a-112e-46c8-87dc-d94dc27ade37" />

<img width="986" height="553" alt="image" src="https://github.com/user-attachments/assets/1b4739dc-49aa-4f22-b1df-26ef0e873b16" />

Example Ingress:

    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: application-ingress
      namespace: production
      annotations:
        alb.ingress.kubernetes.io/scheme: internet-facing
        alb.ingress.kubernetes.io/target-type: ip
    spec:
      ingressClassName: alb
      rules:
        - host: app.example.com
          http:
            paths:
              - path: /api
                pathType: Prefix
                backend:
                  service:
                    name: api-service
                    port:
                      number: 80

The AWS Load Balancer Controller watches this Ingress and reconciles the corresponding AWS resources.

Important: Kubernetes Ingress is not itself an ALB. It is a Kubernetes API resource that the controller translates into AWS load-balancer configuration.

<img width="800" height="580" alt="image" src="https://github.com/user-attachments/assets/0237b9c4-cab8-46ef-9110-31f04b80bda2" />


<img width="776" height="482" alt="image" src="https://github.com/user-attachments/assets/defb168d-678d-48af-8f3c-2de1f0141751" />

Production troubleshooting scenarios

Scenario 1: ALB returns 502 Bad Gateway

Possible causes:
- Backend closes the connection unexpectedly.
- Target sends an invalid response.
- Backend application crashes.
- Target protocol or connection configuration is incorrect.
Check:
1. Target group health.
2. Application logs.
3. Backend port.
4. ALB access logs.




Scenario 2: ALB returns 503 Service Unavailable

Possible causes:
- Target group has no registered targets.
- No healthy targets are available.
- Target registration is incorrect.
Check:
1. Target group registered targets.
2. Target health status.
3. Listener rules.
4. Auto Scaling registration.




Scenario 3: ALB returns 504 Gateway Timeout

Possible causes:
- Backend takes too long to respond.
- Application is stuck.
- Backend network connectivity problem.
- Backend response exceeds the ALB idle timeout.
Check:
1. TargetResponseTime.
2. Application performance.
3. Database response time.
4. Backend connectivity.
5. Timeout configurations.
