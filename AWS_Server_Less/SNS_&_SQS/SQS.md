What is AWS SQS?
=================

* Amazon SQS is a fully managed message queuing service.

* In simple words, SQS acts like a temporary storage area between two applications.

* Imagine an e-commerce application:

      1. Customer places an order.
      2. Order application sends the order message to SQS.
      3. SQS stores the message.
      4. Payment application receives the message and processes the payment.
      5. After successful processing, the application deletes the message from SQS.
* The important point: the order application and payment application don't need to communicate directly at the same time.

Understand SQS architecture
---------------------------

<img width="1276" height="693" alt="image" src="https://github.com/user-attachments/assets/118ec683-a03a-4e2f-bca3-c2d4acfcb50e" />

There are three important components:

<img width="676" height="353" alt="image" src="https://github.com/user-attachments/assets/1350a6ef-7d5e-460f-af99-9192ac8a78ac" />

Why do we need SQS?
-------------------

<img width="647" height="398" alt="image" src="https://github.com/user-attachments/assets/0d0ced61-a435-4697-8075-7374fb06f342" />

Types of SQS queues:
---------------------

<img width="820" height="722" alt="image" src="https://github.com/user-attachments/assets/b4b6b040-0a5e-412a-9e07-63ddce8d9afa" />

Important SQS concepts:
------------------------

<img width="802" height="698" alt="image" src="https://github.com/user-attachments/assets/6cd84361-9e1b-4975-9af0-b67785cc8554" />

<img width="975" height="185" alt="image" src="https://github.com/user-attachments/assets/0acdbafb-9721-42e3-b307-3115f552ff9b" />

<img width="1636" height="862" alt="image" src="https://github.com/user-attachments/assets/9c62761c-2d85-45ba-b9e1-1e8cb750d36e" />

<img width="990" height="400" alt="image" src="https://github.com/user-attachments/assets/50d75e32-3d44-40f0-ae15-faaf66fa1017" />

Real-world AWS DevOps scenario:
-------------------------------

Suppose you deploy an application using ECS.

<img width="942" height="831" alt="image" src="https://github.com/user-attachments/assets/c1c43d69-db33-438a-91f8-ccd22d558b71" />

There are two things to understand:
-----------------------------------

1. SQS Dead-Letter Queue (DLQ) and redrive policy – what happens when message processing repeatedly fails.
2. SQS CloudWatch monitoring metrics – how a DevOps engineer identifies queue problems.


SQS Dead-Letter Queue (DLQ):
-------------------------------

<img width="975" height="631" alt="image" src="https://github.com/user-attachments/assets/7d7c34b4-3852-44bd-8d0a-681e2e30c1a4" />

<img width="997" height="723" alt="image" src="https://github.com/user-attachments/assets/6312dfd1-1abb-48c6-ab9b-666957c3c116" />

Important correction to the diagram: With maxReceiveCount = 4, 
the message is moved after it exceeds four receives, not simply because four processing attempts failed. 
The redrive happens as part of SQS receive/redrive handling; it isn't an instant action at the exact moment the fourth attempt fails.

What is a redrive policy?

A redrive policy tells SQS:

    - Which DLQ to use.
    - How many times a message can be received before it is moved to the DLQ (maxReceiveCount).
For example:

    {
      "maxReceiveCount": 4,
      "deadLetterTargetArn": "arn:aws:sqs:region:account:order-dlq"
    }


What does the enqueue timestamp mean?
---------------------------------------

The enqueue timestamp is when the message originally entered the source queue.

For a Standard queue, that original timestamp is retained when the message moves to the DLQ.
This is why the DLQ retention period should generally be longer than the source queue's retention period.

Eg: for standerd queue if the message has 4 days lifespan. first 2 day it will sit in standerd queue and it was moved to DLQ then the message Lifespan was only 2 days.
    the countdount will continue not reset to 0 after chnaging the queue


For a FIFO queue, the enqueue timestamp resets when the message moves to the DLQ.

Eg: for FIFO queue the message has 4 days life span if it sits in 2 daysin FIFO queue. then it moves to DLQ then countdowm will reset again to 0.


DevOps use case: If a payment message repeatedly fails, inspect it in the DLQ, identify the application or database error, fix the issue,
and then redrive the message when it is safe to process again.

SQS CloudWatch monitoring metrics:
-----------------------------------

Now imagine you are the DevOps engineer responsible for order-queue.

You need to answer these questions:

    - How many messages are waiting?
    - How many are currently being processed?
    - How long have messages been waiting?
    - Are messages coming in faster than they are being processed?

CloudWatch provides SQS metrics to answer these questions.

# ApproximateNumberOfMessagesVisible:

Waiting messages

This tells you approximately how many messages are available in the queue for consumers to receive.

Example:
    
    order-queue
    Visible messages = 500


Meaning: Approximately 500 messages are waiting to be processed.

Production problem: If this number keeps increasing, your consumer application may not be processing messages fast enough.

# ApproximateNumberOfMessagesNotVisible

Messages being processed

    This tells you approximately how many messages have been received by consumers but are currently hidden because of the visibility timeout.

Example:

    order-queue
    NotVisible messages = 100


Meaning: Approximately 100 messages are currently in flight, meaning they have been received and have not yet become visible again or been deleted.

Production problem: If this number stays very high, investigate whether consumers are processing slowly, getting stuck, or failing to delete messages.


# ApproximateAgeOfOldestMessage

Waiting time

This tells you approximately how long the oldest available message has been waiting in the queue.

Example:
ApproximateAgeOfOldestMessage = 600 seconds


600 seconds = 10 minutes.

Meaning: The oldest available message has been waiting approximately 10 minutes.

Production problem: If this value keeps increasing, customers' orders may be delayed.

# NumberOfMessagesSent

Incoming messages

This tells you how many messages have been successfully added to the queue.

Example:
NumberOfMessagesSent = 1000


Meaning: 1,000 messages were sent to the queue during the selected CloudWatch time period.

# NumberOfMessagesDeleted

Deleted after processing
This tells you how many messages were successfully deleted from the queue by consumers.

Example:

    NumberOfMessagesDeleted = 800


Meaning: 800 messages were successfully deleted during the selected time period.

Remember: SQS doesn't automatically delete a message just because a consumer receives it. The consumer must explicitly delete it after successful processing.

<img width="892" height="358" alt="image" src="https://github.com/user-attachments/assets/e05b9a45-7e73-4920-8822-04aa97c3e3b9" />
