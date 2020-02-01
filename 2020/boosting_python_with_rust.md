# Boosting Python with Rust

https://fosdem.org/2020/schedule/event/python2020_rust/

Reimplementing Mercurial in Rust, over Python. For improved performance.

Doing it gradually, by implementing specific Python functions and data structures in Rust and using them in the Python code.

Data exchange is expensive, so choose the Python<-> Rust boundary carefully. e.g. don't call Rust routines in tigh loops in Python, because the overhead of doing that is large.

Not actually deployed to production Mercurial yet?

Open questions: would it better to have Rust only commands? Perhaps not, since each command re-uses so many common Python modules in Mercurial. Would have to re-implement a LOT of code before even finishing a single command. No business value in spending hundreds of hours on that, so perhaps it is indeed worth bothering with Rust/Python integration instead of re-implementing in Rust?
