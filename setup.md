## OpenAMのインストールと初期設定

### JDKのインストール

Oracle社のサイトからJDKをダウンロードし、インストールします。
```
$ wget http://download.oracle.com/otn-pub/java/jdk/6u37-b06/jdk-6u37-linux-i586-rpm.bin

$ sh ./jdk-6u37-linux-i586-rpm.bin
```

Tomcatのインストールと設定

　yumコマンドにより、tomcat6（※2）をインストールします。インストールが完了したら環境変数JAVA_HOMEとJAVA_OPTSをセットします。OpenAMの起動には、1GBのJavaヒープと256MBのpermanent領域が必要です。
　
```
$ yum install tomcat6

$ export JAVA_HOME=/usr/java/default/
$ export JAVA_OPTS="-Xmx1024m -XX:MaxPermSize=256m"
```

OpenAMのデプロイとTomcatの起動

　OpenAMはForgeRock社のサイトからをwarファイルをダウンロードできます。ダウンロードしたら、/usr/share/tomcat6/webapps/以下にコピーします。OpenAMのデプロイが完了したら、Tomcatを起動します。

```
$ wget http://download.forgerock.org/downloads/openam/openam10/10.0.0/openam_10.0.0.war
$ mv openam_10.0.0.war /usr/share/tomcat6/webapps/openam.war 

$ service tomcat6 start 
```


