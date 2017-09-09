### Asynchronous programming with Coroutines in Python

New Cryptography and Techniques

Date: Sun 5th Feb
Time: 14:00
Speaker: Ewoud Van Craeynest

Coroutines arw a style of writing code, that avoids the use of block calls. Rather, an event loop does the heavy scheduling work for you.

What's the issue with blocking APIs?

It makes your thread is "busy", it's doing nothing useful. With event scheduling, we utilise all thread resources at all times.

Yes, threads were designed for OS multitasking. Context switches are expense, doesn't scale well. Think large numbers of sockets (e.g. 10K)

Synchronisaiton is hard to get right -- unpredictable schedulng, race conditions, deadlock, starvation. 

Threads -- the goto of our generation, at least *for concurrency*

In the multicore age, we do need threads for parallelism and processing. We just want to use less of them.

AsyncIO coroutines doesn't solve us crunching data in parallel, we need real threads for that. But they're good for blocking IO.

Python 

* gevent
* tulip (now asyncio)
* twisted (event driven)
* tornado

All of them hacked on top of Python 2.

Coroutines are just special functions that can be **suspended** and **resumed**.

```
async def print_hello():
    while True:
        print("Foo")
        await asyncio.sleep(3)

asyncio.s
```

Callback style asyn was used intensively in the past. It escalates quickly into a cascade of callbacks -- **callback hell**. Very hard to understand what's going on.

Allows you to write code as if it is synchronous, but allows for concurrency.

`asyncio.org` is good list of asyncio compatible Python libraries.

If you have a blocking function that does not support asyncio, then put the blocking call on a `ThreadExecutor`.

> NOTE: File IO calls C -- when you call FFI functions in CPython the GIL is
> locked? So using an asyncio-style event loop makes no difference???

Test asyncio code using `asynctest` package.

Alternative to asyncio is `uvloop`, which implements the event loop using `libuv`. It's apparently much faster.

`curio` is an alternative with a different API. Faster than `asyncio` and `uvloop` apparently.