### OpenAM関連のドキュメント

- OpenAMのコアドキュメント  
http://openam.forgerock.org/docs.html    
ForgeRock社は、以下のドキュメントを公式に公開しています。

 - OpenAM入門    
https://backstage.forgerock.com/#!/docs/openam/13/getting-started/toc

 - OpenAM管理ガイド  
https://backstage.forgerock.com/#!/docs/openam/13/admin-guide/toc
 
 - OpenAMのAPIのJavadoc  
https://backstage.forgerock.com/static/docs/openam/13/apidocs

 - OpenAM配備計画ガイド  
https://backstage.forgerock.com/#!/docs/openam/13/deployment-planning/toc

 - OpenAM開発者ガイド  
https://backstage.forgerock.com/#!/docs/openam/13/dev-guide/toc

 - OpenAMインストールガイド  
https://backstage.forgerock.com/#!/docs/openam/13/install-guide/toc

 - OpenAMリファレンス  
https://backstage.forgerock.com/#!/docs/openam/13/reference/toc

 - OpenAMリリースノート  
https://backstage.forgerock.com/#!/docs/openam/13/release-notes/toc

 - OpenAM アップグレードガイド  
https://backstage.forgerock.com/#!/docs/openam/13/upgrade-guide/toc

- OpenAM Wiki  
https://wikis.forgerock.org/confluence/display/openam/Home  
ForgeRock社のOpenAMのWikiです。
Office 365との連携方法など、管理者ガイドや開発者ガイドなどではまとめづらい、実践的なTips集も掲載されています。
ロードマップも公開されています。

- OpenAM JIRA  
https://bugster.forgerock.org/jira/browse/OPENAM  
OpenAMのJIRA(課題管理システム)。バグやエンハンス要望などが載っています。

- ForgeRockのコミュニティサイト  
https://forgerock.org/  
ForgeRockの開発者のブログへのリンクや、OpenAMのダウンロードページ、脆弱性情報、QAなどが公開されています。

- ForgeRockコミュニティのMLのアーカイブ  
https://lists.forgerock.org  
ForgeRockコミュニティのメーリングリストのアーカイブです。

- OpenAMのソースコード(Bitbucket)  
https://stash.forgerock.org/projects/OPENAM/repos/openam/browse/  
Bitbucketサーバーで管理されているOpenAMのソースコードを確認できます。

- OpenAMのソースコード(GitHub)  
https://github.com/ForgeRock/openam  
Bitbucketサーバーで管理されているOpenAMのソースコードのGitHubでのミラーです。

### 各種プロトコルの仕様

OpenAMは、次のRFC、インターネットドラフト、標準を実装しています:

- OAuth 2.0  
  http://oauth.net/2/

 - The OAuth 2.0 Authorization Framework  
   http://tools.ietf.org/html/rfc6749  
   この仕様において、OpenAMは認可サーバーとクライアントの役割を担うことができます。

 - The OAuth 2.0 Authorization Framework: Bearer Token Usage  
   http://tools.ietf.org/html/rfc6750  
   この仕様において、OpenAMは認可サーバーの役割を担うことができます。

 - JSON Web Signature (JWS)  
   http://tools.ietf.org/html/rfc7515

 - JSON Web Key (JWK)  
   http://tools.ietf.org/html/rfc7517

 - JSON Web Algorithms (JWA)  
   http://tools.ietf.org/html/rfc7518

 - JSON Web Token (JWT)  
   http://tools.ietf.org/html/rfc7519

 - Security Assertion Markup Language (SAML) 2.0 Profile for OAuth 2.0 Client Authentication and Authorization Grants.  
   http://tools.ietf.org/html/rfc7522

 - JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants.  
   http://tools.ietf.org/html/rfc7523

- OpenID Connect 1.0  
  http://openid.net/connect/

    OpenAMは、OpenID Connect 1.0のOpenIDプロバイダの役割を果たすように設定することができます。OpenID Connectの仕様は、OAuth 2.0、JWT(JSON Webトークン)、Simple Web Discoveryおよびそれらに関連する仕様に依存しています。以下の仕様はOpenID Connect 1.0を構成しています。

 - OpenID Connect Core 1.0  
   http://openid.net/specs/openid-connect-core-1_0.html  
   OpenID Connect 1.0のコアとなる機能を定義しています。

 - OpenID Connect Discovery 1.0  
   http://openid.net/specs/openid-connect-discovery-1_0.html  
   クライアントが、動的にOpenIDプロバイダの情報を検索できる方法を定義しています。

 - OpenID Connect Dynamic Client Registration 1.0  
   http://openid.net/specs/openid-connect-registration-1_0.html 
   クライアントが動的にはOpenIDプロバイダに登録できる方法を定義しています。

 - OpenID Connect Session Management 1.0  
   http://openid.net/specs/openid-connect-session-1_0.html 
   ログアウトを含む、OpenID Connectのセッションを管理する方法について説明しています。

 - OAuth 2.0 Multiple Response Type Encoding Practices  
   http://openid.net/specs/oauth-v2-multiple-response-types-1_0.html  
   OpenID Connectで使用される追加のOAuth 2.0の応答タイプを定義しています。

 - OAuth 2.0 Form Post Response Mode  
   http://openid.net/specs/oauth-v2-form-post-response-mode-1_0.html  
   自動サブミットするフォームで、OAuth2.0認可応答のパラメータをOpenIDプロバイダが返す方法を定義しています。

   OpenID Connect 1.0は、クライアント開発者のための実装者のガイドも提供しています。

 - OpenID Connect Basic Client Implementer's Guide 1.0  
   http://openid.net/specs/openid-connect-basic-1_0.html
   
 - OpenID Connect Implicit Client Implementer's Guide 1.0  
   http://openid.net/specs/openid-connect-implicit-1_0.html

- User-Managed Access (UMA) 1.0

 - User-Managed Access (UMA) Profile of OAuth 2.0 (Draft)  
   https://tools.ietf.org/html/draft-hardjono-oauth-umacore-13  
   この中でOpenAMは、認可サーバーの役割を果たすことができます。
   
 - OAuth 2.0 Resource Set Registration  
   https://docs.kantarainitiative.org/uma/draft-oauth-resource-reg-v1_0_1.html  
   この中でOpenAMは、認可サーバーの役割を果たすことができます。

- Representational State Transfer (REST)  
  http://en.wikipedia.org/wiki/Representational_state_transfer  
  Webベースで、分散システムのためのソフトウェアアーキテクチャのスタイル。

- Security Assertion Markup Language (SAML)  
  オンラインパートナー間でセキュリティ情報の作成や交換をするためのXMLベースのフレームワーク標準規格。OpenAMは、2.0、1.1、および1.0を含むSAMLの複数のバージョンをサポートしています。  
  仕様は、OASIS標準のページから入手できます。  
  https://www.oasis-open.org/standards

- Liberty Alliance Project Identity Federation Framework (Liberty ID-FF)  
  http://projectliberty.org/resource_center/specifications/liberty_alliance_id_ff_1_2_specifications/?f=resource_center/specifications/liberty_alliance_id_ff_1_2_specifications  
  フェデレーション標準規格の一つで、その概念と機能はSAML v2.0へと寄与された。

- Simple Object Access Protocol (SOAP)  
  https://www.w3.org/TR/soap/  
  分散環境で構造化情報を交換するための軽量なプロトコル。

- Web Services Description Language (WSDL)  
  http://www.w3.org/TR/wsdl  
  ドキュメント指向または手続き指向の情報を含むメッセージで動作する、一連のエンドポイントなどのネットワークサービスを記述するためのXMLフォーマット。

- Web Services Federation Language (WS-Federation)  
  https://en.wikipedia.org/wiki/WS-Federation  
  アイデンティティフェデレーション標準(Webサービスセキュリティフレームワークの一部)。

- eXtensible Access Control Markup Language (XACML)  
  https://wiki.oasis-open.org/xacml/  
  XMLで実装された宣言型のアクセス制御ポリシー言語であり、ポリシーを解釈する方法を説明する処理モデル。
