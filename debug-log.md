一般的なアプリケーションと同様に、OpenAMも問題が発生した際の解析などを目的としてデバッグログを出力します。OpenAMが意図した通りに動作しない場合、このログを確認することが問題解決の第一歩になります。  

### OpenAMサーバーのデバッグログ

OpenAMサーバーのデバッグログの出力先は、デフォルトで[OpenAMの設定ディレクトリ]/[コンテキスト名]/debug (例えば、/usr/share/tomcat7/openam/openam/debug) になります。この中に以下のようなカテゴリー単位のログが出力されます。

表. デバッグログのファイル名と内容

|ファイル名|内容|
|---|---|
|amUpgrade|OpenAMのアップグレード中に出力されるログ|
|Authentication|認証に関するログ|
|Configuration|設定に関するログ|
|CoreSystem|OpenAMのコアとなるクラスが出力するログ|
|Federation|SAMLなどのフェデレーション機能に関するログ|
|IdRepo|データストアにアクセスするモジュールが出力するログ|
|Policy|ポリシーに関するログ|
|Session|セッションに関するログ|

この他にも、カテゴリーに応じたデバッグログファイルは出力されます。

#### デバッグレベルと出力先の変更 

デバッグログのレベルと出力先は、管理コンソールで変更できます。
設定 > サーバーおよびサイト > サーバー名 をクリックして、表示された一般設定のページの「デバッグ」セクションで以下の設定ができます。

表. OpenAMのデバッグログの設定

|設定項目|デフォルト値|説明|
|---|---|---|
|デバッグレベル|エラー|OpenAM全体のデバッグレベル。  (プロパティー名: com.iplanet.services.debug.level)|
|デバッグファイルのマージ|オン|すべてのデバッグログを 1 つのファイル (debug.out) に転送します。オフ: カテゴリー毎に個別のデバッグファイルを作成します。  (プロパティー名: com.sun.services.debug.mergeall)|	
|デバッグディレクトリ|%BASE_DIR%/%SERVER_URI%/debug|デバッグファイルが存在するディレクトリ。  (プロパティー名: com.iplanet.services.debug.directory)|	

#### カテゴリー単位のデバッグレベルの変更 

OpenAMは、システム全体のログレベルだけでなく、カテゴリーやデバッグインスタンス単位でログレベルを変更することができます。
この機能を利用するには、amadminでOpenAMにログインし、Debug.jsp (例えば、http://openam.example.com:8080/openam/Debug.jsp) にアクセスします。  
例えば、カテゴリー「OAuth2Provider」のログレベルだけを「メッセージ」レベルにして、詳細なデバッグ情報を出力する、といったことができます。

### デバッグログのローテーション

デフォルトでは、OpenAMはデバッグログをローテーションしません。デバッグログをローテーションさせるには、OpenAMがデプロイされているディレクトリのWEB-INF/classes/debugconfig.propertiesを編集します。

表. デバッグログのローテーション設定

|プロパティ名|デフォルト値|説明|
|---|---|---|
|org.forgerock.openam.debug.prefix|無し|デバッグログファイルをローテーションさせる場合は、このプロパティにデバッグログファイルのプレフィックスを文字列で指定します。|
|org.forgerock.openam.debug.suffix|-MM.dd.yyyy-kk.mm|デバッグログファイルをローテーションさせる場合は、このプロパティにデバッグログファイルのサフィックスをSimpleDateFormat形式で指定します。|
|org.forgerock.openam.debug.rotation|無し|ローテーションの間隔を分単位で指定します。デバッグログのローテーションを有効にするには、ゼロよりも大きい値に設定します。|

debugconfig.propertiesへの変更は、OpenAMを再起動後に反映されます。

### ポリシーエージェントのデバッグログ

ポリシーエージェントのデバッグログの設定は、WebエージェントとJ2EEエージェントで若干異なります。

#### Webエージェントのデバッグログ

Webエージェントのデバッグログの出力先はデフォルトで、[Webエージェントのインストールディレクトリ]/agent_[N]/logs/debug/amAgent になります。
出力先を含むデバッグログの設定は、[Webエージェントのインストールディレクトリ]/agent_[N]/config/OpenSSOAgentBootstrap.properties に以下のように定義されています。

```
#
# AGENT DEBUG FILE
#   Name of the file to use for debug information.
#
# Hot-Swap Enabled: No
#
com.sun.identity.agents.config.debug.file = /root/web_agents/apache22_agent/Agent_001/logs/debug/amAgent

#
# AGENT DEBUG LOG LEVEL
# Set the logging level for the specified logging categories.
# The format of the values is
#
# <ModuleName>[:<Level>][,<ModuleName>[:<Level>]]*
#
# The currently used module names are: AuthService, NamingService,
# PolicyService, SessionService, PolicyEngine, ServiceEngine,
# Notification, PolicyAgent, RemoteLog and all.
#
# The all module can be used to set the logging level for all currently
# none logging modules. This will also establish the default level for
# all subsequently created modules.
#
# The meaning of the 'Level' value is described below:
#
# 0 Disable logging from specified module*
# 1 Log error messages
# 2 Log warning and error messages
# 3 Log info, warning, and error messages
# 4 Log debug, info, warning, and error messages
# 5 Like level 4, but with even more debugging messages
# 128 log url access to log file on AM server.
# 256 log url access to log file on local machine.
#
# If level is omitted, then the logging module will be created with
# the default logging level, which is the logging level associated with
# the 'all' module.
#
# For level of 128 and 256, you must also specify audit.access.type property.
#
# Even if the level is set to zero, some messages may be produced for
# a module if they are logged with the special level value of 'always'.
#
# Hot-Swap Enabled: No
#
com.sun.identity.agents.config.debug.level =256
```

また、デバッグログに関する設定の一部は管理コンソールで変更が可能です。
Webエージェントの場合は、 レルム > (レルム名) > エージェント > Web > グローバル をクリックして、表示されたエージェントのグローバル設定ページの「デバッグ」セクションで以下の設定ができます。
全ての項目でホットスワップが有効になっています。

表. Webエージェントのデバッグログの設定

|設定項目|デフォルト値|説明|
|---|---|---|
|エージェントデバッグレベル|エラー|ポリシーエージェントのデバッグレベル。  (プロパティー名: com.sun.identity.agents.config.debug.level)|
|エージェントのデバッグファイルローテーション|有効|デバッグファイルは指定されたサイズに基づいてローテーションされます。  (プロパティー名: com.sun.identity.agents.config.debug.file.rotate) |
|エージェントのデバッグファイルサイズ|10000000|エージェントのデバッグファイルサイズ (バイト単位)。  (プロパティー名: com.sun.identity.agents.config.debug.file.size) |

ただし、「エージェント設定リポジトリの場所」が「集中化」に選択されている場合に限ります。
「ローカル」に設定されている場合は、ポリシーエージェントのインストールディレクトリ配下にある [Webエージェントのインストールディレクトリ]/agent_[N]/config/OpenSSOAgentConfiguration.properties というファイルでこれらの設定を変更可能です。

#### J2EEエージェントのデバッグログ

J2EEエージェントのデバッグログの出力先はデフォルトで、[J2EEエージェントのインストールディレクトリ]/agent_[N]/logs/debug/debug.out になります。
出力先を含むデバッグログの設定は、[J2EEエージェントのインストールディレクトリ]/agent_[N]/config/OpenSSOAgentBootstrap.properties に以下のように定義されています。

```
・OpenSSOAgentBootstrap.properties

#
# DEBUG SERVICE PROPERTIES
# - com.iplanet.services.debug.directory: Specifies the complete path to the
# directory where debug files will be stored by the Agent.
# - com.sun.services.debug.mergeall: consolidates all the debug information
# into one file if it is set to on. Each component has its own debug file
# if it is set to off.
# Hot-Swap Enabled: No
#
com.iplanet.services.debug.level=error
com.iplanet.services.debug.directory=/home/jetty/j2ee_agents/jetty_v7_agent/Agent_003/logs/debug
com.sun.services.debug.mergeall=on
```

また、デバッグログに関する設定の一部は管理コンソールで変更が可能です。J2EEエージェントの場合は、 レルム > (レルム名) > エージェント > Web > グローバル をクリックして、表示されたエージェントのグローバル設定ページの「デバッグ」セクションで以下の設定ができます。全ての項目でホットスワップが有効になっています。

表. J2EEエージェントのデバッグログの設定

|設定項目|デフォルト値|説明|
|---|---|---|
|エージェントデバッグレベル|エラー|ポリシーエージェントのデバッグレベル。  (プロパティー名: com.iplanet.services.debug.level)|

ただし、こちらも「エージェント設定リポジトリの場所」が「集中化」に選択されている場合に限ります。
「ローカル」に設定されている場合は、ポリシーエージェントのインストールディレクトリ配下にある[J2EEエージェントのインストールディレクトリ]/agent_[N]/config/OpenSSOAgentConfiguration.properties というファイルでこれらの設定を変更可能です。
