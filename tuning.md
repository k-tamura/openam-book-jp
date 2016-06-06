[TODO] ※このページは作成中です。

この章では、OpenAMの動作をスムーズにし、応答時間を最小限に抑えつつ、スループットを最大化するための、キーとなるチューニング項目について説明します。

> 注意  
> ここで提供される推奨事項が、すべての状況で正しい設定ではないということに注意して下さい。

パフォーマンスチューニングのアドバイスが適用される範囲は、システム要件、ワークロード、どの程度のリソースが利用可能であるかに大きく依存します。本番環境にOpenAMをデプロイする前に十分なテストを行って下さい。

この章の提案は、次の特性を持つOpenAMの配備に関連します:

- OpenAMサーバーを実行しているホストは、大量のメモリを持っている。
- 配備がコアトークンサービスのための専用OpenDJディレクトリサーバーを持っている。このディレクトリサーバーを実行しているホストは、大容量のメモリと、複数のCPUを搭載するハイエンドサーバーである。
- OpenAMサーバーはステートフルなセッションを使用するように設定されています。

これまでの経験則から、本番環境においてステートフルセッションを使用するように構成された3GBのヒープのOpenAMサーバーは、10万セッションを処理することができます。64ビットJVMで大規模なヒープを使用した方がいいと考えるかもしれませんが、小さいヒープの方が管理が容易です。このように、同時セッションの合計数を増やすためには、単一のサーバーをスケールアップするよりも、むしろ、代わりにより多くのサーバーを追加することにより、スケールアウトすることを検討して下さい。

### OpenAMサーバー設定

OpenAMには、パフォーマンスを向上させるために調整することができるいくつかの設定があります。

#### 一般設定

一般的なポイントが以下があげられます。

- デバッグレベルをエラーに設定する。
- コンテナレベルのロギングを、エラーや重大などの低いレベルに設定する。

#### LDAP 設定

LDAPデータストア、LDAP認証モジュール、CTSおよび設定ストアの接続プールをチューニングします。

##### LDAP データストア設定

LDAPデータストアの設定を変更するには、OpenAMの管理コンソールの レルム > レルム名 > データストア > データストア名 にアクセスします。 データストア毎に独自の接続プールがあるので、データストア毎に独自のチューニングを必要とします:

表. LDAP データストア設定

|プロパティ|デフォルト値|提案|
|---|---|---|
|LDAP接続プール最小サイズ|1|LDAP接続プールの最小サイズ。推奨値は、10です。  (sun-idrepo-ldapv3-config-connection_pool_min_size)|
|LDAP接続プール最大サイズ|10|LDAP接続プールの最大サイズ。推奨値は、65です。LDAPサーバーがクライアントの最大数に対応できることを確認してください。  (sun-idrepo-ldapv3-config-connection_pool_max_size)|

##### LDAP認証モジュール設定のチューニング

LDAP認証モジュールの接続プールの設定を変更するには、OpenAMの管理コンソールの 設定 > 認証 > コア にアクセスします:

表. LDAP認証モジュール設定

|プロパティ|デフォルト値|提案|
|---|---|---|
|デフォルトLDAP接続プールサイズ|LDAP認証モジュールで使用されるLDAP接続プールの最小数と最大数。本番環境での推奨値は、10:65です。  (iplanet-am-auth-ldap-connection-pool-default-size)|

##### LDAP CTSおよび設定ストアの設定のチューニング

When tuning LDAP connection pool settings for the Core Token Service (CTS), what you change depends on whether the directory service backing the CTS is the same directory service backing OpenAM configuration.

When the same directory service backs both the CTS and also OpenAM configuration (the default), then the same connection pool is shared for any LDAP operations requested by the CTS or by a service accessing the OpenAM configuration. In this case, one connection is reserved for cleanup of expired CTS tokens. Roughly half of the connections are allocated for CTS operations, to the nearest power of two.[9] The remaining connections are allocated to services accessing the OpenAM configuration. For a default configuration, where the maximum number of connections in the pool is ten, one connection is allocated for cleanup of expired CTS tokens, four connections are allocated for other CTS operations, and five connections are allocated for services accessing the configuration. If the Maximum Connection Pool size is 20, one connection is allocated for cleanup of expired CTS tokens, eight connections are allocated for other CTS operations, and 11 connections are allocated for services accessing the configuration. If the pool size is 65, then the numbers are 1, 32, and 32, and so on.

The minimum number of connections is 6.

When the directory service backing the CTS is external (differs from the directory service backing the OpenAM configuration) then the connection pool used to access the directory service for the CTS is separate from the pool used to access the directory service for the OpenAM configuration. One connection is reserved for cleanup of expired CTS tokens. Remaining connections are allocated for CTS operations such that the number of connections allocated is equal to a power of two. In this case, set the maximum number of connections to 2^n+1, as in 9, 17, 33, 65, and so forth.

If the same directory service backs both the CTS and also OpenAM configuration, then set pool sizes under Configuration > Servers and Sites > Server Name > Directory Configuration.

If the directory service backing the CTS is external (differs from the directory service backing the OpenAM configuration), then set the maximum connection pool size under Configuration > Servers and Sites > Server Name > CTS > External Store Configuration.

In both cases, if you must change the default connection timeouts, set the advanced properties described below under Configuration > Servers and Sites > Server Name > Advanced:

表. CTSストアLDAP接続プールの設定

|プロパティ|デフォルト値|提案|
|---|---|---|
|Maximum Connection Pool|10|Find this setting in OpenAM console under Configuration > Servers and Sites > Server Name > Directory Configuration.  When the same directory service backs both the CTS and also OpenAM configuration, consider increasing this to at least 19 to allow 9 connections for the CTS, and 10 connections for access to the OpenAM configuration (including for example looking up policies).|
|Max Connections|10|Find this setting in OpenAM console under Configuration > Servers and Sites > Server Name > CTS > External Store Configuration.  When the directory service backing the CTS is external and the load on the CTS is high, consider setting this to 2^n+1, where n = 4, 5, 6, and so on. In other words, try setting this to 17, 33, 65, and so on when testing performance under load.  (org-forgerock-services-cts-store-max-connections)|
|CTS connection timeout (advanced property)|10 (seconds)|Most CTS requests to the directory server are handled quickly, so the default timeout is fine for most cases.  If you choose to vary this setting for performance testing, set the advanced property org.forgerock.services.datalayer.connection.timeout.cts.async, under Configuration > Servers and Sites > Server Name > Advanced.  You must restart OpenAM or the container in which it runs for changes to take effect.|
|CTS reaper timeout (advanced property)|None|The CTS token cleanup connection generally should not time out as it is used to request long-running queries that can return many results.  If you choose to vary this setting for performance testing, set the advanced property, org.forgerock.services.datalayer.connection.timeout.cts.reaper, to the number of seconds desired under Configuration > Servers and Sites > Server Name > Advanced.  You must restart OpenAM or the container in which it runs for changes to take effect.|
|Configuration management connection timeout (advanced property)|10 (seconds)|Most configuration management requests to the directory server are handled quickly, so the default timeout is fine for most cases.  If you choose to vary this setting for performance testing, set the advanced property, org.forgerock.services.datalayer.connection.timeout, under Configuration > Servers and Sites > Server Name > Advanced.|

You must restart OpenAM or the container in which it runs for changes to take effect.

#### 通知設定

OpenAM has two thread pools used to send notifications to clients. The Service Management Service (SMS) thread pool can be tuned in OpenAM console under Configuration > Servers and Sites > Default Server Settings > SDK:

表. SMS通知設定

|プロパティ|デフォルト値|提案|
|---|---|---|
|Notification Pool Size|10|This is the size of the thread pool used to send notifications. In production this value should be fine unless lots of clients are registering for SMS notifications.  (com.sun.identity.sm.notification.threadpool.size)|

The session service has its own thread pool to send notifications to listeners about changes to stateful sessions. This is configured under Configuration > Servers and Sites > Default Server Settings > Session:

表. Session Service Notification Settings

|プロパティ|デフォルト値|提案|
|---|---|---|
|Notification Pool Size|10|This is the size of the thread pool used to send notifications. In production this should be around 25-30.  (com.iplanet.am.notification.threadpool.size)|
|Notification Thread Pool Threshold|5000|This is the maximum number of notifications in the queue waiting to be sent. The default value should be fine in the majority of installations.  (com.iplanet.am.notification.threadpool.threshold)|

####  セッション設定

The session service has additional properties to tune, which are configured under Configuration > Servers and Sites > Default Server Settings > Session. The following suggestions apply to deployments using stateful sessions:

表. スコープ設定

|プロパティ|デフォルト値|提案|
|---|---|---|
|Maximum Sessions|5000|In production, this value can safely be set into the 100,000s. The maximum session limit is really controlled by the maximum size of the JVM heap which must be tuned appropriately to match the expected number of concurrent sessions.  (com.iplanet.am.session.maxSessions)|
|Sessions Purge Delay|0|This should be zero to ensure sessions are purged immediately.  (com.iplanet.am.session.purgedelay)|

###  Java仮想マシン設定

This section gives some initial guidance on configuring the JVM for running OpenAM. These settings provide a strong foundation to the JVM before a more detailed garbage collection tuning exercise, or as best practice configuration for production:

表. ヒープサイズ設定

|JVMパラメータ|推奨値|説明|
|---|---|---|
|-Xms & -Xmx|At least 1024 MB (2048 MB with embedded OpenDJ), in production environments at least 2048 MB to 3072 MB. This setting depends on the available physical memory, and on whether a 32- or 64-bit JVM is used.|-|
|-server|-|Ensures the server JVM is used|
|-XX:PermSize & -XX:MaxPermSize|Set both to 256 MB|Controls the size of the permanent generation in the JVM|
|-Dsun.net.client.defaultReadTimeout|60000|Controls the read timeout in the Java HTTP client implementation. This applies only to the Sun/Oracle HotSpot JVM.|
|-Dsun.net.client.defaultConnectTimeout|High setting: 30000 (30 seconds)|Controls the connect timeout in the Java HTTP client implementation.  When you have hundreds of incoming requests per second, reduce this value to avoid a huge connection queue.  This applies only to the Sun/Oracle HotSpot JVM.|

表. セキュリティ設定

|JVMパラメータ|推奨値|説明|
|---|---|---|
|-Dhttps.protocols|TLSv1,TLSv1.1,TLSv1.2 (for JDK 7, JDK 8)  TLSv1 (for JDK 6)|Controls the protocols used for outbound HTTPS connections from OpenAM.|

This applies only to Sun/Oracle Java environments.

ガベージコレクション設定

|JVMパラメータ|推奨値|説明|
|---|---|---|
|-verbose:gc|-|Verbose garbage collection reporting|
|-Xloggc:|$CATALINA_HOME/logs/gc.log|Location of the verbose garbage collection log file|
|-XX:+PrintClassHistogram|-|Prints a heap histogram when a SIGTERM signal is received by the JVM|
|-XX:+PrintGCDetails|-|Prints detailed information about garbage collection|
|-XX:+PrintGCTimeStamps|-|Prints detailed garbage collection timings|
|-XX:+HeapDumpOnOutOfMemoryError|-|Out of Memory errors generate a heap dump automatically|
|-XX:HeapDumpPath|$CATALINA_HOME/logs/heapdump.hprof|Location of the heap dump|
|-XX:+UseConcMarkSweepGC|-|Use the concurrent mark sweep garbage collector|
|-XX:+UseCMSCompactAtFullCollection|-|Aggressive compaction at full collection|
|-XX:+CMSClassUnloadingEnabled|-|Allow class unloading during CMS sweeps|

### OpenAMでのキャッシング

OpenAM caches data to avoid having to query user and configuration data stores each time it needs the information. By default, OpenAM makes use of LDAP persistent search to receive notification of changes to cached data. For this reason, caching works best when data are stored in a directory server that supports LDAP persistent search.

OpenAM has two kinds of cache on the server side that you can configure, one for configuration data and the other for user data. Generally use the default settings for configuration data cache. This section mainly covers the configuration choices you have for caching user data.

OpenAM implements the global user data cache for its user data stores. Prior to OpenAM 11.0, OpenAM supported a secondary Time-to-Live (TTL) data store caching layer, which has since been removed in OpenAM 11.0 and later versions.

The user data store also supports a DN Cache, used to cache DN lookups that tend to occur in bursts during authentication. The DN Cache can become out of date when a user is moved or renamed in the underlying LDAP store, events that are not always reflected in a persistent search result. You can enable the DN cache when the underlying LDAP store supports persistent search and mod DN operations (that is, move or rename DN).

The following diagram depicts the two kinds of cache, and also the two types of caching available for user data:

Figure 25.1. OpenAM Caches
Caches in OpenAM server

The rest of this section concerns mainly settings for global user data cache and for SDK clients. For a look at data store cache settings, see Table 25.1, "LDAP Data Store Settings".

#### 全体的なサーバーのキャッシュ設定

By default OpenAM has caching enabled both for configuration data and also for user data. This setting is governed by the server property com.iplanet.am.sdk.caching.enabled, which by default is true. When you set this advanced property to false, then you can enable caching independently for configuration data and for user data.

手順. グローバルユーザーデータ・キャッシングをオフにする

Disabling caching can have a severe negative impact on performance. This is because when caching is disabled, OpenAM must query a data store each time it needs data.

If, however, you have at least one user data store that does not support LDAP persistent search, such as a relational database or an LDAP directory server that does not support persistent search, then you must disable the global cache for user data. Otherwise user data caches cannot stay in sync with changes to user data entries:

1. In the OpenAM console, browse to Configuration > Servers and Sites > Server Name > Advanced.
2. Set com.iplanet.am.sdk.caching.enabled to false to disable caching overall.
3. Set com.sun.identity.sm.cache.enabled to true to enable configuration data caching.  
    All supported configuration data stores support LDAP persistent search, so it is safe to enable configuration data caching.  
    You must explicitly set this property to true, because setting com.iplanet.am.sdk.caching.enabled to false in the previous step disables both user and configuration data caching.
4. Save your work.
5. OpenAM starts persistent searches on user data stores when possible [10] in order to monitor changes. With user data store caching disabled, OpenAM still starts the persistent searches, even though it no longer uses the results.  
    Therefore, if you disable user data store caching, you should also disable persistent searches on user data stores in your deployment to improve performance. To disable persistent search on a user data store, remove the value of the Persistent Search Base DN configuration property and leave it blank. Locate this property under Realms > Realm Name > Data Stores > Data Store Name > Persistent Search Controls. 

手順. グローバルユーザーデータ・キャッシングの最大サイズを変更する

With a large user data store and active user base, the number of user entries in cache can grow large:

1. In the OpenAM console, browse to Configuration > Servers and Sites > Default Server Settings > SDK.
2. Change the value of SDK Caching Maximum Size, and then click Save.  
    There is no corresponding setting for configuration data, as the number of configuration entries in a large deployment is not likely to grow nearly as large as the number of user entries.

#### Java EEのポリシーエージェントおよびSDKクライアントのキャッシュプロパティ

Policy agents and other OpenAM SDK clients can also cache user data, using most of the same properties as OpenAM server as described in Table 25.10, "OpenAM Cache Properties" . Clients, however, can receive updates by notification from OpenAM or, if notification fails, by polling OpenAM for changes.

手順. クライアントキャッシュアップデートの通知とポーリングを有効にする

This procedure describes how to enable change notification and polling for policy agent user data cache updates. When configuring a custom OpenAM SDK client using a .properties file, use the same properties as for the policy agent configuration:

1. In OpenAM console, browse to Realms > Realm Name > Agents > Agent Type > Agent Name to view and edit the policy agent profile.
2. On the Global tab page, check that the Agent Notification URL is set.  
    When notification is enabled, the agent registers a notification listener with OpenAM for this URL.  
    The corresponding property is com.sun.identity.client.notification.url.
3. For any changes you make, Save your work.  
    You must restart the policy agent for the changes to take effect.

#### キャッシュ設定

The table below provides a quick reference, primarily for user data cache settings.

Notice that many properties for configuration data cache have sm (for Service Management) in their names, whereas those for user data have idm (for Identity Management) in their names:

表. OpenAMキャッシュプロパティ

|プロパティ|説明|デフォルト|適用対象|
|---|---|---|---|
|com.iplanet.am.sdk.cache.maxSize|Maximum number of user entries cached.|10000|Server and SDK|
|com.iplanet.am.sdk.caching.enabled|Whether to enable caching for both configuration data and also for user data. If true, this setting overrides com.sun.identity.idm.cache.enabled and com.sun.identity.sm.cache.enabled. If false, you can enable caching independently for configuration data and for user data using the aforementioned properties.|true|Server & SDK|
|com.iplanet.am.sdk.remote.pollingTime|How often in minutes the SDK client, such as a policy agent should poll OpenAM for modified user data entries.The SDK also uses this value to determine the age of the oldest changes requested. The oldest changes requested are 2 minutes older than this setting. In other words, by default the SDK polls for entries changed in the last 3 minutes. Set this to 0 or a negative integer to disable polling.|1 (minute)|SDK|
|com.sun.am.event.notification.expire.time|How long OpenAM stores a given change to a cached entry, so that clients polling for changes do not miss the change.|30 (minutes)|Server only|
|com.sun.identity.idm.cache.enabled|If com.iplanet.am.sdk.caching.enabled is true, this property is ignored. Otherwise, set this to true to enable caching of user data.|false|Server & SDK|
|com.sun.identity.idm.cache.entry.default.expire.time|How many minutes to store a user data entry in the global user data cache.|30 (minutes)|Server & SDK|
|com.sun.identity.idm.cache.entry.expire.enabled|Whether user data entries in the global user data cache should expire over time.|false|Server & SDK|
|com.sun.identity.idm.remote.notification.enabled|Whether the SDK client, such as a policy agent should register a notification listener for user data changes with the OpenAM server. The SDK client uses the URL specified by com.sun.identity.client.notification.url to register the listener so that OpenAM knows where to send notifications. If notifications cannot be enabled for some reason, then the SDK client falls back to polling for changes.|true|SDK|
|com.sun.identity.sm.cache.enabled|If com.iplanet.am.sdk.caching.enabled is true, this property is ignored. Otherwise, set this to true to enable caching of configuration data. It is recommended that you always set this to true.|false|Server & SDK|
|sun-idrepo-ldapv3-dncache-enabled|Set this to true to enable DN caching of user data.|false|Server & SDK|
|sun-idrepo-ldapv3-dncache-size|Sets the cache size.|1500|Server & SDK|


[9] To be precise, the number of connections allocated for CTS operations is equal to the power of two that is nearest to half the maximum number of connections in the pool.

[10] OpenAM starts persistent searches on user data stores on directory servers that support the psearch control. 
