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

from celery import bootsteps

default_queue_name = 'default'
default_exchange_name = 'default'
default_routing_key = 'default'
deadletter_suffix = 'deadletter'
deadletter_queue_name = default_queue_name + f'.{deadletter_suffix}'
deadletter_exchange_name = default_exchange_name + f'.{deadletter_suffix}'
deadletter_routing_key = default_routing_key + f'.{deadletter_suffix}'


class DeclareDLXnDLQ(bootsteps.StartStopStep):
    """
    Celery Bootstep to declare the DL exchange and queues before the worker starts
        processing tasks
    """
    requires = {'celery.worker.components:Pool'}

    def start(self, worker):
        app = worker.app

        # Declare DLX and DLQ
        dlx = Exchange(deadletter_exchange_name, type='direct')

        dead_letter_queue = Queue(
            deadletter_queue_name, dlx, routing_key=deadletter_routing_key)

        with worker.app.pool.acquire() as conn:
            dead_letter_queue.bind(conn).declare()


app = Celery(
    'tasks',
    broker='amqp://guest@localhost:5672//',
    backend='redis://localhost:6379/0')

default_exchange = Exchange(default_exchange_name, type='direct')
default_queue = Queue(
    default_queue_name,
    default_exchange,
    routing_key=default_routing_key,
    queue_arguments={
        'x-dead-letter-exchange': deadletter_exchange_name,
        'x-dead-letter-routing-key': deadletter_routing_key
    })

app.conf.task_queues = (default_queue, )

# Add steps to workers that declare DLX and DLQ if they don't exist
app.steps['worker'].add(DeclareDLXnDLQ)

app.conf.task_default_queue = default_queue_name
app.conf.task_default_exchange = default_exchange_name
app.conf.task_default_routing_key = default_routing_key


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

Just run: 

```bash
celery -A tasks worker --loglevel=info
```

```python
from tasks import add, div
add.delay(1,2)
div.delay(2,1)
div.delay(2,0) # throws ZeroDivisionError
```

`Reject` has to be used with `acks_late` together, because by default the worker will acknowledge to remove the message from the queue. If `acks_late` is set, an acknowledgement is sent after the result of task is returned. When `acks_late` is enabled, the worker can reject the task that will be redelivered to a dead letter queue. 


#### Note: 

- Originally, I had the same that if we don't include `deadletter` queue into `app.conf.task_queues` and start workers at first time, `deadletter` queue will not be created. 
- According to [this question](https://stackoverflow.com/questions/38111122/celery-how-can-i-route-a-failed-task-to-a-dead-letter-queue/46128311#46128311), K.P. proposed a solution to create `deadletter` queue which works very well. So I modified my example as well. 
