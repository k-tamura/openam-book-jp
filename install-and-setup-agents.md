## Policy Agentのインストールとセットアップ

Policy Agentを保護対象のアプリケーションのあるサーバーにインストールして、OpenAMと連携できるようにするには、以下の作業が必要です。

1. 事前準備(インストール要件の確認など)
2. エージェントプロファイルの作成
3. エージェントのインストール
4. 認可ポリシーの作成など

###  Java EE Agentのインストールとセットアップ

**手順. TomcatサーバーにTomcatエージェントをインストールする**

#### 1. 事前準備(インストール要件の確認など)

まずはインストールが可能な環境であるかを確認する必要があります。

#### 2. エージェントプロファイルの作成

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

#### 3. エージェントのインストール


#### 4. 認可ポリシーの作成など


###  Web Agentのインストールとセットアップ

**手順. Apache HTTPサーバーにApacheエージェントをインストールする**

#### 1. 事前準備(インストール要件の確認など)

まずはインストールが可能な環境であるかを確認する必要があります。

#### 2. エージェントプロファイルの作成

#### 3. エージェントのインストール


#### 4. 認可ポリシーの作成など

**手順. IISサーバーにIISエージェントをインストールする**

#### 1. 事前準備(インストール要件の確認など)

まずはインストールが可能な環境であるかを確認する必要があります。

#### 2. エージェントプロファイルの作成

#### 3. エージェントのインストール


#### 4. 認可ポリシーの作成など
