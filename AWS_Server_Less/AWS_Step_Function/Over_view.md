AWS Step Functions:
==================

<img width="995" height="598" alt="image" src="https://github.com/user-attachments/assets/b2ea45f3-cadc-4dc2-b2c4-59819b01f372" />

What is AWS Step Functions?

* AWS Step Functions is a serverless workflow orchestration service.

* In simple words:

Imagine you have an application deployment process with multiple steps:

    1. Download application code.
    2. Run automated tests.
    3. Build a Docker image.
    4. Push the image to ECR.
    5. Deploy the application to ECS.
    6. Verify application health.
    7. Send deployment notification.

How do you coordinate all these steps?

* You can use AWS Step Functions to control the complete workflow.

It decides:

    - Which task should execute first.
    - Which task should execute next.
    - What happens if a task fails.
    - Whether a task should be retried.
    - Whether a task should execute conditionally.
    - Whether multiple tasks can run in parallel.
Think of Step Functions as a manager coordinating multiple AWS services.

Real-world architecture:
-----------------------

<img width="1277" height="743" alt="image" src="https://github.com/user-attachments/assets/8d36e72a-bd9b-4312-a875-6f841458d5b8" />

<img width="965" height="462" alt="image" src="https://github.com/user-attachments/assets/bee452fc-ae12-4415-a86c-b5cc4cafbbfd" />

Important Step Functions terminology
------------------------------------

Let's understand the main building blocks.

<img width="847" height="870" alt="image" src="https://github.com/user-attachments/assets/51cbef92-7677-465e-a9d5-f1727eb8fdf5" />

<img width="982" height="602" alt="image" src="https://github.com/user-attachments/assets/d7d5d79b-4d33-4000-b6b2-9f18b34330a3" />

How a workflow executes
-------------------------
Let's consider an application deployment.

<img width="982" height="687" alt="image" src="https://github.com/user-attachments/assets/2165b408-980c-41ec-9ec9-ba16bffd1493" />

<img width="975" height="527" alt="image" src="https://github.com/user-attachments/assets/56f2dac6-1d2d-45b3-b559-c349edcb9ac5" />

<img width="997" height="651" alt="image" src="https://github.com/user-attachments/assets/f1ac4fcb-b7ea-4d3a-8bee-12b4376caa3e" />

<img width="733" height="337" alt="image" src="https://github.com/user-attachments/assets/fe53dbd9-1c32-4b38-a551-10ae153140d6" />


