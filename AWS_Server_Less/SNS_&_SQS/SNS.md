AWS SNS (Simple Notification Service):
=======================================

* SNS = Simple Notification Service.

* AWS SNS is a messaging service used to send notifications or messages from one application or AWS service to multiple subscribers.

* SNS sends one message to multiple subscribers.

<img width="532" height="286" alt="image" src="https://github.com/user-attachments/assets/8d755169-0b28-4c39-90bf-b20447fd2f0f" />

<img width="1446" height="857" alt="image" src="https://github.com/user-attachments/assets/8b365de9-420f-4ddf-9d19-aa476d64233c" />

* SNS types and important concepts

<img width="527" height="370" alt="image" src="https://github.com/user-attachments/assets/fb203f83-b662-4355-bff1-7124e65bdca7" />

<img width="855" height="515" alt="image" src="https://github.com/user-attachments/assets/969c0bff-8f57-4c01-a710-921299d99d6b" />

<img width="942" height="231" alt="image" src="https://github.com/user-attachments/assets/c008317c-0c5a-4fe7-9035-a2122cf73fb0" />

Create SNS topic:

<img width="1537" height="847" alt="image" src="https://github.com/user-attachments/assets/4a560ba1-b8ed-4460-99dc-160d42759ea1" />

Follow these steps:

    1. Open AWS Console.
    2. Search for SNS.
    3. Select Topics.
    4. Click Create topic.
    5. Select Standard.
    6. Give the topic name: devops-alerts.
    7. Click Create topic.
    8. Open the topic and select Create subscription.
    9. Select Protocol: Email.
    10. Enter your email address.
    11. Create subscription.
    12. Open your email and confirm the subscription.
    13. Go back to SNS and select Publish message.
    14. Enter:
        ALERT: Production server CPU utilization is above 90%.
        Please investigate immediately.

Remember this architecture:

<img width="957" height="512" alt="image" src="https://github.com/user-attachments/assets/ebbc8f61-18b7-4c39-9f50-93e71dd75795" />

A producer can publish a message to SNS in three common ways:

        1. AWS service publishes a message.
        2. Application publishes a message using AWS SDK/API.
        3. Developer manually publishes a message through AWS Console or AWS CLI.

AWS service → SNS
-------------------
Example: CloudWatch Alarm detects high CPU utilization on an EC2 instance.

<img width="988" height="670" alt="image" src="https://github.com/user-attachments/assets/6795c195-369f-4a0c-ba3a-bc66db34abc6" />

Application → SNS using AWS SDK:
--------------------------------

Imagine you have a Python application running on EC2.

Whenever your application detects a critical error, you want to send a notification to SNS.

Example Python code:


        import boto3
        
        # Connect to AWS SNS
        sns = boto3.client("sns", region_name="ap-south-1")
        
        # Publish message
        response = sns.publish(
            TopicArn="arn:aws:sns:ap-south-1:123456789012:devops-alerts",
            Subject="Application Error",
            Message="Production application is down!"
        )
        
        print("Message published successfully")
        print(response["MessageId"])

<img width="985" height="315" alt="image" src="https://github.com/user-attachments/assets/2f816a70-09f7-4136-ac95-5b6658076b18" />

Developer → SNS using AWS CLI:
------------------------------

You can also publish a message manually from your terminal.

    aws sns publish \
      --topic-arn "arn:aws:sns:ap-south-1:123456789012:devops-alerts" \
      --subject "Production Alert" \
      --message "Production server CPU utilization is above 90%"

This is useful for testing SNS notifications.

<img width="648" height="320" alt="image" src="https://github.com/user-attachments/assets/e4c5f21a-e273-4045-998b-25f94f7aa6c0" />

Important AWS DevOps interview concept:
----------------------------------------

Question: How does an application publish a message to SNS?

Answer:

An application uses the AWS SDK or SNS API to call the Publish operation. It provides the SNS Topic ARN and message content. SNS receives the message and distributes it to the topic's subscribed endpoints.
