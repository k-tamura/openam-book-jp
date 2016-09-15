## OpenAMをビルドする

OpenAMのビルドはとても簡単です。
基本的には、OpenAMのソースコードをダウンロードして、Mavenのコマンドを実行するだけです。
ただし、注意すべき点がいくつかあります。

- OpenAMと、Java、Maven、Gitのバージョンを確認すること
- Windows上でのビルドは失敗する可能性があるので、LinuxかMac上で実施すること

### ビルド環境の整備

OpenAM 11.0.0以降のビルドに必要なソフトウェアは、JDK、Maven、Gitですが、OpenAMのバージョンによって若干異なります。

|OpenAM|Java JDK|Maven|Git|
|---|---|---|---|
|OpenAM 13.0.0|1.7 以上|3.1.0 以上|1.7.6 以上|
|OpenAM 12.0.0|1.6 以上|3.3.1 以下|1.7.6 以上|
|OpenAM 11.0.0|1.6 または 1.7|3.0.5|1.7.6 以上|

Mavenのビルドコマンドを実行する前に、これらのバージョンを確認しておきましょう。

```
$ mvn -version
Apache Maven 3.1.0 (893ca28a1da9d5f51ac03827af98bb730128f9f2; 2013-06-28 11:15:32+0900)
Maven home: /opt/apache-maven-3.1.0
Java version: 1.7.0_101, vendor: Oracle Corporation
Java home: /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.101.x86_64/jre
Default locale: ja_JP, platform encoding: UTF-8
OS name: "linux", version: "2.6.32-642.el6.x86_64", arch: "amd64", family: "unix"

$ git --version
git version 2.2.0
```

### ソースコードの取得

ソースコードはForgeRock社のBitbucketサーバーからgit cloneで取得してくることもできますが、アカウントをつくってログインしないといけないので、GitHubからダウンロードします。ダウンロードしたら、解凍して出来たディレクトリに移動して下さい。

```
$ wget --no-check-certificate https://github.com/ForgeRock/openam/archive/13.0.0.zip
$ unzip 13.0.0.zip
$ cd openam-13.0.0
```

### Mavenによるビルド

以下のコマンドを実行すると、ビルドが始まります。

```
$ export MAVEN_OPTS="-Xmx1024m -XX:MaxPermSize=512m"
$ mvn rue clean install
```

マシンの性能によりますが、数十分程度の時間がはかかります。完了すると、openam-server/target/にOpenAM-13.0.0.warが作成されます。 ビルドするだけであれば、-DskipTests=trueオプションを付けることにより、テストをスキップして短時間で完了させることもできます。

```
$ mvn -DskipTests=true clean install
```
