## Policy Agentのインストールとセットアップ

Policy Agentを保護対象のアプリケーションのあるサーバーにインストールして、OpenAMと連携できるようにするには、以下の作業が必要です。

1. インストール要件の確認など
2. 事前準備
3. エージェントプロファイルの作成
4. エージェントのインストール
5. 認可ポリシーの作成など

まずは、Policy Agentがインストール可能なサーバーであるかを確認します(1)。Policy Agentがインストール可能なサーバーはApache HTTP Server、IIS、Tomcat、JBossなどで、バージョンもインストール要件を満たしている必要があります。インストール要件を満たしていることを確認したら、サーバーの設定等を必要に応じて変更します(2)。次に、OpenAMの管理コンソールでPolicy Agentのプロファイルを作成します(3)。プロファイルは、OpenAMと連携するためには必要なPolicy Agentの情報です。プロファイルを作成したら、エージェントをインストールします(4)。最後に、保護するアプリケーションへのアクセスを部分的に許可、拒否するための認可ポリシーを作成します(5)。

###  Java EE Agentのインストールとセットアップ

**手順. TomcatサーバーにTomcatエージェントをインストールする**

#### 1. 事前準備(インストール要件の確認など)

まずはインストールが可能な環境であるかを確認する必要があります。

#### 2. エージェントプロファイルの作成

To create a new Java EE policy agent profile, you need to create a name and password for the agent. You also need the URLs to OpenAM and the application to protect:

1. Login to OpenAM Console as an administrative user.
2. On the Realms menu of the OpenAM console, select the realm in which the agent profile is to be managed.
3. Click the Agents link, click the tab page for the kind of agent profile you want to create, and then click the New button in the Agent table.
4. In the Name field, enter a name for the agent profile.
5. In the Password and Re-Enter Password fields, enter a password for the new agent profile.
6. Click Local or Centralized (Default) to determine where the agent properties are stored. If you select Local, the properties are stored on the server on which the agent is running. If you select Centralized, the properties are stored on the OpenAM server.
7. In the Server URL field, enter the URL to OpenAM. For example, http://openam.example.com:8080/openam.
8. In the Agent URL field, enter the primary URL of the web or application server protected by the policy agent. For Java EE policy agents, an example URL must include the agentapp context: http://shop.example.com:28080/agentapp.
9. Click Create. After creating the agent profile, you can click the link to the new profile to adjust and export the configuration.

#### 3. エージェントのインストール


#### 4. 認可ポリシーの作成など


###  Web Agentのインストールとセットアップ

**手順. Apache HTTPサーバーにApacheエージェントをインストールする**

#### 1. 事前準備(インストール要件の確認など)

まずはインストールが可能な環境であるかを確認する必要があります。「OpenAM Web Policy Agent Release Notes
Search」
の「Before You Install」のセクションを確認します。

#### 2. エージェントプロファイルの作成

To create a new Web or Java EE policy agent profile, you need to create a name and password for the agent. You also need the URLs to OpenAM and the application to protect:

1. Login to OpenAM Console as an administrative user.
2. On the Realms menu of the OpenAM console, select the realm in which the agent profile is to be managed.
3. Click the Agents link, click the tab page for the kind of agent profile you want to create, and then click the New button in the Agent table.
4. In the Name field, enter a name for the agent profile.
5. In the Password and Re-Enter Password fields, enter a password for the new agent profile.
6. Click Local or Centralized (Default) to determine where the agent properties are stored. If you select Local, the properties are stored on the server on which the agent is running. If you select Centralized, the properties are stored on the OpenAM server.
7. In the Server URL field, enter the URL to OpenAM. For example, http://openam.example.com:8080/openam.
8. In the Agent URL field, enter the primary URL of the web or application server protected by the policy agent. Note for web agents, an example URL would look like: http://www.example.com:80. For Java EE policy agents, an example URL must include the agentapp context: http://www.example.com:8080/agentapp.
9. Click Create. After creating the agent profile, you can click the link to the new profile to adjust and export the configuration.

#### 3. エージェントのインストール


#### 4. 認可ポリシーの作成など

**手順. IISサーバーにIISエージェントをインストールする**

#### 1. 事前準備(インストール要件の確認など)

まずはインストールが可能な環境であるかを確認する必要があります。

#### 2. エージェントプロファイルの作成

#### 3. エージェントのインストール


#### 4. 認可ポリシーの作成など
