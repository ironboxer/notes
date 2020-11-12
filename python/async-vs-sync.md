### async vs sync



- There is high load (without high load there is no advantage in having access to high concurrency)
- The tasks are I/O bound (if the tasks are CPU bound, then concurrency above the number of CPUs does not help)
- You look at average number of requests handled per unit of time. If you look at individual request handling times you will not see a big difference, and async may even be slightly slower due to having more concurrent tasks competing for the CPU(s).





---



- An async application will only do better than a sync equivalent under high load.
- Thanks to greenlets, it is possible to benefit from async even if you write normal code and use traditional frameworks such as Flask or Django.