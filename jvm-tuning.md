### Java仮想マシンの設定

このセクションでは、OpenAMを実行する際のJVMオプションの設定について解説します。これらの設定は、ガベージコレクションのチューニング作業を行う前の、本番環境用のベストプラクティスな設定として、利用できます:

表. ヒープサイズなどの設定

|JVMパラメータ|推奨値|説明|
|---|---|---|
|-Xmsと-Xmx|少なくとも1024MB(組み込みOpenDJを使用する場合は2048MB)、本番環境では少なくとも2048MB～3072MB。この設定は、使用可能な物理メモリや、32ビットまたは64ビットのJVMが使用されているかどうかに依存する。|-|
|-server|-|サーバーモードのJVMが使用されることを保証する。|
|-XX:PermSizeと-XX:MaxPermSize (JDK 6、JDK 7)、-XX:MetaspaceSizeと-XX:MaxMetaspaceSize (JDK 8)|両方とも256MBに設定。|JVM内のパーマネント領域のサイズを制御する。|
|-Dsun.net.client.defaultReadTimeout|60000|JavaのHTTPクライアントの実装における読み取りタイムアウトを制御します。  この設定は、Sun/Oracle HotSpot JVMに適用されます。|
|-Dsun.net.client.defaultConnectTimeout|高い値: 30000 (30秒)|JavaのHTTPクライアントの実装におけるコネクションタイムアウトを制御します。   秒間数百のリクエストを受信する場合は、巨大なコネクションキューになることを避けるために、この値を減らします。  この設定は、Sun/Oracle HotSpot JVMに適用されます。|

表. セキュリティの設定

|JVMパラメータ|推奨値|説明|
|---|---|---|
|-Dhttps.protocols|TLSv1,TLSv1.1,TLSv1.2 (JDK 7、JDK 8向け)、TLSv1 (JDK 6向け)|OpenAMからのHTTPS接続に使用されるプロトコルを制御する。|

この設定は、Sun/Oracle HotSpot JVMに適用されます。

表. ガベージコレクションの設定

|JVMパラメータ|推奨値|説明|
|---|---|---|
|-verbose:gc|-|一般的なガベージコレクションの情報を出力する。|
|-Xloggc:|$CATALINA_HOME/logs/gc.log|ガーベッジコレクションのログファイルのパス。|
|-XX:+PrintClassHistogram|-|SIGTERMシグナルをJVMが受信した際に、ヒープのヒストグラムを出力する。|
|-XX:+PrintGCDetails|-|ガベージコレクションの詳細情報を出力する。|
|-XX:+PrintGCTimeStamps|-|ガベージコレクションの詳述なタイムスタンプを出力する。|
|-XX:+HeapDumpOnOutOfMemoryError|-|OutofMemoryErrorが発生したときに、自動的にヒープダンプを生成する。|
|-XX:HeapDumpPath|$CATALINA_HOME/logs/heapdump.hprof|ヒープダンプのパス。|
|-XX:+UseConcMarkSweepGC|-|CMS(コンカレントマークスイープ)ガベージコレクタを使用する。|
|-XX:+UseCMSCompactAtFullCollection|-|フルガベージコレクションでの積極的なコンパクションを行う。|
|-XX:+CMSClassUnloadingEnabled|-|CMSスイープ時のクラスのアンロードを許可する。|
