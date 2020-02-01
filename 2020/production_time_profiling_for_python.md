# Production-time Profiling for Python

https://fosdem.org/2020/schedule/event/python2020_profiling/

https://fosdem.org/2020/schedule/event/python2020_profiling/attachments/slides/3612/export/events/attachments/python2020_profiling/slides/3612/Production_time_profiling_for_Python_FOSDEM_2020.pdf

If you're program is slow, it's hard to guess which part of code is consumping all the CPU and mem. Goal of profiling is to get inside your program and get insights into the resource usage of different parts of code (e.g. n% of CPU in this line). Then you can focus on optimising the specific bits of code that take most time.

Two types of profiling:

* Deterministic -- run a scenario, measure all execution times by function.
    - e.g. cProfile
    - cProfile shortcomings are only wall time, only function granularity so larger functions don't give you more insight. If thousands of functions, so much mor overhead and the looking at the measurements might be hard with so many functions.
* Statistical -- faster, less instrumentation and so can run in production. Can be done in a per line basis. Runs over few hours or days of your program running.
    - wake up
    - register what the program's doing right now
    - go back to sleep
    - repeat
    - BENEFITS: can measure CPU time, very low overhead, can each specific lines of code that can be executed

For prod systems, you need statistical. Gives you better insight into what really takes a lot of time and doesn't slow your prod systems like deterministic.

For memory profiling in Python, `tracemalloc` is used. Replaces `malloc` used in CPython by its own wrapper that keeps track of the allocations. `tracemalloc` maps the allocations on C-level to the corresponding Python line.

It measures:
    - allocation count
    - allocation size

Downsides:

* it's _really_ slow -- therefore, can't do this in a prod system as-is.
* no per-thread information
* only file names and numbers, not function

But you can use in the prod system by sleeping, waking up and enabling `tracemalloc` for a bit, then disabling it and sleeping again. i.e. statistical memory profling.

Thread profiling in Python is very limited. Not a big issue since threads are often used in Python. But if you want to do it, one solution that's been implemented is to intercept `threading.Lock` instances and collecting info like total lock acquire/wait time, total time a lock is held, how many locks exist, etc.

For all the above tools, DataDog decided to export the profling info in a reasonably **standard profling data format**. This is **pprof**: protobuf profiling data format. It's a format and also a CLI tool to analyse/visualise pprof files.

DataDog has a (soon to be) open source library for implementing all of the above production profiling in Python techniques: `dd-trace-py`.

Will be open sourced in a couple of weeks.
