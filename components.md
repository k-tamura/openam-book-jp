[TODO 作成中]

OpenAMは、もともとantでビルドされていましたが、バージョン11.0.0からMavenでビルドされるようになってています。これにより、OpenAMはマルチプロジェクト構成のMavenプロジェクトとなり、「OpenAM Project」というトッププロジェクトの配下に以下のようなサブプロジェクトを持つ構成になっています(バージョン13.5.0での構成)。

表. OpenAM 13.5.0でのMavenプロジェクトの構成

|ディレクトリ名|プロジェクト名|説明|
|---|---|---|
|openam-annotations|OpenAM Annotations|OpenAMアノテーション|
|openam-audit|OpenAM Audit|OpenAM監査モジュール|
|openam-audit<br>\openam-audit-configuration|OpenAM Audit Configuration||
|openam-audit<br>\openam-audit-context|OpenAM Audit Context||
|openam-audit<br>\openam-audit-core|OpenAM Audit Core||
|openam-audit<br>\openam-audit-rest|OpenAM Audit REST||
|openam-authentication|OpenAM Authentication|OpenAMの全認証モジュール|
|openam-authentication<br>\deviceprint|OpenAM Auth Device Print|デバイスプリント認証モジュール|
|openam-authentication<br>\deviceprint<br>\module|OpenAM Auth Device Print Module|デバイスプリント認証モジュール(モジュール)|
|openam-authentication<br>\deviceprint<br>\scripts|OpenAM Auth Device Print Module Scripts|デバイスプリント認証モジュール(スクリプト)|
|openam-authentication<br>\openam-auth-ad|OpenAM Auth AD|Active Directory認証モジュール|
|openam-authentication<br>\openam-auth-adaptive|OpenAM Auth Adaptive|アダプティブリスク認証モジュール|
|openam-authentication<br>\openam-auth-anonymous|OpenAM Auth Anonymous|匿名認証モジュール|
|openam-authentication<br>\openam-auth-application|OpenAM Auth Application|アプリケーション認証モジュール|
|openam-authentication<br>\openam-auth-cert|OpenAM Auth Cert|証明書認証モジュール|
|openam-authentication<br>\openam-auth-common|OpenAM Auth Common|認証モジュール共通ユーティリティ|
|openam-authentication<br>\openam-auth-datastore|OpenAM Auth Datastore|データストア認証モジュール|
|openam-authentication<br>\openam-auth-device-id|OpenAM Auth Device Id|デバイスID認証モジュール|
|openam-authentication<br>\openam-auth-fr-oath|OpenAM Auth OATH|OATH認証モジュール|
|openam-authentication<br>\openam-auth-hotp|OpenAM Auth HOTP|HOTP認証モジュール|
|openam-authentication<br>\openam-auth-httpbasic|OpenAM Auth HttpBasic|HTTP基本認証モジュール|
|openam-authentication<br>\openam-auth-jdbc|OpenAM Auth JDBC|JDBC認証モジュール|
|openam-authentication<br>\openam-auth-ldap|OpenAM Auth LDAP|LDAP認証モジュール|
|openam-authentication<br>\openam-auth-membership|OpenAM Auth Membership|メンバーシップ認証モジュール|
|openam-authentication<br>\openam-auth-msisdn|OpenAM Auth MSISDN|MSISDN認証モジュール|
|openam-authentication<br>\openam-auth-nt|OpenAM Auth NT|Windows NT認証モジュール|
|openam-authentication<br>\openam-auth-oath|OpenAM Auth OATH|OATH認証モジュール|
|openam-authentication<br>\openam-auth-oauth2|OpenAM Auth OAUTH2|OAuth 2.0認証モジュール|
|openam-authentication<br>\openam-auth-oidc|OpenAM AuthN OpenId Connect|OpenID Connect認証モジュール|
|openam-authentication<br>\openam-auth-persistentcookie|OpenAM Auth Persistent Cookie|持続Cookie認証モジュール|
|openam-authentication<br>\openam-auth-radius|OpenAM Auth Radius|RADIUS認証モジュール|
|openam-authentication<br>\openam-auth-saml2|OpenAM Auth SAML2|SAML 2.0認証モジュール|
|openam-authentication<br>\openam-auth-scripted|OpenAM Auth Scripted|スクリプト認証モジュール|
|openam-authentication<br>\openam-auth-securid|OpenAM Auth SecurID|SecurID認証モジュール|
|openam-authentication<br>\openam-auth-windowsdesktopsso|OpenAM Auth WindowsDesktopSSO|WindowsデスクトップSSO認証モジュール|
|openam-cli|OpenAM CLI Base|OpenAMコマンドラインインタフェースの基盤|
|openam-cli<br>\openam-cli-definitions|OpenAM CLI Definitions|OpenAMコマンドラインインタフェースの定義|
|openam-cli<br>\openam-cli-impl|OpenAM CLI Implementation|OpenAMコマンドラインインタフェースの実装|
|openam-clientsdk|OpenAM Client SDK|OpenAM JavaクライアントSDK|
|openam-common-auth-ui|OpenAM Common Auth UI|OpenAM共通認証UI|
|openam-console|OpenAM Admin Console|OpenAM管理コンソール|
|openam-core|OpenAM Core|OpenAMコアコンポーネント|
|openam-core-rest|OpenAM Core REST|OpenAMコアREST|
|openam-coretoken|OpenAM Core Token|OpenAMコアトークンサービス|
|openam-dashboard|OpenAM Dashboard|OpenAMダッシュボードサービス|
|openam-datastore|OpenAM User Data Store|OpenAMユーザーデータストア|
|openam-distribution|OpenAM Distribution Packaging|OpenAMモジュールの配布パッケージ|
|openam-distribution<br>\openam-distribution-diagnostics|OpenAM Distribution Diagnostics|診断ツール|
|openam-distribution<br>\openam-distribution-fedlet-unconfigured|OpenAM Distribution Fedlet UnConfigured|Fedlet|
|openam-distribution<br>\openam-distribution-kit|OpenAM Distribution Kit|OpenAM配布キット|
|openam-distribution<br>\openam-distribution-ssoadmintools|OpenAM Distribution ssoAdminTools|SSO管理ツールキット|
|openam-distribution<br>\openam-distribution-ssoconfiguratortools|OpenAM Distribution ssoConfiguratorTools|SSO設定ツールキット|
|openam-documentation|OpenAM Documentation||
|openam-documentation<br>\openam-doc-log-message-ref|OpenAM Log Message Reference||
|openam-documentation<br>\openam-doc-ssoadm-ref|OpenAM ssoadm Reference|コアドキュメントを作成/生成するためのツール|
|openam-entitlements|OpenAM Entitlements|OpenAMエンタイトルメント|
|openam-examples|OpenAM Example Projects|OpenAMサンプルプロジェクト|
|openam-examples<br>\openam-example-clientsdk-cli|OpenAM Example ClientSDK CLI|OpenAMサンプルClientSDK CLIモジュール|
|openam-examples<br>\openam-example-clientsdk-war|OpenAM Example ClientSDK WAR|OpenAMサンプルClientSDK WAR|
|openam-federation|OpenAM Federation|OpenAMフェデレーション|
|openam-federation<br>\openam-federation-library|OpenAM Federation Library|OpenAMフェデレーションライブラリコンポーネント|
|openam-federation<br>\openam-idpdiscovery|OpenAM IdP Discovery|OpenAM IdPディスカバリー|
|openam-federation<br>\openam-idpdiscovery-war|OpenAM IdP Discovery WAR|OpenAM IdPディスカバリーWAR|
|openam-federation<br>\OpenFM|OpenFM|OpenAMフェデレーション|
|openam-http|OpenAM HTTP|OpenAM HTTP統合|
|openam-http-client|OpenAM HTTP client|OpenAMカスタムスクリプトモジュールで使用される共通HTTPクライアント|
|openam-ldap-utils|OpenAM LDAP Utilities|OpenAM LDAPユーティリティ|
|openam-oauth|OpenAM OAuth|OAuth 1.0モジュール|
|openam-oauth2|OpenAM OAuth2|OAuth 2.0モジュール|
|openam-oauth2-common|OpenAM OAuth2 Common|OAuth 2.0共通ライブラリ|
|openam-oauth2-common<br>\oauth2-core|OAuth2 Core|OAuth 2.0コアライブラリ|
|openam-oauth2-common<br>\oauth2-restlet|OAuth2 Restlet Integration|HTTPの統合を提供するためのOAuth2のRestletライブラリ|
|openam-oauth2-common<br>\openid-connect-core|OpenID Connect Core||
|openam-oauth2-common<br>\openid-connect-restlet|OpenId Connect Restlet Integration||
|openam-oauth2-saml2|OpenAM OAuth2 SAML2 Grant Flow|OAuth2.0のSAML2.0グラントフローを実現するモジュール|
|openam-plugins|OpenAM Plugins|OpenAMのプラグイン|
|openam-plugins<br>\openam-auth-postauthentication|OpenAM PostAuthN plugins|OpenAMポスト認証プロセスプラグイン|
|openam-radius|OpenAM RADIUS|RADIUSサーバーと共通RADIUSライブラリ|
|openam-radius<br>\openam-radius-common|OpenAM RADIUS common library.|RADIUS共通ライブラリ|
|openam-radius<br>\openam-radius-server|OpenAM RADIUS Server|RADIUSサーバー|
|openam-rest|OpenAM Rest|RESTサービス|
|openam-restlet|OpenAM Restlet|Restletのカスタマイズ|
|openam-schema|OpenAM Schemata|OpenAMスキーマコンポーネント|
|openam-schema<br>\openam-diagnostics-schema|OpenAM Diagnostics Schema|診断スキーマコンポーネント|
|openam-schema<br>\openam-dtd-schema|OpenAM DTD Schema|アイデンティティサービスDTDコンポーネント|
|openam-schema<br>\openam-idsvcs-schema|OpenAM Identity Services Schema|アイデンティティサービスのスキーマコンポーネント|
|openam-schema<br>\openam-jaxrpc-schema|OpenAM JAXRPC Schema|JAXRPCスキーマコンポーネント|
|openam-schema<br>\openam-liberty-schema|OpenAM Liberty Schema|Libertyスキーマコンポーネント|
|openam-schema<br>\openam-mib-schema|OpenAM MIB Schema|MIBスキーマコンポーネント|
|openam-schema<br>\openam-saml2-schema|OpenAM SAML2 Schema|SAML 2.0スキーマコンポーネント|
|openam-schema<br>\openam-wsfederation-schema|OpenAM WS Federation Schema|WS Federationスキーマコンポーネント|
|openam-schema<br>\openam-xacml3-schema|OpenAM XACML3 Schema|XACML3スキーマ|
|openam-scripting|OpenAM Scripting|認証モジュール用のスクリプトのサポート|
|openam-selfservice|OpenAM Self Service|セルフサービス|
|openam-server|OpenAM Server|OpenAMサーバーコンポーネント|
|openam-server-auth-ui|OpenAM Server Auth UI|OpenAMサーバー認証UI|
|openam-server-only|OpenAM Server Only|OpenAMサーバーコンポーネントのみ|
|openam-shared|OpenAM Shared|OpenAM共有コンポーネントとモジュール|
|openam-slf4j|OpenAM slf4j binding||
|openam-sts|OpenAM STS||
|openam-sts<br>\openam-client-sts|OpenAM STS Client Classes|REST-STSインスタンスを公開し、それらを消費するために必要なクラス|
|openam-sts<br>\openam-common-sts|OpenAM STS Common|REST/SOAP STSに共通のクラス|
|openam-sts<br>\openam-publish-sts|OpenAM STS Publish Service|REST/SOAP STSインスタンスを公開するサービス|
|openam-sts<br>\openam-rest-sts|OpenAM REST STS|RESTセキュアトークンサービス|
|openam-sts<br>\openam-soap-sts|OpenAM Soap STS|SOAPセキュアトークンサービス|
|openam-sts<br>\openam-soap-sts<br>\openam-soap-sts-client|OpenAM SOAP STS Client|WS-TrustのSecureTokenServiceクライアント|
|openam-sts<br>\openam-soap-sts<br>\openam-soap-sts-server|OpenAM SOAP STS Server|WS-TrustのSecureTokenServiceサーバー|
|openam-sts<br>\openam-token-service-sts|OpenAM STS Token Service|STSによって消費されるトークンの生成と検証サービス|
|openam-test|OpenAM Common Test Suite|OpenAM共通テストスイート|
|openam-tools|OpenAM Tools|OpenAM全ツール|
|openam-tools<br>\build-helper-plugin|OpenAM Build Helper Maven Plugin||
|openam-tools<br>\openam-build-tools|OpenAM Build Tools|OpenAMのビルドツール|
|openam-tools<br>\openam-configurator-tool|OpenAM Configurator Tool|OpenAMの設定ツール|
|openam-tools<br>\openam-diagnostics|OpenAM Diagnostics|OpenAMの診断コンポーネント|
|openam-tools<br>\openam-diagnostics<br>\openam-diagnostics-base|OpenAM Diagnostics Base|OpenAMの診断コンポーネントの基盤|
|openam-tools<br>\openam-diagnostics<br>\openam-diagnostics-plugins|OpenAM Diagnostics Plugins|OpenAMの診断プラグインコンポーネント|
|openam-tools<br>\openam-installer-utils|OpenAM Installer Utils|OpenAMのインストーラユーティリティ|
|openam-tools<br>\openam-installtools|OpenAM Install Tools|OpenAMインストールツールコンポーネント|
|openam-tools<br>\openam-installtools-launcher|OpenAM Install Tools Launcher|OpenAMインストールツールランチャー|
|openam-tools<br>\openam-license-core|OpenAM License Core|OpenAMライセンスコア|
|openam-tools<br>\openam-license-manager-cli|OpenAM CLI License Manager|OpenAMコマンドラインインターフェイスのライセンスマネージャ|
|openam-tools<br>\openam-license-servlet|OpenAM ServletContext License Locator|OpenAMのServletContextのライセンスロケータ|
|openam-tools<br>\openam-upgrade-tool|OpenAM Upgrade Tool|OpenAMのアップグレードツール|
|openam-ui|OpenAM UI Parent||
|openam-ui<br>\openam-ui-ria|OpenAM RIA Web UI||
|openam-uma|OpenAM UMA|OpenAMユーザー管理アクセス|
|openam-upgrade|OpenAM Upgrade|OpenAMのアップグレードサポート|

