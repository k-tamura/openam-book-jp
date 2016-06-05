[TODO] ※このページは作成中です。

この章では、OpenAMの動作をスムーズにし、応答時間を最小限に抑えつつ、スループットを最大化するための、キーとなるチューニング項目について説明します。

Note
The recommendations provided here are guidelines for your testing rather than hard and fast rules for every situation. Said another way, the fact that a given setting is configurable implies that no one setting is right in all circumstances.

The extent to which performance tuning advice applies depends to a large extent on your requirements, on your workload, and on what resources you have available. Test suggestions before rolling them out into production.

The suggestions in this chapter pertain to OpenAM deployments with the following characteristics:

- The host running the OpenAM server has a large amount of memory.
- The deployment has a dedicated OpenDJ directory server for the Core Token Service. The host running this directory server is a high-end server with a large amount of memory and multiple CPUs.
- The OpenAM server is configured to use stateful sessions.

As a rule of thumb, an OpenAM server in production with a 3 GB heap configured to use stateful sessions can handle 100,000 sessions. Although you might be tempted to use a larger heap with a 64-bit JVM, smaller heaps are easier to manage. Thus, rather than scaling single servers up to increase the total number of simultaneous sessions, consider scaling out by adding more servers instead.

### OpenAMサーバー設定

OpenAMには、パフォーマンスを向上させるために調整することができるいくつかの設定があります。

#### 一般設定

一般的なポイントが以下があげられます。

- デバッグレベルをエラーに設定する。
- コンテナレベルのロギングを、エラーや重大などの低いレベルに設定する。

#### LDAP 設定

LDAPデータストア、LDAP認証モジュール、CTSおよび設定ストアの接続プールをチューニングします。

##### LDAP データストア設定

LDAPデータストアの設定を変更するには、管理コンソールの レルム > レルム名 > データストア > データストア名 にアクセスします。 データストア毎に独自の接続プールがあるので、データストア毎に独自のチューニングを必要とします:

|プロパティ|デフォルト値|提案|
|---|---|---|
|LDAP接続プール最小サイズ|1|LDAP接続プールの最小サイズ。推奨値は、10です。  (sun-idrepo-ldapv3-config-connection_pool_min_size)
|LDAP接続プール最大サイズ|10|LDAP接続プールの最大サイズ。推奨値は、65です。LDAPサーバーがクライアントの最大数に対応できることを確認してください。  (sun-idrepo-ldapv3-config-connection_pool_max_size)|


