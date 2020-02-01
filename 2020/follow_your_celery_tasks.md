# Follow your Celery Tasks

https://fosdem.org/2020/schedule/event/python2020_celery/

```
Celery is an asynchronous task queue/job queue based on distributed message passing. It is focused on real-time operation, but supports scheduling as well.

The execution units, called tasks, are executed concurrently on a single or more worker servers using multiprocessing, Eventlet, or gevent. Tasks can execute asynchronously (in the background) or synchronously (wait until ready).
```

Choose a broker (Redis, RabbitMQ, files).

Build a Python app that creates tasks, etc. and have its config point to the backend broker.

You then run one or more "celery workers". These is a generic program (just invoked using `celery`) which looks at the configured broker for tasks and executes them.

You can:

* Chain tasks sequentially
* Run tasks in parallel in a group

## Pain Points with Celery / Extra Requirements

* The speaker's team found it hard to track task evolution, the status of the ETL pipeline, etc.
* Also wanted to execute tasks via an API call, _not_ in the Python celery producer process directly. (e.g. like chronos)
* Create workflows in YAML
* Be able to periodically execute the workflows (e.g. at a schedule, like chronos -- a scheduler)
* Intelligently restart failed workflows due to failed asks (but only relaunch the tasks that were necessary, given the assumption tasks are idempotent).

The speaker built a tool -- `celery-director`. Installs new Python command -- `director`.

```
pip3 install celery-director
```

# `celery-directory` Usage:

Define Celery tasks as Python functions using the `@director.task`. The decorator takes the name of the task. Then define a YAML file containing your workflows:

```
workflow_name:
    tasks:
        - EXTRACT
        - TRANSFORM
        - LOAD
```

The above defines an ordered sequence. You can specify _groups_ of tasks that can run parallel like so:

```
workflow_name:
    tasks:
        - EXTRACT
        - TODO
        - LOAD
```

Various CLI utilities to inspect, add, remove and run worflows.

`director workflow list`
`director workflow run ...`

Then run a celery worker to consume the jobs from backend (e.g. Redis).

View the state of all workflows/tasks in a web UI by running:

```
director ...something
```

You can also send HTTP requests to the local celery `director` to execute workflows, query state, etc.
