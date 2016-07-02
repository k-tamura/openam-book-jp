この章では、WebベースのOpenAMコンソールについて簡単に紹介します。
また、各コマンドラインインターフェイス（CLI）管理ツールについても一覧表示し、説明します。

### OpenAM管理コンソール

OpenAMをインストールした後、インストール時に設定したパスワードを使用して、OpenAM管理者(amadmin)としてWebベースのコンソールにログインします。http://openam.example.com:8080/openamのようなURLにアクセスします。この場合、HTTPプロトコルを介して、FQDN(openam.example.com)上の、Java EE標準のWebコンテナのポート番号(8080)に、指定したデプロイメントURI(/openam)に、通信が行われます。

図. OpenAM管理コンソール

When you log in as the OpenAM administrator, amadmin, you have access to the complete OpenAM console. In addition, OpenAM has set a cookie in your browser that lasts until the session expires, you logout, or you close your browser.[1]

When you log in to the OpenAM console as a non-administrative end user, you do not have access to the administrative console. Your access is limited to self-service profile pages and user dashboard.

図. 管理者以外のユーザーのOpenAM管理コンソール

If you configure OpenAM to grant administrative capabilities to another user, then that user is able to access both the administration console in the realms they can administrate, and their self-service profile pages.

図. 委任された管理者のOpenAM管理コンソール

For more on delegated administration, see Delegating Realm Administration Privileges .
