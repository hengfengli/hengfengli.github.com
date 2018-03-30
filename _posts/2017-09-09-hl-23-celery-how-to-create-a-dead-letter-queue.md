---
layout: post
title: "[HL-23] How to create a dead letter queue in Celery + RabbitMQ"
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

<script src="https://gist.github.com/HengfengLi/11f268cfb990772c8b40c6c55ffc5b37.js"></script>

You need a rabbitmq as a broker and redis as a backend. Run `docker-compose up -d` to start containers.

<script src="https://gist.github.com/HengfengLi/e0a2e84915138070811e8086712e6e3c.js"></script>

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
