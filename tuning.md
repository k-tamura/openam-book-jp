[TODO] ※このページは作成中です。

This chapter covers key OpenAM tunings to ensure smoothly performing access and federation management services, and to maximize throughput while minimizing response times.

Note
The recommendations provided here are guidelines for your testing rather than hard and fast rules for every situation. Said another way, the fact that a given setting is configurable implies that no one setting is right in all circumstances.

The extent to which performance tuning advice applies depends to a large extent on your requirements, on your workload, and on what resources you have available. Test suggestions before rolling them out into production.

The suggestions in this chapter pertain to OpenAM deployments with the following characteristics:

- The host running the OpenAM server has a large amount of memory.
- The deployment has a dedicated OpenDJ directory server for the Core Token Service. The host running this directory server is a high-end server with a large amount of memory and multiple CPUs.
- The OpenAM server is configured to use stateful sessions.

As a rule of thumb, an OpenAM server in production with a 3 GB heap configured to use stateful sessions can handle 100,000 sessions. Although you might be tempted to use a larger heap with a 64-bit JVM, smaller heaps are easier to manage. Thus, rather than scaling single servers up to increase the total number of simultaneous sessions, consider scaling out by adding more servers instead.

### OpenAM Server Settings

OpenAM has a number of settings that can be tuned to increase performance.
#### General Settings

The following general points apply:

    Set debug level to error.

    Set container-level logging to a low level, such as error or severe.

25.1.2. LDAP Settings

Tune your LDAP data stores, your LDAP authentication modules, and connection pools for CTS and configuration stores.
25.1.2.1. Tuning LDAP Data Store Settings

To change LDAP data store settings, browse to Realms > Realm Name > Data Stores > Data Store Name in the OpenAM console. Each data store has its own connection pool and therefore each data store needs its own tuning:
