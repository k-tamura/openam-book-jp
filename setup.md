[TODO] 作成中

## OpenAMのインストールと初期設定

この章では、OpenAMをインストールと初期設定の方法について解説します。OSは、CentOS 7.0を使用しましたが、Windowsやそれ以外のOSでも基本的な手順は変わりません。

### OpenAMのインストール

#### JDKのインストール

まずは、Javaがインストールされているか確認します。

```
$ java -version
openjdk version "1.8.0_101"
OpenJDK Runtime Environment (build 1.8.0_101-b13)
OpenJDK 64-Bit Server VM (build 25.101-b13, mixed mode)
```

Javaがインストールされていないようでしたら、yumコマンドでOpenJDKをインストールします。

```
$ yum install java-1.8.0-openjdk
```

#### Tomcatのインストールと設定

　yumコマンドにより、tomcat7をインストールします。OpenAMの起動には、1GBのJavaヒープと256MBのpermanent領域が必要です。
　
```
$ yum install tomcat

$ export JAVA_OPTS="-Xmx1024m -XX:MaxPermSize=256m"
```

Tomcatを使用する場合は、server.xmlのConnectorタグにURIEncoding=”UTF-8″を追記しておいた方がいいです。いくつかの画面の文字化けや文字コードに起因する問題を回避できます。

```
<Connector port="8080" protocol="HTTP/1.1"
   connectionTimeout="20000"
   redirectPort="8443" URIEncoding="UTF-8" />
```

#### OpenAMのデプロイとTomcatの起動

OpenAMはForgeRock社のサイトからをOpenAM 13.0.0のwarファイルをダウンロードできます。

https://backstage.forgerock.com/#!/downloads/OpenAM/OpenAM%20Enterprise/13.0.0/OpenAM%2013

> **情報**
> ダウンロードするにはアカウント登録が必要です。

ダウンロードしたら、/usr/share/tomcat7/webapps/以下にコピーします。OpenAMのデプロイが完了したら、Tomcatを起動します。

```
$ mv OpenAM-13.0.0.war /usr/share/tomcat7/webapps/openam.war 

$ service tomcat7 start 
```

### OpenAMの初期設定

Tomcatの起動が確認できたら、次のURLにアクセスしてください。

http://openam.example.co.jp:8080/openam

デプロイが正常に完了していると、初期設定画面が表示されます。 「カスタム設定」の「新しい設定の作成」のリンクをクリックします。 

図. 初期設定画面

手順1： 一般

　デフォルトユーザーのパスワードを入力して、「次へ」ボタンをクリックします。デフォルトユーザーamadminとは、OpenAM管理コンソールでOpenAMの設定をするための管理者アカウントのことです。

図. 手順1： 一般

手順2： サーバー設定

　サーバーURLに「http://openam.example.co.jp:8080」を、Cookieドメインに「.example.co.jp」を入力して、「次へ」ボタンをクリックします。プラットフォームロケールはen_USのままで構いません。

図. 手順2： サーバー設定

手順3： 設定データストア設定

　初めての初期設定なので、「最初のインスタンス」を選択して、「次へ」ボタンをクリックします。 

図. 手順3： 設定データストア設定

手順4： ユーザーデータストア設定

　「OpenAMのユーザーデータストア」を選択して 「次へ」ボタンをクリックします。「OpenAMのユーザーデータストア」とは、OpenAMに内蔵されているディレクトリサーバー（OpenDJ）のことを意味します。
　
図. 手順4： ユーザーデータストア設定

手順5： サイト設定

　今回はロードバランサは使用しないので、「いいえ」を選択して 「次へ」ボタンをクリックします。

図. 手順5： サイト設定

手順6： デフォルトのポリシーエージェントユーザー

　デフォルトポリシーエージェントユーザーのパスワードを入力して、「次へ」ボタンをクリックします。パスワードは先程入力したamadminのパスワードとは異なるものにしてください。

図. 手順6： デフォルトのポリシーエージェントユーザー

　最後に設定に誤りがないことを確認して、「設定の作成」ボタンをクリックします。

図. 設定ツールの概要と詳細

　初期設定が実行されます。

図. 設定完了

　しばらくすると「設定が完了しました」というダイアログが表示されます。以上で、インストールと初期設定は完了です。 

　この中にある「ログインに進む」のリンクをクリックすると、OpenAM管理コンソールのログイン画面が表示されます。

