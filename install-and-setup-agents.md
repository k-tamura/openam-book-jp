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

**Tomcatエージェントインストール前の事前準備**

**手順. TomcatサーバーにTomcatエージェントをインストールする**

ポリシーエージェントをアンインストールする前に、Tomcatサーバーをシャットダウンします:
```bash
$ /path/to/tomcat/bin/shutdown.sh
```
Java EEポリシーエージェントを削除するには、 agentadmin --uninstall を使用します。Tomcatサーバー設定ディレクトリの場所を指定する必要があります。

アンインストールはエージェントインスタンスのディレクトリを削除しませんが、Tomcatからエージェントの設定を削除した後で手動で削除することができます。

###  Web Agentのインストールとセットアップ

**インストール前の事前準備**

This section describes the prerequisite steps you should take before installing the web policy agents into Apache HTTP servers.

Avoid installing the web server and the web policy agent as root. Instead, create a web server user and install as that user.

If you cannot avoid installing the web server and web policy agent as root, then you must give all users read and write permissions to the logs and logs/debug directories under the agent instance directory (/web_agents/type/Agent_nnn/logs/). Otherwise, the web policy agent fails with an error when attempting to rotate log files.

Tip
The installer can automatically set permissions on folders that require write access, by reading the Apache config file to determine the correct group and user to grant privileges to. Answer yes when prompted:
COPY TO CLIPBOARDChange ownership of created directories using
User and Group settings in httpd.conf
[ q or 'ctrl+c' to exit ]
(yes/no): [no]: yes
The SELinux OS feature can prevent the agents from being able to write to audit and debug logs. See Troubleshooting .

Ensure OpenAM is installed and running, so that you can contact OpenAM from the system running the policy agent.

Create a profile for your policy agent as described in Configuring Web Policy Agents .

Create at least one policy in OpenAM to protect resources with the agent, as described in the section on Configuring Policies . Consider creating a simple policy, such as a policy that allows only authenticated users to access your resources. This allows you to test your policy agent after installation.

If the OpenAM server uses SSL, you must install OpenSSL on the agent machine.

On UNIX systems, ensure the OpenSSL libraries libcrypto.so and libssl.so are available in the path specified by either the LD_LIBRARY_PATH or LD_LIBRARY_PATH_64 environment variables.

On Windows systems, ensure the OpenSSL libraries libeay32.dll and ssleay32.dll are available in the lib folder of your agent installation, for example c:\path\to\web_agents\iis_agent\lib\.

Install Apache HTTP Server before you install the policy agent. You must stop the server during installation.

See the OpenAM Installation Guide section, Obtaining OpenAM Software to determine which version of the agent to download, and download the agent. Also, verify the checksum of the file you download against the checksum posted on the download page.

Unzip the file in the directory where you plan to install the web policy agent. The agent stores its configuration and logs under this directory.

When you unzip the policy agent .zip download, you find the following directories:

bin
The installation and configuration program agentadmin.

config
Configuration templates used by the agentadmin command during installation.

instances
Configuration files, and audit and debug logs for individual instances of the web policy agents will be created here. The folder is empty when first extracted.

legal
Contains licensing information including third-party licenses.

lib
Shared libraries used by the policy agent.

log
Location for log files written during installation. The folder is empty when first extracted.

4.1.1. Tuning Apache Multi-Processing Modules
Apache 2.0 and later comes with Multi-Processing Modules (MPMs) that extend the basic functionality of a web server to support the wide variety of operating systems and customizations for a particular site.

The key area of performance tuning for Apache is to run in worker mode ensuring that there are enough processes and threads available to service the expected number of client requests. Apache performance is configured in the conf/extra/http-mpm.conf file.

The key properties in this file are ThreadsPerChild and MaxClients. Together the properties control the maximum number of concurrent requests that can be processed by Apache. The default configuration allows for 150 concurrent clients spread across 6 processes of 25 threads each.

COPY TO CLIPBOARD<IfModule mpm_worker_module>
   StartServers          2
   MaxClients          150
   MinSpareThreads      25
   MaxSpareThreads      75
   ThreadsPerChild      25
   MaxRequestsPerChild   0
</IfModule>
Important
For the policy agent notification feature, the MaxSpareThreads, ThreadLimit and ThreadsPerChild default values must not be altered; otherwise the notification queue listener thread cannot be registered.
Any other values apart from these three in the worker MPM can be customized. For example, it is possible to use a combination of MaxClients and ServerLimit to achieve a high level of concurrent clients.

**手順. Apache HTTPサーバーにApacheエージェントをインストールする**

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

**手順. IISのサイトにIISエージェントをインストールする**

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
