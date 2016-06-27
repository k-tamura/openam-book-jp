次のOpenAMのコマンドラインツールをインストールすることができます。:

- **agentadmin**  
 このツールを使用すると、ポリシーエージェントのインストールやアンインストールができます。
 ポリシーエージェントの圧縮ファイルをダウンロードして、解凍したディレクトリ内にこのツールが存在します。

- **ampassword**  
 このツールを使用すると、OpenAM管理者のパスワードを変更したり、暗号化されたパスワードの値を表示することができます。
 SSOAdminTools-13.0.0.zipを解凍して、これをインストールして下さい。
 
- **amverifyarchive**  
 このツールは改ざんが無いかログアーカイブをチェックします。
 SSOAdminTools-13.0.0.zipを解凍して、これをインストールして下さい。

- **openam-distribution-configurator-13.0.0.jar**  
 この実行可能な.jarファイルは、設定ファイルを使用してOpenAMサーバーのサイレントインストールを実行できます。
例えば、`java -jar configurator.jar -f config.file`コマンドは、config.fileに記載された設定をもとにインストールを実行します。
ツールで提供されたsampleconfigurationファイルには、config.fileのフォーマットが記載されており、それを使用している環境に合わせて変更する必要があります。
SSOConfiguratorTools-13.0.0.zipを解凍して、これをインストールして下さい。
 
- **ssoadm**  
 このツールは、OpenAMのコアサービスの設定のための、豊富なコマンドラインインタフェースを提供します。
 テスト環境であれば、ブラウザで同じ機能にアクセスするためにssoadm.jspをアクティブにすることができます。
アクティブにしたら、次のようなURLに移動して、ssoadmコマンドの多くの機能を使用することができます。
 http://openam.example.com:8080/openam/ssoadm.jsp

> **情報**  
> 上のリストにあるスクリプトツールは、Microsoft Windows上で使用するための.batのバージョンもあります。

コマンドは、HTTP(またはHTTPS)経由でOpenAMの設定にアクセスします。
サイト構成で管理コマンドを使用している場合、コマンドはフロントエンドのロードバランサーを介して設定にアクセスします。

以下の理由により、コマンドがロードバランサーにアクセスできないことがあります:

- ネットワークルーティングの制限により、ツールがロードバランサーにアクセスすることを拒否される場合
- テストのために、ロードバランサーがHTTPS用の自己署名証明書を使用し、ツールが自己署名証明書を信頼する方法がない場合
- ロードバランサは、一時的に利用不可能な場合

このようなケースでは、ツールのスクリプト内のjavaコマンドに、次のように各ノードに対してオプションを追加することで問題を回避することができます。

ノード 1:

> -D"com.iplanet.am.naming.map.site.to.server=https://lb.example.com:443/openam=
> http://server1.example.com:8080/openam"

ノード 2:

> -D"com.iplanet.am.naming.map.site.to.server=https://lb.example.com:443/openam=
> http://server2.example.com:8080/openam"

上記の例では、ロードバランサーはlbのホスト上にあり、https://lb.example.com:443/openam はサイト名で、サイト内のOpenAMサーバーはserver1のホスト上とserver2のホスト上にあります。

以下のようなマッピングがある場合、ssoadmコマンドはマップ内の最新の値のみを使用します。

> -D"com.iplanet.am.naming.map.site.to.server=https://lb.example.com:443/openam=
> http://server1.example.com:8080/openam, https://lb.example.com:443/openam=
> http://server2.example.com:8080/openam"

ssoadmコマンドは常に以下とやりとりします:

> http://server2.example.com:8080/openam
