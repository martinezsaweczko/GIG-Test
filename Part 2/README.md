# Technical Proposal

## Introduction

During a recent data purging exercise on a PostgreSQL RDS instance, the team hit an IOPS limit of 5,000, causing application degradation

## Tuning and Optimization

Before starting to share any tuning investigation lets evaluate where the problem comes from:

*Application is running a purging exercise*

Possible solutions:

1. How is the application running this purging exercise? Are they deleting all records in a row? Can we ask them to change the strategy to relax the purging? Like delete X number of rows, wait, delete X number of rows, wait. It would be nice to have a parametrized procedure able to do that where we can control how many rows we can delete at a time. We can increase that number until we find a good balance beween performance and purging time and stability.

Why this helps? It spreads IO over time.

2.- When lof of records are updated/insert/deleted, any RDBMS needs to write on disk in order to be sure if any crash any data is recovered. This is the *D* of ACID. When there is a massive delete, that file that PostgreSQL needs to write on disk (WAL). If we increase that setting (file), we will make the system to syns with the disk less ofthen reducing the IO load.

The big trade off of chaning this setting, in case of recovery after crash, it could take more time to recover.

```
max_wal_size = X GB
```

3.- Another option would be to change the table(s) definition to use partitioned table by the field that is being used for purging. This way, when the application needs to purge, instead of deleting records, it can just drop the partition, which is a metadata operation and does not require writing on disk for each deleted record. This would significantly reduce the IO load during purging.

4.- As a last resource, and only if business side allows us to loose some data in case of crash, we could change the setting to not wait for the disk to syns. This would reduce IO load significantly but we would be risking data loss in case of crash. We could run our batch with this settings and in case of any crash we could run the batch again, but we would need to make sure that the batch is idempotent to avoid any data inconsistency.


Local connection
```
set synchronous_commit = off

DELETE FROM events WHERE created_at < now() - interval '90 days';
```

Transaction level
```
BEGIN;

SET LOCAL synchronous_commit = off;

DELETE FROM events WHERE created_at < now() - interval '90 days';

COMMIT;
```

### Summary:
Short term solution: Change the purging strategy to delete in batches and increase max_wal_size to reduce IO load during purging.
Long term solution: Consider partitioning the table(s) being purged to allow for faster purging by dropping partitions instead of deleting records.


## Scaling

Let me focus on the difference between RDS Proxy and basic connection pool at app side ( I want to remove pgbouncer for the equation as I dont feel condident enough to talk about it)

Under my opinion RDS proxy main goal is to "defend" your database. I mean, one of the most underated features is to multiplext your connection from application side. It means, that maybe you can have thousands of connections from your application, but eventually at your RDBMS you will only see a very few of them. Furthermore RDS Proxy brings other interesting feature as failover, split read/write at layer 4.

In the other side, connection pooling at application side resolves the problem of how many connections are going to be needed at the same time by the application. Connection pooling helps you to have ready connection in case any thread from the application wants to use.

So, when to use which:

Connection pooling:
* Is your database already ably to manage those 10000 connections?  Yes (In case this could be a problem we can multiple with pgbouncer)
* Do you have in place a failover strategy in case of database failure? Yes
* You dont need split read/write path? No

RDS Proxy:
* Your database cannot manage a high number of connections or you want to protect agains any connection storm? Yes
* You want to have a failover strategy in place? Yes
* You want to split read/write path at layer 4? Yes



