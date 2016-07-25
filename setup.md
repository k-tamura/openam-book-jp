[TODO] 作成中

## OpenAMのインストールと初期設定

以下の環境にOpenAMをインストール

- CentOS 6.7

### OpenAMのインストール

#### JDKのインストール

Oracle社のサイトからJDKをダウンロードし、インストールします。
```
$ wget http://download.oracle.com/otn-pub/java/jdk/6u37-b06/jdk-6u37-linux-i586-rpm.bin

$ sh ./jdk-6u37-linux-i586-rpm.bin
```

#### Tomcatのインストールと設定

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

