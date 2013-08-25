Architecture
==================

Relative To: **Milestone-01**

# Components

The presented architecture can be described as a [3-tier](http://en.wikipedia.org/wiki/Multitier_architecture) domain based model. The goal of which is to allow for differnt technology and hardware based clients to connect to the server REST API and fetch/modify data.

Note, that **availability** and **backup** scenarios are currently **not covered**!

![alt text](https://raw.github.com/petarov/e-additives.server/master/docs/eadditives_architecture.png "Components")

# Technology Stack

Performance is a function of many factors - hardware specs, components configuration, ISP stabilty, etc. The technology stack presented here aims to achieve relatively fast performance given the API requirements and our available hosting hardware.

![alt text](https://raw.github.com/petarov/e-additives.server/master/docs/eadditives_components_stack.png "Deployment Stack")

Technology | Comments
------|------------
**Linux** | No _Windowses_ allowed.
**Apache 2.2** | Apache running in [prefork](http://httpd.apache.org/docs/2.2/mod/prefork.html) mode.
**PHP 5.3** | A list of required extensions is available in the [README.md](../README.md)
**Redis 2.2** | [Redis](http://redis.io/) is configured as [cache-only](http://redis.io/topics/config). There is **no persistance** enabled. We are only interested in temporary caching database fetched items and using them on client READ requests.
**MySQL 5.5** | We will be using the ACID compliant [InnoDB](http://dev.mysql.com/doc/refman/5.5/en/innodb-storage-engine.html) engine.


# Database Schema

![alt text](https://raw.github.com/petarov/e-additives.server/master/docs/eadditives_database.png "Database Schema")

Database schema is relatively simple. There are currently 3 tables:

Table | Description
------|------------
**Additive** | Holds info about additive code and last updates.
**AdditiveProp** | Currently the longest table. Holds **key/value** additive properties. Each property is assigned to an **Additive** and **Locale**
**Locale** | Available service localizations. Locales can be disabled which means they will be temporary invisible for clients. There is a database constraint that will NOT allow a locale to be deleted until additive properties for that locale still exist.
