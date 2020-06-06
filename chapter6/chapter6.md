### Terms 
### Chapter6 Strucures
### Questions 
* What's the different and relationship between replication and partition?
> Diff
```
replication: 
1. replication is multiple copies o the same data on different nodes; 
2. replication aim to resolve fault tolerance in distributed system;

partition: 
1. when node's replicated data size and request frequency becomes huge, replicated data gonna broken up into smaller fractions; 
2. partition aims to improve data scalability reduce hot spot and improve parallel resource usage: 
   the more data partitioned more complex query will be brokern in, 
   the more queries will be send to different partitions and parallel executing;   
```

> Relation
```
partition is usually combined with repication, copies of each partition are stored on multiple nodes, 
and each record belongs to exactly partition, it may still be stored on different nodes for fault tolerance;
```

* What's the goal of partition?
1. For data's scalability t reduce hot spots in which multiple requests are sent to single node and this node's load gonna be high; 
2. Large datasets can be distributed across many disks, cpus, more partitions you break up your dataset into, more query load can be distributed across multiple processors; 

* What's the simplest way to avoid hot spot? 
> The simplest way to reduce(avoid) hot spot is assigning records to nodes randomally.
> But, this has a disadvantage that is : when you trying to read a paticular item, a global partitions scanning will be executed.

* What's key range partition, what's th pros and cons of this key range based partition approach?
> 
