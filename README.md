# Subscriber

## What is AMQP?

AMQP stands for Advanced Message Queuing Protocol. It is a protocol used for communication between applications through a message broker. In this tutorial, AMQP is used so that the publisher can send events to RabbitMQ, and the subscriber can receive and process those events from RabbitMQ.

## What does `guest:guest@localhost:5672` mean?

In the URL:

```txt
amqp://guest:guest@localhost:5672