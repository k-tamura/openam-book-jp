[TODO] ※このページは作成中です。

### OpenAMサーバーのデバッグログ

一般的なアプリケーションと同様に、OpenAMも問題が発生した際の解析などを目的としてデバッグログを出力します。OpenAMが意図した通りに動作しない場合、このログを確認することが問題解決の第一歩になります。デバッグログの出力先は、デフォルトで[OpenAMの設定ディレクトリ]/[コンテキスト名]/debug (例えば、/usr/share/tomcat7/openam/openam/debug) になります。この中に以下のようなカテゴリー単位のログが出力されます。

- Authentication
- Configuration
- CoreSystem
- Entitlement
- IdRepo
- Policy
- Session

#### OpenAM全体のデバッグレベルと出力先の変更 

OpenAM全体のデバッグレベルは、管理コンソールで変更できます。
設定 > サーバーおよびサイト > サーバー名 をクリックして、表示された一般設定のページの「デバッグ」セクションで以下の設定ができます。

表. デバッグログの設定

|設定項目|デフォルト値|説明|
|---|---|---|
|デバッグレベル|エラー|OpenAM全体のデバッグレベル。  (プロパティー名: com.iplanet.services.debug.level)|
|デバッグファイルのマージ|オン|すべてのデバッグログを 1 つのファイル (debug.out) に転送します。オフ: コンポーネントごとに個別のデバッグファイルを作成します  (プロパティー名: com.sun.services.debug.mergeall)|	
|デバッグディレクトリ|%BASE_DIR%/%SERVER_URI%/debug|デバッグファイルが存在するディレクトリ。  (プロパティー名: com.iplanet.services.debug.directory)|	

#### カテゴリー単位のデバッグレベルの変更 

OpenAMは、システム全体のログレベルだけでなく、カテゴリーやデバッグインスタンス単位でログレベルを変更することができます。
この機能を利用するには、amadminでOpenAMにログインし、Debug.jsp (例えば、http://openam.example.com:8080/openam/Debug.jsp) にアクセスします。

### Policy Agentのデバッグログ
