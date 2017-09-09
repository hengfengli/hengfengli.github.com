---
layout: post
title: "[HL-16] How to create a dead letter queue in Celery + RabbitMQ"
categories: 
    - development
tags: 
    - Python
    - celery
    - dead letter queue
published: true
---

Recently, I started learning how to run async tasks in Celery, which has been used in our company product. One interesting requirement is how to create a 
dead letter queue in Celery? 

RabbitMQ has already supported arguments, `x-dead-letter-exchange` and `x-dead-letter-routing-key`, when creating a new queue. 

I have made a simple example to create a dead letter queue: 

```bash
# docker-compose.yml
version: "2"
services:
  rabbitmq:
    container_name: rabbitmq
    image: rabbitmq:3.6-management-alpine
    ports:
      - 15672:15672
      - 5672:5672
    networks:
      - my_network
  redis:
    container_name: redis
    image: redis:3.2-alpine
    ports:
      - 6379:6379
    networks:
      - my_network

networks:
  my_network:
    driver: bridge
```

You need a rabbitmq as a broker and redis as a backend. Run `docker-compose up -d` to start containers. 

```python
# tasks.py
from celery import Celery
from kombu import Exchange, Queue
from celery.exceptions import Reject

app = Celery(
    'tasks',
    broker='amqp://guest@localhost:5672//',
    backend='redis://localhost:6379/0')

dead_letter_queue_option = {
    'x-dead-letter-exchange': 'dlx',
    'x-dead-letter-routing-key': 'dead_letter'
}

default_exchange = Exchange('default', type='direct')
dlx_exchange = Exchange('dlx', type='direct')

default_queue = Queue(
    'default',
    default_exchange,
    routing_key='default',
    queue_arguments=dead_letter_queue_option)
dead_letter_queue = Queue(
    'dead_letter', dlx_exchange, routing_key='dead_letter')

app.conf.task_queues = (default_queue, dead_letter_queue)

app.conf.task_default_queue = 'default'
app.conf.task_default_exchange = 'default'
app.conf.task_default_routing_key = 'default'


@app.task
def add(x, y):
    return x + y


@app.task(acks_late=True)
def div(x, y):
    try:
        z = x / y
        return z
    except ZeroDivisionError as exc:
        raise Reject(exc, requeue=False)
```

First run the following command to create queues and start a worker for getting tasks from all queues: 

```bash
celery -A tasks worker --loglevel=info
```

Then, you can just run a worker to only receive tasks from the `default` queue: 

```bash
celery -A tasks worker --loglevel=info -Q default
```

```python
from tasks import add, div
add.delay(1,2)
div.delay(2,1)
div.delay(2,0) # throws ZeroDivisionError
```

`Reject` has to be used with `acks_late` together, because by default the worker will acknowledge to remove the message from the queue. If `acks_late` is set, an acknowledgement is sent after the result of task is returned. When `acks_late` is enabled, the worker can reject the task that will be redelivered to a dead letter queue. 