## OpenAMのアンインストール

この章では、OpenAMとポリシーエージェントをアンインストールする方法を説明します。

### OpenAMのコアサーバーのアンインストール

**手順. OpenAMのコアサーバーをアンインストールする**

OpenAMのコアサーバーをデプロイし、設定が完了していると、OpenAMはファイルシステム上に4か所に保存されている可能性があります。

以下の手順で、OpenAMおよび内部設定ストアを削除します。外部設定ストアを使用した場合は、すべてのソフトウェアを削除した後でOpenAMの設定データを削除することができます。

1. OpenAMをデプロイしているWebアプリケーションコンテナをシャットダウンします。

    ```bash
    $ /etc/init.d/tomcat stop
    Password:
    Using CATALINA_BASE:   /path/to/tomcat
    Using CATALINA_HOME:   /path/to/tomcat
    Using CATALINA_TMPDIR: /path/to/tomcat/temp
    Using JRE_HOME:        /path/to/jdk/jre
    Using CLASSPATH:       /path/to/tomcat/bin/bootstrap.jar:
     /path/to/tomcat/bin/tomcat-juli.jar
    ```
    
2. Webアプリケーションコンテナを実行しているユーザーの$HOMEディレクトリにある設定ファイルを削除することで、OpenAMの設定を解除します。

    OpenAMのコアサービスと設定ファイルの完全なアンインストールは、以下のディレクトリの削除で成り立っています:
    - 設定ディレクトリ  
    デフォルトでは$HOME/openam。デフォルト設定の場所を使用しなかった場合は、Deployment > Servers > Server Name > General > System 以下のベースインストールディレクトリの設定値をチェックして下さい。
    - 設定ディレクトリを指すファイルを保持している隠しディレクトリ  
    WebコンテナとしてApache Tomcatを使用している場合、このファイルは例えば、$HOME/.openamcfg/AMConfig_path_to_tomcat_webapps_openam_や$HOME/.openssocfg/AMConfig_path_to_tomcat_webapps_openam_である可能性があります。  
      ```bash
      $ rm -rf $HOME/openam $HOME/.openamcfg
      ```
    
      または

      ```bash
      $ rm -rf $HOME/openam $HOME/.openssocfg
      ```
    
    外部設定ストアを使用した場合は、外部ディレクトリサーバーから手動で設定を削除する必要があります。 OpenAMの設定のベースDNは、デフォルトでdc=openam,dc=forgerock,dc=orgです。
    > **注意**  
    > OpenAMを完全に削除するのではなく、クリーンな設定で最初からやり直したい場合は、この時点でWebコンテナを再起動して、新たにOpenAMを設定することができます。

3. OpenAM Webアプリケーションをアンデプロイします。
    WebコンテナとしてApache Tomcatを使用している場合、コンテナから.warファイルと展開したWebアプリケーションを削除します。

    ```bash
    $ cd /path/to/tomcat/webapps/
    $ rm -rf openam.war openam/
    ```

### ポリシーエージェントのアンインストール

ポリシーエージェントのアンインストールは、エージェントのタイプによって若干異なるので、詳細は書くエージェントのインストールガイドを参照して下さい。ここでは、Apacheエージェント、IISエージェント、Tomcatエージェントについて説明します。

#### Apacheエージェントのアンインストール

**手順. Apache HTTPサーバーからWebポリシーエージェントをアンインストールする**

1. エージェントがインストールされているApacheサーバーをシャットダウンします。
2. インストールされているWebポリシーエージェントの設定インスタンスのリストを出力するため、agentadmin --lを実行します。
   削除する設定インスタンスのID値をメモします。
3. agentadmin --rを実行し、削除するWebポリシーエージェント構成インスタンスのIDを指定します。警告が表示されます。
   構成インスタンスの削除を続行するには、yesと入力します。
   ```bash
    ./agentadmin --r agent_3
    
    Warning! This procedure will remove all OpenAM Web Agent references from
    a Web server configuration. In case you are running OpenAM Web Agent in a
    multi-virtualhost mode, an uninstallation must be carried out manually.
    
    Continue (yes/no): [no]: yes
    
    Removing agent_3 configuration...
    Removing agent_3 configuration... Done.
   ```
4. Apache HTTPサーバーを再起動します。

#### IISエージェントのアンインストール

**手順. IISのサイトからWebポリシーエージェントをアンインストールする**

1. 管理者権限を持つユーザーでWindowsにログオンします。
2. インストールされたWebポリシーエージェントの設定インスタンスのリストを出力するために agentadmin.exe --l を実行します。
   ```cmd
    c:\web_agents\iis_agent\bin> agentadmin.exe --l
    OpenAM Web Agent configuration instances:
    
       id:            agent_1
       configuration: c:\web_agents\iis_agent\bin\..\instances\agent_1
       server/site:   2
   ```
   削除する設定インスタンスのID値をメモします。
3. agentadmin.exe --r を実行し、削除するWebポリシーエージェントの構成インスタンスのIDを指定します。
   ```cmd
    c:\web_agents\iis_agent\bin> agentadmin.exe --r agent_1
    
    Removing agent_1 configuration...
    Removing agent_1 configuration... Done.
   ```

**手順. IISからWebポリシーエージェントをアンインストールする**

1. 管理者権限を持つユーザーでWindowsにログオンします。
2. agentadmin --g を実行。警告が表示されます。コンフィギュレーションインスタンスの削除を続行するには、yesと入力します。
   ```cmd
    c:\web_agents\iis_agent\bin> agentadmin.exe --g
    
    Warning! This procedure will remove all OpenAM Web Agent references from
    IIS Server configuration.
    
    Continue (yes/no): [no]: yes
    
    Removing agent module from IIS Server configuration...
    Removing agent module from IIS Server configuration... Done.
   ```

#### Java EEエージェントのアンインストール

**手順. TomcatサーバーからJava EEポリシーエージェントをアンインストールする**

ポリシーエージェントをアンインストールする前に、Tomcatサーバーをシャットダウンします:
```bash
$ /path/to/tomcat/bin/shutdown.sh
```
Java EEポリシーエージェントを削除するには、 agentadmin --uninstall を使用します。Tomcatサーバー設定ディレクトリの場所を指定する必要があります。

アンインストールはエージェントインスタンスのディレクトリを削除しませんが、Tomcatからエージェントの設定を削除した後で手動で削除することができます。
