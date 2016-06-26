次のOpenAMのコマンドラインツールをインストールすることができます。:

> **情報**  
> 次のリストにあるスクリプトツールは、Microsoft Windows上で使用するための.batのバージョンもあります。

- **agentadmin**  
 このツールを使用すると、OpenAMポリシーエージェントのインストールを管理することができます。
 ポリシーエージェントのインストールの一部として、このツールを解凍します。

- **ampassword**  
 このツールを使用すると、OpenAM管理者のパスワードを変更したり、暗号化されたパスワードの値を表示することができます。
 SSOAdminTools-13.0.0.zipを解凍して、これをインストールして下さい。
 
- **amverifyarchive**  
 このツールは改ざんが無いかログアーカイブをチェックします。
 SSOAdminTools-13.0.0.zipを解凍して、これをインストールして下さい。

- **openam-distribution-configurator-13.0.0.jar**  
 この実行可能な.jarファイルは、設定ファイルを使用してOpenAMサーバのサイレントインストールを実行できます。
例えば、`java -jar configurator.jar -f config.file`コマンドは、config.fileとconfigurator.jarのアーカイブを結びつけます。
ツールで提供されたsampleconfigurationファイルは、config.fileのフォーマットで設定されており、それが使用している環境に適合させる必要があります。
SSOConfiguratorTools-13.0.0.zipを解凍して、これをインストールして下さい。
 
- **ssoadm**  
 このツールは、OpenAMのコアサービスの設定のための、豊富なコマンドラインインタフェースを提供します。
 テスト環境であれば、ブラウザで同じ機能にアクセスするためにssoadm.jspをアクティブにすることができます。
アクティブにしたら、次のようなURLに移動して、ssoadmコマンドの多くの機能を使用することができます。
 http://openam.example.com:8080/openam/ssoadm.jsp

コマンドは、HTTP(またはHTTPS)経由でOpenAMの設定にアクセスします。
サイト構成で管理コマンドを使用している場合、コマンドはフロントエンドのロードバランサーを介して設定にアクセスします。

以下の理由により、コマンドがロードバランサーにアクセスできないことがあります:

- ネットワークルーティングの制限により、ツールがロードバランサーにアクセスすることを拒否される場合
- テストのために、ロードバランサーがHTTPS用の自己署名証明書を使用し、ツールが自己署名証明書を信頼する方法がない場合
- ロードバランサは、一時的に利用不可能な場合

In such cases you can work around the problem by adding an option for each node, such as the following to the java command in the tool's script.
このようなケースでは、ツールのスクリプト内のjavaコマンドに、次のように各ノードに対してオプションを追加することで問題を回避することができます。

ノード 1:

> -D"com.iplanet.am.naming.map.site.to.server=https://lb.example.com:443/openam=
> http://server1.example.com:8080/openam"

ノード 2:

> -D"com.iplanet.am.naming.map.site.to.server=https://lb.example.com:443/openam=
> http://server2.example.com:8080/openam"

上記の例では、ロードバランサーはlbのホスト上にあり、https://lb.example.com:443/openamはサイト名で、サイト内のOpenAMサーバーはserver1のホスト上とserver2のホスト上にあります。

以下のようなマッピングがある場合、ssoadmコマンドはマップ内の最新の値のみを使用します。

> -D"com.iplanet.am.naming.map.site.to.server=https://lb.example.com:443/openam=
> http://server1.example.com:8080/openam, https://lb.example.com:443/openam=
> http://server2.example.com:8080/openam"

ssoadmコマンドは常に以下とやりとりします:

> http://server2.example.com:8080/openam
