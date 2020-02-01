# Introduction to Reactive Programming with RxPY

https://fosdem.org/2020/schedule/event/python2020_reactive_programming/

https://fosdem.org/2020/schedule/event/python2020_reactive_programming/attachments/slides/3719/export/events/attachments/python2020_reactive_programming/slides/3719/fosdem2020_rxpy

Reactive programming is a way to write event driven code.

Build a computation graph whose root nodes are triggered when events occur.

Nodes = transformation
Edges = represents flow of data

In practice, most reactive programming graphs used in the wild are acylic. But there do exist cases where graph has cycles (note to self: how to prevent infinite recursion then?)

RxPY is the Python implementation of ReactiveX. V3 released in summer 2019.

### Nomenclature

Observable: a stream of events
Observer: subscribes to an Observable
Operator: applies transformations on items

### Designing

Can use *marble diagrams* to show data flow, etc.

Like UML data flow diagrams, but rotated 90 degrees.

### Examples of RxPy in Code

Look at slides linked above.

### Concurrency

It's possible to execute different nodes of a graph concurrently.

Can configure diff schedulers, like asyncio ones.

CPU concurrency: process pool, new pools

IO concurrency: asyncio driven nodes

### Modularity/Testability

Can use the cyclotron lib (https://github.com/mainro/cyclotron-py) to separate pure code nodes and nodes w/ side effects from each other in RxPY.
