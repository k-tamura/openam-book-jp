※[TODO 作成中]

## ポリシーエージェントの概要

ポリシーエージェント(Policy Agent)は、OpenAMと連携して、後述する次の方式のSSOを実現するソフトウェアです。

- エージェント方式 (4.1 エージェント方式 を参照)
- リバースプロキシー方式 (4.2 リバースプロキシー方式 を参照)
- 代理認証方式 (4.3 代理認証方式 を参照)

基本的な役割は、Webサイトへのクライアント(エンドユーザーのブラウザなど)からリクエストをインターセプトして、
アクセス許可・拒否を判別することと、未認証であればログイン(認証)をさせるようにOpenAMにリダイレクトすることです。

エージェント方式の場合、
ポリシーエージェントはWebサイトを公開しているWebサーバーやJavaコンテナにインストールして、
クライアントがWebサイト上の保護されたリソースへのアクセスしようとした際に、
WebサーバーやJavaコンテナによって呼び出されるように構成します。

リバースプロキシー方式や代理認証方式の場合、
ポリシーエージェントはWebサイトを公開しているWebサーバーやJavaコンテナに直接インストールせずに、
Webサイトの前段にあるリバースプロキシーサーバーにインストールします。
クライアントがWebサイト上の保護されたリソースへのアクセスしようとした際に、
リバースプロキシーサーバーによって呼び出されるように構成します。

ポリシーエージェントは、インストールする対象となるサーバーが何であるかで、以下の2つに大別できます。

- Webエージェント
- Java EEエージェント

Webポリシーエージェントは、ApacheなどのWebサーバーにインストールされたソフトウェア(ライブラリ)で、
クライアント(エンドユーザーのブラウザなど)がWebサイト上の保護されたリソースへのアクセスを要求すると、
Webサーバーによって呼び出されるように構成されます。
OpenAM 13.0.0と同時期にリリースされたWebポリシーエージェント(バージョン4.0.0)には以下があります。

- Apache 2.2 Policy Agent
- Apache 2.4 Policy Agent
- Microsoft IIS 6 Policy Agent
- Microsoft IIS 7 Policy Agent
- Oracle iPlanet/Sun Web Server Policy Agent

Java EEエージェントは、TomcatなどのJavaコンテナにインストールされたソフトウェア(サーブレットフィルター)で、
クライアント(エンドユーザーのブラウザなど)がWebアプリケーション上の保護されたリソースへのアクセスを要求すると、
Javaコンテナによって呼び出されるように構成されます。
OpenAM 13.0の管理コンソールなどでは「J2EEエージェント」と表記されています。
OpenAM 13.0.0と同時期にリリースされたJava EEポリシーエージェント(バージョン3.5.0)には以下があります。

- Apache Tomcat 6, 7, 8 Policy Agent
- IBM WebSphere Application Server 8, 8.5 Policy Agent
- JBoss Enterprise Application Platform 6 Policy Agent
- JBoss Application Server 7 Policy Agent
- Jetty 8 (at least 8.1.13) Policy Agent
- Oracle WebLogic Server 11g, 12c Policy Agent

## ポリシーエージェントの動作原理
