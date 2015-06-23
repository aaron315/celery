.. _intro:

========================
 Introduction to Celery
 Celery介绍
========================

.. contents::
    :local:
    :depth: 1

What is a Task Queue?
什么是任务队列
=====================

Task queues are used as a mechanism to distribute work across threads or
machines.
任务队列是通过线程或者机器分发任务的机制。

A task queue's input is a unit of work called a task. Dedicated worker
processes constantly monitor task queues for new work to perform.
一个任务队列的输入是被成为task的工作单元。专门的worker为新任务执行持续监控任务队列。

Celery communicates via messages, usually using a broker
to mediate between clients and workers.  To initiate a task, a client adds a
message to the queue, which the broker then delivers to a worker.
Celery通过笑死通信，通常使用一个borker作为客户端和worker的媒介。发起一个任务，客户端向队列发送一个消息，broker将任务分发到worker。

A Celery system can consist of multiple workers and brokers, giving way
to high availability and horizontal scaling.
一个Celery系统由多个worker和broker组成，提供高可用性和水平扩展性。

Celery is written in Python, but the protocol can be implemented in any
language.  So far there's RCelery_ for the Ruby programming language,
node-celery_ for Node.js and a `PHP client`_. Language interoperability can also be achieved
by :ref:`using webhooks <guide-webhooks>`.
Celery是Python实现，但是协议可以由任务语言实现。目前有Ruby编程语言的RCelery_，Node.js的node-celery_ 和`PHP client`_.
语言之间的教书可以由:ref:`using webhooks <guide-webhooks>` 实现。
.. _RCelery: http://leapfrogdevelopment.github.com/rcelery/
.. _`PHP client`: https://github.com/gjedeer/celery-php
.. _node-celery: https://github.com/mher/node-celery

What do I need?
我需要做什么？
===============

.. sidebar:: Version Requirements
    :subtitle: Celery version 3.0 runs on

    - Python ❨2.5, 2.6, 2.7, 3.2, 3.3, 3.4❩
    - PyPy ❨1.8, 1.9❩
    - Jython ❨2.5, 2.7❩.

    This is the last version to support Python 2.5,
    and from the next version Python 2.6 or newer is required.
    The last version to support Python 2.4 was Celery series 2.2.

*Celery* requires a message transport to send and receive messages.
The RabbitMQ and Redis broker transports are feature complete,
but there's also support for a myriad of other experimental solutions, including
using SQLite for local development.
*Celery* 需要一个消息运输系统发送和接收消息。RabbitMQ和Redis是特性完全支持，同事也支持大量的其他实验方案，包含针对本地开发的SQLite。

*Celery* can run on a single machine, on multiple machines, or even
across data centers.
*Celery*可以运行在一个机器，或者多个机器上，或者跨越数据中心。

Get Started
开始
===========

If this is the first time you're trying to use Celery, or you are
new to Celery 3.0 coming from previous versions then you should read our
getting started tutorials:
如果这是你第一次尝试使用Celery,或者你是来自Celery3.0以前的版本你应该阅读我们的getting started tutorials

- :ref:`first-steps`
- :ref:`next-steps`

Celery is…
Celery是......
==============

.. _`mailing-list`: http://groups.google.com/group/celery-users

.. topic:: \ 

    - **Simple简单**

        Celery is easy to use and maintain, and it *doesn't need configuration files*.Celery容易使用和维护，它不需要  配置文件。

        It has an active, friendly community you can talk to for support,
        including a `mailing-list`_ and an :ref:`IRC channel <irc-channel>`.
        它有一个活跃的，友好的社区你可以获得支持，包含一个`mailing-list`_ 和 an :ref:`IRC channel <irc-channel>`。

        Here's one of the simplest applications you can make:
        这是一个你可以使用的最简单的应用程序。


        .. code-block:: python

            from celery import Celery

            app = Celery('hello', broker='amqp://guest@localhost//')

            @app.task
            def hello():
                return 'hello world'

    - **Highly Available高可用**

        Workers and clients will automatically retry in the event
        of connection loss or failure, and some brokers support
        HA in way of *Master/Master* or *Master/Slave* replication.
        当连接丢失或者失败worker和clients自动重试，和一些brokers支持的*Master/Master* or *Master/Slave* replication        高可用方法。

    - **Fast快速**

        A single Celery process can process millions of tasks a minute,
        with sub-millisecond round-trip latency (using RabbitMQ,
        py-librabbitmq, and optimized settings).
        单个Celery进程可以每分钟可以处理百万级的任务，毫秒级的往返延迟（使用RabbitMQ，py-librabbitmq，和优化设置）

    - **Flexible灵活性**

        Almost every part of *Celery* can be extended or used on its own,
        Custom pool implementations, serializers, compression schemes, logging,
        schedulers, consumers, producers, autoscalers, broker transports and much more.
        Celery都可以扩展或者使用它原有的部分，自定义池实现，序列化，压缩方案，日志，调度器，消费者，生产者，自动扩容
        ，broker传送等等。


.. topic:: It supports

    .. hlist::
        :columns: 2

        - **Brokers**

            - :ref:`RabbitMQ <broker-rabbitmq>`, :ref:`Redis <broker-redis>`,
            - :ref:`MongoDB <broker-mongodb>` (exp), ZeroMQ (exp)
            - :ref:`CouchDB <broker-couchdb>` (exp), :ref:`SQLAlchemy <broker-sqlalchemy>` (exp)
            - :ref:`Django ORM <broker-django>` (exp), :ref:`Amazon SQS <broker-sqs>`, (exp)
            - and more…

        - **Concurrency**

            - prefork (multiprocessing),
            - Eventlet_, gevent_
            - threads/single threaded

        - **Result Stores**

            - AMQP, Redis
            - memcached, MongoDB
            - SQLAlchemy, Django ORM
            - Apache Cassandra

        - **Serialization**

            - *pickle*, *json*, *yaml*, *msgpack*.
            - *zlib*, *bzip2* compression.
            - Cryptographic message signing.

Features
特性
========

.. topic:: \ 

    .. hlist::
        :columns: 2

        - **Monitoring监控**

            A stream of monitoring events is emitted by workers and
            is used by built-in and external tools to tell you what
            your cluster is doing -- in real-time.
            监控事件流是由worker和内建和扩展的工具发出，他实时告诉你你的集群正在做什么。

            :ref:`Read more… <guide-monitoring>`.

        - **Workflows工作流**

            Simple and complex workflows can be composed using
            a set of powerful primitives we call the "canvas",
            including grouping, chaining, chunking and more.
            简单和复杂工作流可以组合使用，我们称为canvas的有一组强大的原语组成，包含grouping，chaining,chunking等。

            :ref:`Read more… <guide-canvas>`.

        - **Time & Rate Limits时间和限制频率**

            You can control how many tasks can be executed per second/minute/hour,
            or how long a task can be allowed to run, and this can be set as
            a default, for a specific worker or individually for each task type.
            你可以控制每秒/分钟/小时执行多少任务,或者每个任务可以执行多长时间，这些可以设为默认，为每个任务类型一
            个特定或单独的worker。

            :ref:`Read more… <worker-time-limits>`.

        - **Scheduling调度器**

            You can specify the time to run a task in seconds or a
            :class:`~datetime.datetime`, or or you can use
            periodic tasks for recurring events based on a
            simple interval, or crontab expressions
            supporting minute, hour, day of week, day of month, and
            month of year.
            你可以通过秒或者:class:`~datetime.datetime` 指定时间去运行一个任务，你可以通过简单的时间间隔或者crontab
            表达式使用周期任务处理反复事件。

            :ref:`Read more… <guide-beat>`.

        - **Autoreloading自动重新加载**

            In development workers can be configured to automatically reload source
            code as it changes, including :manpage:`inotify(7)` support on Linux.
            在开发worker可以配置当代码变动时候自动重新加载，包含 :manpage:`inotify(7)`。

            :ref:`Read more… <worker-autoreloading>`.

        - **Autoscaling自动扩容**

            Dynamically resizing the worker pool depending on load,
            or custom metrics specified by the user, used to limit
            memory usage in shared hosting/cloud environments or to
            enforce a given quality of service.
            动态设置worker pool大小依赖于加载，或者用户自定义指标在共享机器上限制内存使用，或者抱住那个提供
            高质量的服务。

            :ref:`Read more… <worker-autoscaling>`.

        - **Resource Leak Protection 资源泄露保护**

            The :option:`--maxtasksperchild` option is used for user tasks
            leaking resources, like memory or file descriptors, that
            are simply out of your control.
            选项:option:`--maxtasksperchild` 用来限制资源泄露，像内存或者文件描述符,他们可以简单的由你控制。

            :ref:`Read more… <worker-maxtasksperchild>`.

        - **User Components用户组件**

            Each worker component can be customized, and additional components
            can be defined by the user.  The worker is built up using "bootsteps" — a
            dependency graph enabling fine grained control of the worker's
            internals.
            每一个worker组件可以被自定义，用户可以定义组件的组件。worker使用bootsteps,一个依赖图可以细粒度
            的控制worker内部组件。

.. _`Eventlet`: http://eventlet.net/
.. _`gevent`: http://gevent.org/

Framework Integration
框架集成
=====================

Celery is easy to integrate with web frameworks, some of which even have
integration packages:

    +--------------------+------------------------+
    | `Django`_          | `django-celery`_       |
    +--------------------+------------------------+
    | `Pyramid`_         | `pyramid_celery`_      |
    +--------------------+------------------------+
    | `Pylons`_          | `celery-pylons`_       |
    +--------------------+------------------------+
    | `Flask`_           | not needed             |
    +--------------------+------------------------+
    | `web2py`_          | `web2py-celery`_       |
    +--------------------+------------------------+
    | `Tornado`_         | `tornado-celery`_      |
    +--------------------+------------------------+

The integration packages are not strictly necessary, but they can make
development easier, and sometimes they add important hooks like closing
database connections at :manpage:`fork(2)`.

.. _`Django`: http://djangoproject.com/
.. _`Pylons`: http://pylonshq.com/
.. _`Flask`: http://flask.pocoo.org/
.. _`web2py`: http://web2py.com/
.. _`Bottle`: http://bottlepy.org/
.. _`Pyramid`: http://docs.pylonsproject.org/en/latest/docs/pyramid.html
.. _`pyramid_celery`: http://pypi.python.org/pypi/pyramid_celery/
.. _`django-celery`: http://pypi.python.org/pypi/django-celery
.. _`celery-pylons`: http://pypi.python.org/pypi/celery-pylons
.. _`web2py-celery`: http://code.google.com/p/web2py-celery/
.. _`Tornado`: http://www.tornadoweb.org/
.. _`tornado-celery`: http://github.com/mher/tornado-celery/

Quickjump
=========

.. topic:: I want to ⟶

    .. hlist::
        :columns: 2

        - :ref:`get the return value of a task <task-states>`
        - :ref:`use logging from my task <task-logging>`
        - :ref:`learn about best practices <task-best-practices>`
        - :ref:`create a custom task base class <task-custom-classes>`
        - :ref:`add a callback to a group of tasks <canvas-chord>`
        - :ref:`split a task into several chunks <canvas-chunks>`
        - :ref:`optimize the worker <guide-optimizing>`
        - :ref:`see a list of built-in task states <task-builtin-states>`
        - :ref:`create custom task states <custom-states>`
        - :ref:`set a custom task name <task-names>`
        - :ref:`track when a task starts <task-track-started>`
        - :ref:`retry a task when it fails <task-retry>`
        - :ref:`get the id of the current task <task-request-info>`
        - :ref:`know what queue a task was delivered to <task-request-info>`
        - :ref:`see a list of running workers <monitoring-control>`
        - :ref:`purge all messages <monitoring-control>`
        - :ref:`inspect what the workers are doing <monitoring-control>`
        - :ref:`see what tasks a worker has registered <monitoring-control>`
        - :ref:`migrate tasks to a new broker <monitoring-control>`
        - :ref:`see a list of event message types <event-reference>`
        - :ref:`contribute to Celery <contributing>`
        - :ref:`learn about available configuration settings <configuration>`
        - :ref:`receive email when a task fails <conf-error-mails>`
        - :ref:`get a list of people and companies using Celery <res-using-celery>`
        - :ref:`write my own remote control command <worker-custom-control-commands>`
        - :ref:`change worker queues at runtime <worker-queues>`

.. topic:: Jump to ⟶

    .. hlist::
        :columns: 4

        - :ref:`Brokers <brokers>`
        - :ref:`Applications <guide-app>`
        - :ref:`Tasks <guide-tasks>`
        - :ref:`Calling <guide-calling>`
        - :ref:`Workers <guide-workers>`
        - :ref:`Daemonizing <daemonizing>`
        - :ref:`Monitoring <guide-monitoring>`
        - :ref:`Optimizing <guide-optimizing>`
        - :ref:`Security <guide-security>`
        - :ref:`Routing <guide-routing>`
        - :ref:`Configuration <configuration>`
        - :ref:`Django <django>`
        - :ref:`Contributing <contributing>`
        - :ref:`Signals <signals>`
        - :ref:`FAQ <faq>`
        - :ref:`API Reference <apiref>`

.. include:: ../includes/installation.txt
