### OpenAMのアンインストール

この章では、OpenAMとポリシーエージェントをアンインストールする方法を示します。

#### OpenAMのコアサーバーのアンインストール

手順. OpenAMのコアサーバーをアンインストールする

OpenAMのコアサーバーをデプロイして設定が完了していると、OpenAMのファイルはシステムシステム上に4か所に保持されている可能性があります。

以下の手順で、OpenAMおよび内部設定ストアを削除します。外部の設定ストアを使用した場合は、すべてのソフトウェアを削除した後でOpenAMの設定データを削除することができます。

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

#### ポリシーエージェントのアンインストール

ポリシーエージェントのアンインストールは、エージェントのタイプによって若干異なるので、詳細は書くエージェントのインストールガイドを参照して下さい。ここでは、Apacheエージェント、IISエージェント、Tomcatエージェントについて説明します。

##### Apacheエージェントのアンインストール

Removing Apache Web Policy Agents
Procedure 4.8. To remove Web Policy Agents from Apache HTTP Server
Shut down the Apache server where the agent is installed.

Run agentadmin --l to output a list of the installed web policy agent configuration instances.

Make a note of the ID value of the configuration instance you want to remove.

Run agentadmin --r, and specify the ID of the web policy agent configuration instance to remove. A warning is displayed. Type yes to proceed with removing the configuration instance.

COPY TO CLIPBOARD$ ./agentadmin --r agent_3

Warning! This procedure will remove all OpenAM Web Agent references from
a Web server configuration. In case you are running OpenAM Web Agent in a
multi-virtualhost mode, an uninstallation must be carried out manually.

Continue (yes/no): [no]: yes

Removing agent_3 configuration...
Removing agent_3 configuration... Done.
Restart the Apache HTTP Server.

##### IISエージェントのアンインストール

Procedure 5.5. To remove a web policy agent from an IIS site
Log on to Windows as a user with administrator privileges.

Run agentadmin.exe --l to output a list of the installed web policy agent configuration instances.

COPY TO CLIPBOARDc:\web_agents\iis_agent\bin> agentadmin.exe --l
OpenAM Web Agent configuration instances:

   id:            agent_1
   configuration: c:\web_agents\iis_agent\bin\..\instances\agent_1
   server/site:   2
Make a note of the ID value of the configuration instance you want to remove.

Run agentadmin.exe --r, and specify the ID of the web policy agent configuration instance to remove.

COPY TO CLIPBOARDc:\web_agents\iis_agent\bin> agentadmin.exe --r agent_1

Removing agent_1 configuration...
Removing agent_1 configuration... Done.
Procedure 5.6. To remove web policy agents from IIS
Log on to Windows as a user with administrator privileges.

Run agentadmin --g. A warning is displayed. Type yes to proceed with removing the configuration instance.

COPY TO CLIPBOARDc:\web_agents\iis_agent\bin> agentadmin.exe --g

Warning! This procedure will remove all OpenAM Web Agent references from
IIS Server configuration.

Continue (yes/no): [no]: yes

Removing agent module from IIS Server configuration...
Removing agent module from IIS Server configuration... Done.

##### Tomcatエージェントのアンインストール

Remove Tomcat Policy Agent Software
Shut down the Tomcat server before you uninstall the policy agent:

COPY TO CLIPBOARD$ /path/to/tomcat/bin/shutdown.sh
To remove the Java EE policy agent, use agentadmin --uninstall. You must provide the Tomcat server configuration directory location.

Uninstall does not remove the agent instance directory, but you can do so manually after removing the agent configuration from Tomcat.
