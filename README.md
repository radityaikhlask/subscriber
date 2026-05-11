# Subscriber

## What is AMQP?

AMQP stands for Advanced Message Queuing Protocol. It is a protocol used for communication between applications through a message broker. In this tutorial, AMQP is used so that the publisher can send events to RabbitMQ, and the subscriber can receive and process those events from RabbitMQ.

## What does `guest:guest@localhost:5672` mean?

In the URL:

```txt
amqp://guest:guest@localhost:5672

## Simulation slow subscriber

![Slow subscriber queue](slow-subscriber-queue.png)

In this simulation, I added `thread::sleep(ten_millis);` inside the subscriber handler so that each message takes around 1 second to process. After that, I ran the publisher several times quickly. Each publisher run sends 5 `UserCreatedEventMessage` events, so running the publisher multiple times creates many messages in a short time.

The screenshot above shows that the queued messages increased temporarily. This happened because the publisher sent messages faster than the subscriber could process them. RabbitMQ stored the messages in the queue while waiting for the subscriber to consume them one by one.

This demonstrates the usefulness of a message broker in event-driven architecture. When the consumer is slower than the producer, RabbitMQ can buffer the messages instead of losing them.

## Reflection and Running at least three subscribers

![Three subscribers RabbitMQ](three-subscribers-rabbitmq.png)

![Three subscribers console 1](three-subscribers-console-1.png)

![Three subscribers console 2](three-subscribers-console-2.png)

![Three subscribers console 3](three-subscribers-console-3.png)

In this experiment, I ran three subscriber consoles at the same time. All subscribers connected to the same RabbitMQ message broker and listened to the same `user_created` queue. After that, I ran the publisher several times quickly. Each publisher execution sent 5 `UserCreatedEventMessage` events to RabbitMQ.

The RabbitMQ dashboard shows 3 connections, 3 channels, and 3 consumers. This means all three subscribers were connected and ready to consume messages from RabbitMQ.

The console outputs also show that messages were processed by different subscriber instances. This means the workload was distributed between the subscribers. Compared to the previous slow subscriber simulation with only one subscriber, the queue can be reduced faster because RabbitMQ can deliver messages to multiple consumers.

This demonstrates scalability in an event-driven architecture. When one consumer is too slow, we can add more subscriber instances to process messages in parallel. The publisher does not need to know how many subscribers exist. It only sends events to RabbitMQ, and RabbitMQ manages the delivery to available consumers.

One possible improvement is to avoid hardcoding the AMQP URL directly in `main.rs`. It would be better to store the URL in an environment variable so the same code can run in different environments. Another possible improvement is to avoid duplicating the `UserCreatedEventMessage` struct in both publisher and subscriber by moving the shared message definition into a common crate or library.