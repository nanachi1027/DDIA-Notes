## Definitions 

* What's Replication?
```
Replicaiton means keeping a copy of the same data on multiple machines that are connected via a network. 
```

* What's replica
```
Each node that stores a copy of the database is called replica.
```

## Question 

* Why we want to replicate data?
```
1. Keep data geographically close to your users(reduce latency).
2. Allow the system to continue working even if some of its parts have failed(increase availability).
3. Scale out the nuber of machines that serve read queries(increase read throughput).
```

* How do you handle the situation : replicating changes between nodes?
```
There are three mainstream popular algorithms for replicating changes between nodes: single-leader, multi-leader, leaderless replicaiton.
```

* Can you provide the pros. and cons. among those replicing changes algorithms above? 

```
```

* How does active/passive, master-slave replication work when client write && read from the database?
```
One of the replicas is designated the leader which can also known as master or primary. 

And other replicas are known as the followers which can also known as read replicas, slaves, sencondaries or hot standbys. 

Write : 

When clients want to write to database, they must send their requests to the leader.

When the leader receives the requests from which they will extract data and write data to local storage first. 
When leader write new received data to its local storage, it also send the data change to all of its followers as a part of replication log or cange stream. 


When the follower takes the log from the leader and updates its local copy of data base accordingly, by applying all writes in the same order as they were processed on the leader. 


Read:
When a client wants to read from the database, it can query either the leader or any of the followers. 
However, writes are only accepted on the leader. 
```

* Do you know this mode of replication is deployed in which databases?
```
Leader based replications is a build-in feature of many relational databases, like: PostSQL(since verison 9.0), MySQL, Oracle Data Guard, and SQL Server's AlwaysOn Availability Groups.

NoSQL databases like MongoDB, RethinkDB and Espresso also used leader based replicaitons mode.

Distributed messages brokers like Kafka and RabbitMQ highly available queues also adopt this leader based replciation mode. 
```

* What's the difference between sync and async? What about the block and non-block
```

```

* What's the differences between for leader based replication mode when followers work in sync and async mode ?
```
In leader-based replication mode, when leader send the message of data change to followers which work in sync: the leader waits until follower has confirmed that it received the write request before reporting success to the user, and before making the write visible to other clients. 

However, if the followers work in async: the leader sends the message will not wait for responses from followers. After sending message to followers it directly reporting success to the user and making the write visible to other clients. 
```

* Please analize what's the cons and pros between the followers work in async and sync, and can you provide some examples for these special scenarios? 
```
Pros. async 
Replication is quite fast: most database systems apply changes to followers in less than a second. 


Cons. async 
No guarantee of how long it might take. The followers might delayed from the pace of leader caused by network issues, follower works under greate pressure. 
Cause for client reading from the system either leader or any follower can be the one who response the request from the client.
However, if the follower is working asynchronously and dealy from the pace of leader, the reading will get different answer if it query the leader and follower at the same time which will result in apprent inconsistency. 



Pros. sync 
The follower is guaranteed to have an up-to-date copy of the data that is consistent with the leader.
If the leader failes, we can be sure that the data is still available on the follower.

Cons. sync 
If the synchronous follower doesn't respond(crashed, network fault, other reasons), the write cannot be processed.
Leader must block all writes and wait until the synchronous replica is available again. 
```

* Is it practical for all followers work synchronously? Why? 
```
It is impractical for all followers to be synchronous, because any one node outage would cause the whole system to grind to a halt.
```

* Can you tell me how followers work in practical scenario? 
```
In practice, if you enable synchronous replication on a database, it usually means that one of the followers is synchronous, and the others are asynchronous. 

If the synchronous follower becomes unavailable or slow, one of the asynchronous followers is made synchronous which guarantees that you have an up-to-date copy of the data on at least two nodes: the leader and one synchronous follower. 

And this configuraiton is sometimes also called semi-synchronous. 
```
* sync and async
```
total-sync: impractical, one follower outage will cause whole system halt.

semi-sync: if we set this option in system, one of the followers will be set to work synchronously when handling request from leader, and other followers will work asynchronously. 

total-async: even though not recoverable if leader failes, writes replicated to the followers will be lost. But the leader can continue processing writes, even if all of its followers have fallen behind.
```

* OK here comes to another question, cause we know that leader-based replciation mode almost all followers are configured work asynchronously in practical, how we deal with the circumstances: All of the leader's followers have fallen behind? 

```
To solve this problem, the engineers provide a term called 'replicaiton lag' which used to describe the time delay between a write happening on the leader and being refelected on a follower.

Replication lag may be only a fraction of a second. 
However, if the system is operating near capacity or if there is network issues, the replication lag can easily increase to several seconds or even minutes. 

At base of replication lag there are some approaches to solving these problems. 

```




