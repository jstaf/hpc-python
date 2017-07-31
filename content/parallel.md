---
title: "Running code in parallel"
menu: main
weight: 9
---

Python does not thread very well.
Specifically, Python has what's known as a Global Interpreter Lock (GIL).
The GIL ensures that only one thread can run at a time.
This can make multithreaded processing very difficult.
Instead, the best way to go about doing things is to use multiple independent processes to perform the computations.
This is done using the `multiprocessing` module.

Before we start, we will need the number of CPU cores in our computer.
To get the number of cores in our computer, we can use the `psutil` module.
We are using `psutil` instead of `multiprocessing` because `psutil` counts cores instead of threads.
Long story short, cores are the actual computation units, 
threads allow additional multitasking using the cores you have.
For heavy compute jobs, you are generally interested in cores.

```
import psutil
# logical=True counts threads, but we are interested in cores
psutil.cpu_count(logical=False)
```
```
2
```

Using this number, we can create a pool of worker processes withh which to parallelize our jobs:

```
from multiprocessing import Pool
pool = Pool(psutil.cpu_count(logical=False))
```

The `pool` object gives us a set of parallel workers we can
use to parallelize our calculations.
In particular, there is a map function
(with identical syntax to the `map()` function used earlier),
that runs a workflow in parallel.

```
pool.map()
```

The `chunksize` argument lets you process data in chunks, 
cutting down time lost to inter-process communication.

## [Next section](../sm-intro/)


