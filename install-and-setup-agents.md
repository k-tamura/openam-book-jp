## Policy Agentのインストールとセットアップ

1. エージェントプロファイルの作成
2. エージェントのインストール
3. エージェントプロファイルの修正
4. 認可ポリシーの作成

### エージェントプロファイルの作成

This section concerns creating agent profiles, and creating groups that let agents inherit settings when you have many agents with nearly the same profile settings.
Procedure 3.1. To Create an Agent Profile

To create a new Web or Java EE policy agent profile, you need to create a name and password for the agent. You also need the URLs to OpenAM and the application to protect:

    Login to OpenAM Console as an administrative user.

    On the Realms menu of the OpenAM console, select the realm in which the agent profile is to be managed.

    Click the Agents link, click the tab page for the kind of agent profile you want to create, and then click the New button in the Agent table.

    In the Name field, enter a name for the agent profile.

    In the Password and Re-Enter Password fields, enter a password for the new agent profile.

    Click Local or Centralized (Default) to determine where the agent properties are stored. If you select Local, the properties are stored on the server on which the agent is running. If you select Centralized, the properties are stored on the OpenAM server.

    In the Server URL field, enter the URL to OpenAM. For example, http://openam.example.com:8080/openam.

    In the Agent URL field, enter the primary URL of the web or application server protected by the policy agent. Note for web agents, an example URL would look like: http://www.example.com:80. For Java EE policy agents, an example URL must include the agentapp context: http://www.example.com:8080/agentapp.

    Click Create. After creating the agent profile, you can click the link to the new profile to adjust and export the configuration.

###  Java EE Agentのインストールとセットアップ

**手順. TomcatサーバーからJava EEポリシーエージェントをアンインストールする**

ポリシーエージェントをアンインストールする前に、Tomcatサーバーをシャットダウンします:
```bash
$ /path/to/tomcat/bin/shutdown.sh
```
Java EEポリシーエージェントを削除するには、 agentadmin --uninstall を使用します。Tomcatサーバー設定ディレクトリの場所を指定する必要があります。

アンインストールはエージェントインスタンスのディレクトリを削除しませんが、Tomcatからエージェントの設定を削除した後で手動で削除することができます。

###  Web Agentのインストールとセットアップ

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
