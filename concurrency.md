## concurrency: what is it and how do programmers use it?
(numbers under terms represent different definitions/phrasings/explanations/ways of thinking about the term/etc)

----------------------------
### various words that I maybe know how to define:

**actors**

**asynchronous**
In the loosest way, it is a way to execute code non-sequentially (non-blocking operations, etc.)
commonly used with event loop, I think.

**atomics**
values that guarantee 1) fast lookup 2) threadsafe? allocated on the stack, I think? (in retrospect, being allocated on the stack doesn't seem to imply any kind of threadsafe-ness, since each thread would have it's own stack)

**channels**
in go, a unidirectional way to send data from one goroutine to another.
I think they behave somewhat similarly in Rust

**context switching**
happens whenever a new thread is scheduled to run. the old thread's state needs to be saved (register values, [more stuff to be looked up later]) and the new thread's state needs to be loaded (register values, [more stuff to be looked up later])

**continuation passing style**

**cooperative (async) concurrency/multitasking**
1. assume all your code can't run concurrently and mark all the places where it can
2. threads are interrupted ("hey! I'm the scheduler and I'm here to say you can't run anymore!")
3. the language runtime can't/won't run tasks/"threads" on multiple processors

**coroutine**

**data parallelism**

**event loop**
used with asynchronous programming?
non-blocking operations?
callbacks?

**Futures**

**greenthreads (aka "goroutines" or "erlang/elixir processes")**
units of computation that are managed by the language runtime/interpreter
Basically, the language will create some number of OS threads, then take these GTs (greenthreads) and plop them on different OS threads and/or processors as it sees fit.
[needs citation] Some advantages (over using OS threads directly) may be: faster context switching because the language knows more about each unit than the OS knows about OS threads (what this means in practice (i.e. what the language knows about each unit) varies from language to language I think)

**hazard pointers**

**kernel dispatching**
process by which an OS determines which of the active threads is sent (dispatched) to the CPU

**kernel schedulable entities**
roughly equivalent to threads, but different

**message passing**

**OS threads**
"special case of OS processes"

**OTP**

**preemptive concurrency/multitasking**
1) assume all your code can run concurrently and mark all the places where it can't
2) threads relinquish the processor ("here you go, scheduler. I'm all done with the processor")

**RCU: read-copy-update**

**safe memory reclamation**

**segmented stacks**
dynamic stack-growing for threads?

**supervision trees**

**work-sharing schedulers**
when a processor generates new threads, it tries to move them to other processors that might be idle/underutilized

**work-stealing schedulers**
an idle/underutilized processor will actively look for threads to "steal" from other processors
thread migration will occur less frequently in this scenario than with work-sharing

----------------------------

## Languages and their contructs/
### python
asyncio
trio
threading
multiprocessing


Python's GIL (global interpreter lock) make preemptive concurrency impossible because you can't run two things at exactly the same time (basically, I think).
Python's async/await keywords, then, are ways that the programmer can tell Python where it's allowed to switch tasks on em

----------------------------

rust
threading is a 1:1 model where each thread is actually an OS thread, not one of these green threads parading around as a real thread

----------------------------

## Questions:

**What makes a thread "lightweight"?***
from elixir homepage: All Elixir code runs inside lightweight threads of execution
from https://gobyexample.com/goroutines: A goroutine is a lightweight thread of execution.

"Lightweight threads" and "green threads" are commonly-interchanged terms, and typically they mean:
* units of computation that can execute indepedently of each other
* a language construct that will (sometimes) map these units to actual OS threads

**Do green threads minimize the cost of context switching by reusing OS threads? Do green threads even reuse OS threads?**
