[TODO 作成中]

OpenAMは、もともとantでビルドされていましたが、バージョン11.0.0からMavenに移行しています。これにより、マルチプロジェクト構成のMavenプロジェクトとなり、バージョン13.5.0では「OpenAM Project」というトッププロジェクトの配下に以下のようなサブプロジェクトを持つ構成になっています。

表. OpenAM 13.5.0でのMavenプロジェクトの構成

|ファイル名|プロジェクト名|説明|
|---|---|---|
|openam-annotations|OpenAM Annotations|OpenAMアノテーション|
|openam-audit|OpenAM Audit|OpenAM監査モジュール|
|openam-audit\openam-audit-configuration|OpenAM Audit Configuration||
|openam-audit\openam-audit-context|OpenAM Audit Context||
|openam-audit\openam-audit-core|OpenAM Audit Core||
|openam-audit\openam-audit-rest|OpenAM Audit REST||
|openam-authentication|OpenAM Authentication|OpenAMの全認証モジュール|
|openam-authentication\deviceprint|OpenAM Auth Device Print|デバイスプリント認証モジュール|
|openam-authentication\deviceprint\module|OpenAM Auth Device Print Module|デバイスプリント認証モジュール(モジュール)|
|openam-authentication\deviceprint\scripts|OpenAM Auth Device Print Module Scripts|デバイスプリント認証モジュール(スクリプト)|
|openam-authentication\openam-auth-ad|OpenAM Auth AD|Active Directory認証モジュール|
|openam-authentication\openam-auth-adaptive|OpenAM Auth Adaptive|アダプティブリスク認証モジュール|
|openam-authentication\openam-auth-anonymous|OpenAM Auth Anonymous|匿名認証モジュール|
|openam-authentication\openam-auth-application|OpenAM Auth Application|アプリケーション認証モジュール|
|openam-authentication\openam-auth-cert|OpenAM Auth Cert|証明書認証モジュール|
|openam-authentication\openam-auth-common|OpenAM Auth Common|認証モジュール共通ユーティリティ|
|openam-authentication\openam-auth-datastore|OpenAM Auth Datastore|データストア認証モジュール|
|openam-authentication\openam-auth-device-id|OpenAM Auth Device Id|デバイスID認証モジュール|
|openam-authentication\openam-auth-fr-oath|OpenAM Auth OATH|OATH認証モジュール|
|openam-authentication\openam-auth-hotp|OpenAM Auth HOTP|HOTP認証モジュール|
|openam-authentication\openam-auth-httpbasic|OpenAM Auth HttpBasic|HTTP基本認証モジュール|
|openam-authentication\openam-auth-jdbc|OpenAM Auth JDBC|JDBC認証モジュール|
|openam-authentication\openam-auth-ldap|OpenAM Auth LDAP|LDAP認証モジュール|
|openam-authentication\openam-auth-membership|OpenAM Auth Membership|メンバーシップ認証モジュール|
|openam-authentication\openam-auth-msisdn|OpenAM Auth MSISDN|MSISDN認証モジュール|
|openam-authentication\openam-auth-nt|OpenAM Auth NT|Windows NT認証モジュール|
|openam-authentication\openam-auth-oath|OpenAM Auth OATH|OATH認証モジュール|
|openam-authentication\openam-auth-oauth2|OpenAM Auth OAUTH2|OAuth 2.0認証モジュール|
|openam-authentication\openam-auth-oidc|OpenAM AuthN OpenId Connect|OpenID Connect認証モジュール|
|openam-authentication\openam-auth-persistentcookie|OpenAM Auth Persistent Cookie|持続Cookie認証モジュール|
|openam-authentication\openam-auth-radius|OpenAM Auth Radius|RADIUS認証モジュール|
|openam-authentication\openam-auth-saml2|OpenAM Auth SAML2|SAML 2.0認証モジュール|
|openam-authentication\openam-auth-scripted|OpenAM Auth Scripted|スクリプト認証モジュール|
|openam-authentication\openam-auth-securid|OpenAM Auth SecurID|SecurID認証モジュール|
|openam-authentication\openam-auth-windowsdesktopsso|OpenAM Auth WindowsDesktopSSO|WindowsデスクトップSSO認証モジュール|
|openam-cli|OpenAM CLI Base|OpenAMコマンドラインインタフェースの基盤|
|openam-cli\openam-cli-definitions|OpenAM CLI Definitions|OpenAMコマンドラインインタフェースの定義|
|openam-cli\openam-cli-impl|OpenAM CLI Implementation|OpenAMコマンドラインインタフェースの実装|
|openam-clientsdk|OpenAM Client SDK|OpenAM JavaクライアントSDK|
|openam-common-auth-ui|OpenAM Common Auth UI|OpenAM共通認証UI|
|openam-console|OpenAM Admin Console|OpenAM管理コンソール|
|openam-core|OpenAM Core|OpenAMコアコンポーネント|
|openam-core-rest|OpenAM Core REST|OpenAMコアREST|
|openam-coretoken|OpenAM Core Token|OpenAMコアトークンサービス|
|openam-dashboard|OpenAM Dashboard|OpenAMダッシュボードサービス|
|openam-datastore|OpenAM User Data Store|OpenAMユーザーデータストア|
|openam-distribution|OpenAM Distribution Packaging|OpenAMモジュールの配布パッケージ|
|openam-distribution\openam-distribution-diagnostics|OpenAM Distribution Diagnostics|診断ツール|
|openam-distribution\openam-distribution-fedlet-unconfigured|OpenAM Distribution Fedlet UnConfigured|Fedlet|
|openam-distribution\openam-distribution-kit|OpenAM Distribution Kit|OpenAM配布キット|
|openam-distribution\openam-distribution-ssoadmintools|OpenAM Distribution ssoAdminTools|SSO管理ツールキット|
|openam-distribution\openam-distribution-ssoconfiguratortools|OpenAM Distribution ssoConfiguratorTools|SSO設定ツールキット|
|openam-documentation|OpenAM Documentation||
|openam-documentation\openam-doc-log-message-ref|OpenAM Log Message Reference||
|openam-documentation\openam-doc-ssoadm-ref|OpenAM ssoadm Reference|コアドキュメントを作成/生成するためのツール|
|openam-entitlements|OpenAM Entitlements|OpenAMエンタイトルメント|
|openam-examples|OpenAM Example Projects|OpenAMサンプルプロジェクト|
|openam-examples\openam-example-clientsdk-cli|OpenAM Example ClientSDK CLI|OpenAMサンプルClientSDK CLIモジュール|
|openam-examples\openam-example-clientsdk-war|OpenAM Example ClientSDK WAR|OpenAMサンプルClientSDK WAR|
|openam-federation|OpenAM Federation|OpenAMフェデレーション|
|openam-federation\openam-federation-library|OpenAM Federation Library|OpenAMフェデレーションライブラリコンポーネント|
|openam-federation\openam-idpdiscovery|OpenAM IdP Discovery|OpenAM IdPディスカバリー|
|openam-federation\openam-idpdiscovery-war|OpenAM IdP Discovery WAR|OpenAM IdPディスカバリーWAR|
|openam-federation\OpenFM|OpenFM|OpenAMフェデレーション|
|openam-http|OpenAM HTTP|OpenAM HTTP統合|
|openam-http-client|OpenAM HTTP client|OpenAMカスタムスクリプトモジュールで使用される共通HTTPクライアント|
|openam-ldap-utils|OpenAM LDAP Utilities|OpenAM LDAPユーティリティ|
|openam-oauth|OpenAM OAuth|OAuth 1.0モジュール|
|openam-oauth2|OpenAM OAuth2|OAuth 2.0モジュール|
|openam-oauth2-common|OpenAM OAuth2 Common|OAuth 2.0共通ライブラリ|
|openam-oauth2-common\oauth2-core|OAuth2 Core|OAuth 2.0コアライブラリ|
|openam-oauth2-common\oauth2-restlet|OAuth2 Restlet Integration|HTTPの統合を提供するためのOAuth2のRestletライブラリ|
|openam-oauth2-common\openid-connect-core|OpenID Connect Core||
|openam-oauth2-common\openid-connect-restlet|OpenId Connect Restlet Integration||
|openam-oauth2-saml2|OpenAM OAuth2 SAML2 Grant Flow|OAuth2.0のSAML2.0グラントフローを実現するモジュール|
|openam-plugins|OpenAM Plugins|OpenAMのプラグイン|
|openam-plugins\openam-auth-postauthentication|OpenAM PostAuthN plugins|OpenAMポスト認証プロセスプラグイン|
|openam-radius|OpenAM RADIUS|RADIUSサーバーと共通RADIUSライブラリ|
|openam-radius\openam-radius-common|OpenAM RADIUS common library.|RADIUS共通ライブラリ|
|openam-radius\openam-radius-server|OpenAM RADIUS Server|RADIUSサーバー|
|openam-rest|OpenAM Rest|RESTサービス|
|openam-restlet|OpenAM Restlet|Restletのカスタマイズ|
|openam-schema|OpenAM Schemata|OpenAMスキーマコンポーネント|
|openam-schema\openam-diagnostics-schema|OpenAM Diagnostics Schema|診断スキーマコンポーネント|
|openam-schema\openam-dtd-schema|OpenAM DTD Schema|アイデンティティサービスDTDコンポーネント|
|openam-schema\openam-idsvcs-schema|OpenAM Identity Services Schema|アイデンティティサービスのスキーマコンポーネント|
|openam-schema\openam-jaxrpc-schema|OpenAM JAXRPC Schema|JAXRPCスキーマコンポーネント|
|openam-schema\openam-liberty-schema|OpenAM Liberty Schema|Libertyスキーマコンポーネント|
|openam-schema\openam-mib-schema|OpenAM MIB Schema|MIBスキーマコンポーネント|
|openam-schema\openam-saml2-schema|OpenAM SAML2 Schema|SAML 2.0スキーマコンポーネント|
|openam-schema\openam-wsfederation-schema|OpenAM WS Federation Schema|WS Federationスキーマコンポーネント|
|openam-schema\openam-xacml3-schema|OpenAM XACML3 Schema|XACML3スキーマ|
|openam-scripting|OpenAM Scripting|認証モジュール用のスクリプトのサポート|
|openam-selfservice|OpenAM Self Service|セルフサービス|
|openam-server|OpenAM Server|OpenAMサーバーコンポーネント|
|openam-server-auth-ui|OpenAM Server Auth UI|OpenAMサーバー認証UI|
|openam-server-only|OpenAM Server Only|OpenAMサーバーコンポーネントのみ|
|openam-shared|OpenAM Shared|OpenAM共有コンポーネントとモジュール|
|openam-slf4j|OpenAM slf4j binding||
|openam-sts|OpenAM STS||
|openam-sts\openam-client-sts|OpenAM STS Client Classes|REST-STSインスタンスを公開し、それらを消費するために必要なクラス|
|openam-sts\openam-common-sts|OpenAM STS Common|REST/SOAP STSに共通のクラス|
|openam-sts\openam-publish-sts|OpenAM STS Publish Service|REST/SOAP STSインスタンスを公開するサービス|
|openam-sts\openam-rest-sts|OpenAM REST STS|RESTセキュアトークンサービス|
|openam-sts\openam-soap-sts|OpenAM Soap STS|SOAPセキュアトークンサービス|
|openam-sts\openam-soap-sts\openam-soap-sts-client|OpenAM SOAP STS Client|WS-TrustのSecureTokenServiceクライアント|
|openam-sts\openam-soap-sts\openam-soap-sts-server|OpenAM SOAP STS Server|WS-TrustのSecureTokenServiceサーバー|
|openam-sts\openam-token-service-sts|OpenAM STS Token Service|STSによって消費されるトークンの生成と検証サービス|
|openam-test|OpenAM Common Test Suite|OpenAM共通テストスイート|
|openam-tools|OpenAM Tools|OpenAM全ツール|
|openam-tools\build-helper-plugin|OpenAM Build Helper Maven Plugin||
|openam-tools\openam-build-tools|OpenAM Build Tools|OpenAMのビルドツール|
|openam-tools\openam-configurator-tool|OpenAM Configurator Tool|OpenAMの設定ツール|
|openam-tools\openam-diagnostics|OpenAM Diagnostics|OpenAMの診断コンポーネント|
|openam-tools\openam-diagnostics\openam-diagnostics-base|OpenAM Diagnostics Base|OpenAMの診断コンポーネントの基盤|
|openam-tools\openam-diagnostics\openam-diagnostics-plugins|OpenAM Diagnostics Plugins|OpenAMの診断プラグインコンポーネント|
|openam-tools\openam-installer-utils|OpenAM Installer Utils|OpenAMのインストーラユーティリティ|
|openam-tools\openam-installtools|OpenAM Install Tools|OpenAMインストールツールコンポーネント|
|openam-tools\openam-installtools-launcher|OpenAM Install Tools Launcher|OpenAMインストールツールランチャー|
|openam-tools\openam-license-core|OpenAM License Core|OpenAMライセンスコア|
|openam-tools\openam-license-manager-cli|OpenAM CLI License Manager|OpenAMコマンドラインインターフェイスのライセンスマネージャ|
|openam-tools\openam-license-servlet|OpenAM ServletContext License Locator|OpenAMのServletContextのライセンスロケータ|
|openam-tools\openam-upgrade-tool|OpenAM Upgrade Tool|OpenAMのアップグレードツール|
|openam-ui|OpenAM UI Parent||
|openam-ui\openam-ui-ria|OpenAM RIA Web UI||
|openam-uma|OpenAM UMA|OpenAMユーザー管理アクセス|
|openam-upgrade|OpenAM Upgrade|OpenAMのアップグレードサポート|

