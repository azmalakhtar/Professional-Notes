# Introduction to Coroutines
As nowadays most of the systems are interconnected, they most of the time communicate with each other and do nothing. So the problem is **How do we write code that waits for something most of the time?**.

We can solve this problem using *threads*, but the problem is that when we encounter a wait on any thread, it blocks the thread. Thus this thread can not be used for any other process. This won't be problem if we could create threads without any overhead, but threads have memory overhead.

The other solution was to use *callbacks*. The problem was callback hell.

Then comes the *Futures/Promises*. They surely avoid the callback hell, but nonetheless they have many combinators which varies for different libraries.

---
### Reference
- https://www.youtube.com/watch?v=_hfBv0a09Jc