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
OpenAMのBitbucketサーバーで管理されているソースコードを確認できます。

- OpenAMのソースコード(GitHub)  
https://github.com/ForgeRock/openam  
OpenAMのBitbucketサーバーで管理されているソースコードのGitHubでのミラーです。

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

    OpenAM can be configured to play the role of OpenID provider. The OpenID Connect specifications depend on OAuth 2.0, JSON Web Token, Simple Web Discovery and related specifications. The following specifications make up OpenID Connect 1.0.

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

    OpenID Connect 1.0 also provides implementer's guides for client developers.

 - OpenID Connect Basic Client Implementer's Guide 1.0
 - OpenID Connect Implicit Client Implementer's Guide 1.0

- User-Managed Access (UMA) 1.0

 - User-Managed Access (UMA) Profile of OAuth 2.0 (Draft), in which OpenAM can play the role of authorization server.
 - OAuth 2.0 Resource Set Registration, in which OpenAM plays the role of authorization server.

- Representational State Transfer (REST)

    Style of software architecture for web-based, distributed systems.

- Security Assertion Markup Language (SAML)

    Standard, XML-based framework for creating and exchanging security information between online partners. OpenAM supports multiple versions of SAML including 2.0, 1.1, and 1.0.

    Specifications are available from the OASIS standards page.

- Liberty Alliance Project Identity Federation Framework (Liberty ID-FF)

    Federation standard, whose concepts and capabilities contributed to SAML v2.0.

- Simple Object Access Protocol

    Lightweight protocol intended for exchanging structured information in a decentralized, distributed environment.

- Web Services Description Language (WSDL)

    XML format for describing network services as a set of endpoints operating on messages containing either document-oriented or procedure-oriented information.

- Web Services Federation Language (WS-Federation)

    Identity federation standard, part of the Web Services Security framework.

- eXtensible Access Control Markup Language (XACML)

    Declarative access control policy language implemented in XML, and also a processing model, describing how to interpret policies.

