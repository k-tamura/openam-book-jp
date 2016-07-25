OpenAMのインストール

　OpenAMのインストールは簡単です。基本的にはサーブレットコンテナにwarファイルをデプロイするだけです。ただし、Javaのヒープサイズやホストの名前解決の設定などが必要です。今回はCentOSを使って説明しますが、Windowsなどでも同様の手順になります。
OSのセットアップ

　まずはOpenAMをインストールするために必要なOSをセットアップします。※以降の作業は全てrootユーザで行っています。

　OpenAMサーバにアクセスするには、OpenAMサーバのホスト名をFQDN（注1）で名前解決できなければなりません。ここでは OpenAMサーバのFQDNを「openam.example.co.jp」とします。

$ vi /etc/sysconfig/network

HOSTNAME=openam.example.co.jp 

　今回は、DNSでの名前解決はせず、hostsファイルに定義を追加します（IPアドレス部分は環境に合わせて変更してください）。

$ vi /etc/hosts

192.168.1.11 openam.example.co.jp

　動作検証が目的であるため、ファイアウォールやSELinuxは無効にします。CentOSの端末で以下を実行します。

$ service iptables stop
$ chkconfig iptables off

$ vi /etc/sysconfig/selinux

#SELINUX=enforcing
#↓disabledに変更
SELINUX=disabled

　設定を反映させるため、OSを再起動します。

注1　FQDN

　ホスト名、ドメイン名（サブドメイン名）などすべてを省略せずに指定した識別子のこと。「openam.example.co.jp」はホスト名「openam」とドメイン名「example.co.jp」をすべて揃えたFQDNとなります。
JDKのインストール

　Oracle社のサイトからJDKをダウンロードし、インストールします。

$ wget http://download.oracle.com/otn-pub/java/jdk/6u37-b06/jdk-6u37-linux-i586-rpm.bin

$ sh ./jdk-6u37-linux-i586-rpm.bin

Tomcatのインストールと設定

　yumコマンドにより、tomcat6（※2）をインストールします。インストールが完了したら環境変数JAVA_HOMEとJAVA_OPTSをセットします。OpenAMの起動には、1GBのJavaヒープと256MBのpermanent領域が必要です。

$ yum install tomcat6

$ export JAVA_HOME=/usr/java/default/
$ export JAVA_OPTS="-Xmx1024m -XX:MaxPermSize=256m"

OpenAMのデプロイとTomcatの起動

　OpenAMはForgeRock社のサイトからをwarファイルをダウンロードできます。ダウンロードしたら、/usr/share/tomcat6/webapps/以下にコピーします。OpenAMのデプロイが完了したら、Tomcatを起動します。

$ wget http://download.forgerock.org/downloads/openam/openam10/10.0.0/openam_10.0.0.war
$ mv openam_10.0.0.war /usr/share/tomcat6/webapps/openam.war 

$ service tomcat6 start 

注2

　最新のTomcatにOpenAMをデプロイした場合、Tomcat停止時に以下のように「致命的エラー」がコンソールとログに出力されます。これはTomcat 6.0.24から追加されたメモリリーク検知機能によるものです。SEVEREとなっていますが、プログラム停止とともにメモリは解放されるので問題ありません。

SEVERE: A web application appears to have started a thread named [Session Monitor] but has failed to stop it. This is very likely to create a memory leak.
2012/11/11 23:37:33 org.apache.catalina.loader.WebappClassLoader clearReferencesThreads

