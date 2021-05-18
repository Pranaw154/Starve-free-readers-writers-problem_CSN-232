# The Readers-Writers Problem

## Introduction to the Problem
There is a ***database*** which is shared with several processes which are running ***concurrently***. Some of the processes are ***readers*** (who want to read data only) and others are ***writers*** (who want to update the data). 
Now there can be three different situations. **First** being simultaneous access of the data by readers only, which causes no problem. **Second** case occurs when both reader and writer want to access the data simultaneously. Finally, the **third** case is when more than one writer is simultaneously trying to update the data. In **second** and **third** case there can be a problem of synchronization.

Now there are different variants of the readers-writers problem

### First readers-writers problem
This is the simplest variation of the readers-writers problem.
> No reader should wait for accessing the data unless there is a writer who is currently having the permission to access the shared data.

To put it simply, suppose if a reader is currently accessing the data and a writer is in waiting state. Then in this case if a new reader arrives, then he do not need to wait. A reader will wait only when a writer is in the critical section. The priority is given to the readers.
> In this case the writers may starve.

### Second readers-writers problem
It is same as the first readers-writers problem, only exception is that the priority is given to the writers in this case.
> If both writer and reader are in waiting state, writers perform their tasks first.

Again, a reader can access the data only if there is no writer waiting to access the data.
> In this case the readers may starve.

### Third readers-writers problem
It is also known as ***Starve Free Readers Writers Problem*** because it solves the problem of starvation which were caused in the above two variants. In this case priority is given to the processes (readers or writers) on the basis of the ***arrival*** of their requests (to access the data).

Now we can move to it's implementation part in *[implementation.md](./implementation.md)*