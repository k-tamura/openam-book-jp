### OpenAMサーバーの設定

OpenAMには、パフォーマンスを向上させるために調整できるいくつかの設定があります。

#### 一般設定

一般的なポイントとして以下が挙げられます。

- デバッグレベルをエラーに設定する。
- コンテナレベルのロギングを、エラーや重大などの低いレベルに設定する。

#### LDAP 設定

LDAPデータストア、LDAP認証モジュール、CTSおよび設定ストアの接続プールをチューニングします。

##### LDAP データストア設定

LDAPデータストアの設定を変更するには、OpenAMの管理コンソールの レルム > レルム名 > データストア > データストア名 にアクセスします。 データストア毎に独自の接続プールがあるので、データストア毎に独自のチューニングが必要です:

表. LDAP データストア設定

|プロパティ|デフォルト値|提案|
|---|---|---|
|LDAP接続プール最小サイズ|1|LDAP接続プールの最小サイズ。推奨値は10です。  (sun-idrepo-ldapv3-config-connection_pool_min_size)|
|LDAP接続プール最大サイズ|10|LDAP接続プールの最大サイズ。推奨値は65です。  LDAPサーバーがクライアントの最大数に対応できることを確認してください。  (sun-idrepo-ldapv3-config-connection_pool_max_size)|

##### LDAP認証モジュールの設定のチューニング

LDAP認証モジュールの接続プールの設定を変更するには、OpenAMの管理コンソールの 設定 > 認証 > コア にアクセスします:

表. LDAP認証モジュール設定

|プロパティ|デフォルト値|提案|
|---|---|---|
|デフォルトLDAP接続プールサイズ|1:10|LDAP認証モジュールで使用されるLDAP接続プールの最小数と最大数。本番環境での推奨値は10:65です。  (iplanet-am-auth-ldap-connection-pool-default-size)|

##### CTSおよび設定ストアの設定のチューニング

コアトークンサービス(CTS)のLDAP接続プールの設定をチューニングする場合、CTSをバックアップしているディレクトリサービスと、OpenAMの設定をバックアップしているディレクトリサービスが同じであるかどうかによって、変更する内容が異なります。

両者が同じである場合(デフォルトの場合)、要求された任意のLDAP操作に対して、OpenAMの設定にアクセスするサービスとCTSは、同じ接続プールを共有します。この場合、有効期限切れのCTSトークンのクリーンアップのために、コネクションが一つ予約されます。2の累乗に最も近い約半分のコネクションが、CTSの操作のために割り当てられます(※)。残りのコネクションは、OpenAMの設定にアクセスするサービスに割り当てられます。デフォルトの設定では、プール内のコネクションの最大数は10で、1つのコネクションが有効期限切れのCTSトークンのクリーンアップに、4つのコネクションが他のCTSの操作に、5つのコネクションが設定にアクセスするサービスに割り当てられます。プール内のコネクションの最大数が20の場合、1つのコネクションが有効期限切れのCTSトークンのクリーンアップに、8つのコネクションが他のCTSの操作に、11つのコネクションが設定にアクセスするサービスに割り当てられます。65の場合は、それぞれ1、32、32となります。

※ 正確には、CTSの操作のために割り当てられたコネクションの数は、プール内のコネクションの半分の最大数に最も近い2の累乗に等しいです。

最小コネクション数は6です。

CTSをバックアップするディレクトリサービスが外部にある場合(OpenAMの設定をバックアップするディレクトリサービスとは異なる場合)、CTS用のディレクトリサービスにアクセスするために使用される接続プールは、OpenAMの設定データ用のディレクトリサービスにアクセスするために使用されるプールとは、異なります。一つのコネクションが有効期限が切れたCTSトークンのクリーンアップのために予約されています。残りのコネクションは、割り当てられたコネクションの数が2の累乗に等しくなるように、CTS操作に割り当てられます。この場合、9、17、33、65のように2^n+1の最大コネクション数を設定します。

同じディレクトリサービスがCTSとOpenAMの設定の両方をバックアップする場合、 設定 > サーバーおよびサイト > サーバー名 > ディレクトリ設定 のプールサイズを設定します。

CTSをバックアップしているディレクトリサービスが外部にある(OpenAMの設定をバックアップしているディレクトリサービスとは異なる)場合は、設定 > サーバーおよびサイト > サーバー名 > CTS > 外部ストア設定 の最大接続プールサイズを設定します。

両方のケースで、デフォルトの接続タイムアウトを変更する必要がある場合、設定 > サーバーおよびサイト > サーバー名 > 高度 の、次に説明する詳細なプロパティを設定します:

表. CTSストアLDAP接続プールの設定

|プロパティ|デフォルト値|提案|
|---|---|---|
|最大接続プール|10|管理コンソールで 設定 > サーバーおよびサイト > サーバー名 > ディレクトリの設定 をクリックします。同じディレクトリサービスがOpenAMの設定とCTSの両方をバックアップする場合は、CTSのための9個のコネクションと、OpenAMの設定にアクセスするための10個のコネクション(例えば、ポリシーの検索などを含む)を可能にするために、少なくともこれを19に増やすことを検討してください。|
|最大接続数|10|管理コンソールの 設定 > サーバーおよびサイト > サーバー名 > CTS > 外部ストア設定 で、この設定を検索します。 CTSのバックアップディレクトリサービスが外部にあり、CTSの負荷が高い場合、この設定を2^n+1にすることを検討してください(n = 4, 5, 6, など)。つまり、高負荷の下でパフォーマンスをテストする際に、17、33、65のような値にこれを設定してみてください。 (org.forgerock.services.cts.store.max.connections)|
|CTS 接続タイムアウト (高度なプロパティ)|10 (秒)|ディレクトリサーバへのほとんどのCTSリクエストは速やかに処理されるので、ほとんどの場合、デフォルトのタイムアウトで問題ありません。性能テストのために、この設定を変更する場合、設定 > サーバーおよびサイト > サーバー名 > 高度 で、高度なプロパティorg.forgerock.services.datalayer.connection.timeout.cts.asyncを設定します。 変更を有効にするには、OpenAMが実行されるコンテナを再起動する必要があります。|
|CTS リーパータイムアウト (高度なプロパティ)|無し|CTSトークンのクリーンアップコネクションは、多くの結果を返す可能性がある時間のかかるクエリを要求するために使用されるので、一般的にタイムアウトしないようにしてください。パフォーマンステストのために、この設定を変更する場合、設定 > サーバーおよびサイト > サーバー名 > 高度 で、高度なプロパティorg.forgerock.services.datalayer.connection.timeout.cts.reaperに希望の秒数を設定します。 変更を有効にするには、OpenAMが実行されるコンテナを再起動する必要があります。|
|設定管理接続タイムアウト (高度なプロパティ)|10 (秒)|ディレクトリサーバーへのほとんどの設定管理要求は速やかに処理されるので、ほとんどの場合、デフォルトのタイムアウトで問題ありません。パフォーマンステストのために、この設定を変更する場合、設定 > サーバーおよびサイト > サーバー名 > 高度 で、高度なプロパティorg.forgerock.services.datalayer.connection.timeoutを設定します。|

変更を有効にするには、OpenAMを再起動する必要があります。

#### 通知設定

OpenAMは、クライアントに通知を送信するために使用される2つのスレッドプールを持っています。サービス管理サービス（SMS）スレッドプールは、OpenAMコンソールの 設定 > サーバーおよびサイト > デフォルトサーバー設定 > SDK 以下で調整することができます:

表. SMSの通知設定

|プロパティ|デフォルト値|提案|
|---|---|---|
|通知プールサイズ|10|通知を送信するために使用されるスレッドプールのサイズです。本番環境では、SMS通知のために多数のクライアントが登録されていない限り、デフォルト値のままで問題ないはずです。  (com.sun.identity.sm.notification.threadpool.size)|

セッションサービスは、ステートフルセッションへの変更についてリスナーに通知を送信するための独自のスレッドプールも持っています。これについては、設定 > サーバーおよびサイト > デフォルトサーバー設定 > セッション 以下に設定されています:

表. セッションサービスの通知設定

|プロパティ|デフォルト値|提案|
|---|---|---|
|通知プールサイズ|10|通知を送信するために使用されるスレッドプールのサイズです。本番環境では、25〜30程度にする必要があります。 (com.iplanet.am.notification.threadpool.size)|
|通知スレッドプールのしきい値|5000|送信されるのを待っているキュー内の通知の最大数です。一般的な構成においては、デフォルト値で問題ないはずです。 (com.iplanet.am.notification.threadpool.threshold)|

#### セッションの設定

セッションサービスには、チューニングのための追加のプロパティがあります(設定 > サーバーおよびサイト > デフォルトサーバー設定 > セッション 以下に設定されています)。次の提案は、ステートフルセッションを使用した配備に適用されます:

表. セッションの設定

|プロパティ|デフォルト値|提案|
|---|---|---|
|最大セッション数|5000|本番環境では、この値を100,000に設定することができます。最大セッションの制限は、JVMのヒープの最大サイズによって制御されます(実際には同時セッション数の期待値と一致するように適切に調整されなければなりません)。 (com.iplanet.am.session.maxSessions)|
|セッションのパージ遅延|0|セッションがすぐにパージされることを保証するためにはゼロにする必要があります。 (com.iplanet.am.session.purgedelay)|
