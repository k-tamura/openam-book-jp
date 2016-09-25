## Java EEエージェントのビルド

Java EEエージェントのビルドもOpenAMサーバー同様にmvnコマンドを実行するだけで完了します。

### ビルド環境の整備

ビルドに必要なソフトウェアは、JDK、Mavenです。前提ソフトウェアのバージョンについては命じされていませんが、OpenAMのビルドができるバージョンを使えば問題はないはずです。

|Java JDK|Maven|
|---|---|
|1.7 以上|3.1.0 以上|

IBM WebSphere Application ServerとOracle WebLogic Server用のモジュールは、サーバベンダーが提供する非公開のライブラリを必要とすることに注意してください。これらの依存関係を解決するには、これらのサーバーソフトウェアに含まれるライブラリを取得し、mvnコマンド実行時に参照できるようにしておく必要があります。詳細に関しては、Java EEエージェントのソースコードに添付されているreadme.mdファイルを参照してください。

**手順. Java EEエージェントのビルド**

まずは、ビルド用のディレクトリを作成し、そこに移動します。

```bash
$ mkdir jeeagent_build
$ cd jeeagent_build/
```

ソースコードはForgeRock社のBitbucketサーバー上で公開されています。ここでは、バージョン3.5.0のソースコードを圧縮したファイルをダウンロードします。

https://stash.forgerock.org/projects/OPENAM/repos/jee-agents/browse?at=refs%2Ftags%2F3.5.0

ダウンロードが完了したら、圧縮ファイルを解凍してください。

```bash
$ unzip jee-agents-master\@36016b991ff.zip 
Archive:  jee-agents-master@36016b991ff.zip
36016b991ff2aeb694a5d09e062591704f9eec45
 extracting: .gitignore              
   creating: jee-agents-agentapp/
  inflating: jee-agents-agentapp/pom.xml  

  .......
      
  inflating: pom.xml                 
  inflating: readme.md               
```

解凍したら圧縮ファイルは削除します。

```bash
$ rm jee-agents-master\@36016b991ff.zip 
rm: remove 通常ファイル `jee-agents-master@36016b991ff.zip'? yes
```

ディレクトリ構成は、以下のようになっています。各サーバー毎のエージェントモジュールやサンプルアプリケーション(sampleapp)用のモジュールが、ディレクトリ毎に分かれて配置されています。

```bash
$ ll
合計 1828
drwxr-xr-x  3 root root    4096  9月 16 23:26 2016 jee-agents-agentapp
drwxr-xr-x  4 root root    4096  9月 16 23:26 2016 jee-agents-appserver
drwxr-xr-x 12 root root    4096  9月 16 23:26 2016 jee-agents-distribution
drwxr-xr-x  5 root root    4096  9月 16 23:26 2016 jee-agents-jboss
drwxr-xr-x  4 root root    4096  9月 16 23:26 2016 jee-agents-jetty
drwxr-xr-x  3 root root    4096  9月 16 23:26 2016 jee-agents-jsr196
drwxr-xr-x  3 root root    4096  9月 16 23:26 2016 jee-agents-library
drwxr-xr-x 15 root root    4096  9月 16 23:26 2016 jee-agents-sampleapp
drwxr-xr-x  3 root root    4096  9月 16 23:26 2016 jee-agents-sdk
drwxr-xr-x  3 root root    4096  9月 16 23:26 2016 jee-agents-tomcat
drwxr-xr-x  3 root root    4096  9月 16 23:26 2016 jee-agents-weblogic
drwxr-xr-x  4 root root    4096  9月 16 23:26 2016 jee-agents-websphere
drwxr-xr-x  2 root root    4096  9月 16 23:26 2016 legal
-rwxr-xr-x  1 root root   33221  9月 16 23:26 2016 pom.xml
-rw-r--r--  1 root root    2097  9月 16 23:26 2016 readme.md
$ 
```

全体のpom.xmlがあるので、ここでmvnコマンドを実行すれば、ビルドが開始されますが、前述した商用のサーバーのライブラリが存在しないので、ビルドエラーとなってしまいます。Tomcatエージェントだけをビルドしたいということでれば、以下のようにTomcat以外のサーバー用のmoduleタグをコメントアウトする必要があります。

```bash
$ vi pom.xml
    <modules>
        <module>jee-agents-sdk</module>
        <module>jee-agents-library</module>
        <module>jee-agents-agentapp</module>
        <module>jee-agents-tomcat</module>
<!-- ここから
        <module>jee-agents-jboss</module>
        <module>jee-agents-jetty</module>
        <module>jee-agents-appserver</module>
        <module>jee-agents-weblogic</module>
        <module>jee-agents-websphere</module>
        <module>jee-agents-jsr196</module>
ここまでコメントアウト -->
        <module>jee-agents-sampleapp</module>
        <module>jee-agents-distribution</module>
    </modules>
```

`mvn clean install`コマンドでビルドを実行します。テストを省略する場合は、`-DskipTests=true`を付加してください。

```bash
$ mvn -DskipTests=true clean install
[INFO] Scanning for projects...
[WARNING] 
[WARNING] Some problems were encountered while building the effective model for org.forgerock.openam.agents:jee-agents-tomcat-v6:jar:4.0.0-SNAPSHOT

  .......

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1:00.510s
[INFO] Finished at: Sun Sep 25 09:42:15 JST 2016
[INFO] Final Memory: 64M/349M
[INFO] ------------------------------------------------------------------------
```

数分で、ビルドは完了します。jee-agents-distribution/jee-agents-distribution-tomcat-v6/target/以下に、Tomcatエージェントの圧縮ファイル(tomcat_v6_agent_4.0.0-SNAPSHOT.zip)が作成されているはずです。

```bash
$ ll jee-agents-distribution/jee-agents-distribution-tomcat-v6/target/
合計 33888
drwxr-xr-x 2 root root     4096  9月 25 09:41 2016 archive-tmp
-rw-r--r-- 1 root root       27  9月 25 09:41 2016 build_date.js
-rw-r--r-- 1 root root 34692736  9月 25 09:41 2016 tomcat_v6_agent_4.0.0-SNAPSHOT.zip
```
