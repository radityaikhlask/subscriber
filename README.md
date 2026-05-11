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