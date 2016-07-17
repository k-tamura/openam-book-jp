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
ForgeRock社の社員の方のブログへのリンクや、OpenAMのダウンロードページ、脆弱性情報のページなどが公開されています。

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
 - JSON Web Key (JWK)
 - JSON Web Algorithms (JWA)
 - JSON Web Token (JWT)
 - Security Assertion Markup Language (SAML) 2.0 Profile for OAuth 2.0 Client Authentication and Authorization Grants.
 - JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants.

- OpenID Connect 1.0

    OpenAM can be configured to play the role of OpenID provider. The OpenID Connect specifications depend on OAuth 2.0, JSON Web Token, Simple Web Discovery and related specifications. The following specifications make up OpenID Connect 1.0.

 - OpenID Connect Core 1.0 defines core OpenID Connect 1.0 features.
 - OpenID Connect Discovery 1.0 defines how clients can dynamically recover information about OpenID providers.
 - OpenID Connect Dynamic Client Registration 1.0 defines how clients can dynamically register with OpenID providers.
 - OpenID Connect Session Management 1.0 describes how to manage OpenID Connect sessions, including logout.
 - OAuth 2.0 Multiple Response Type Encoding Practices defines additional OAuth 2.0 response types used in OpenID Connect.
 - OAuth 2.0 Form Post Response Mode defines how OpenID providers return OAuth 2.0 Authorization Response parameters in auto-submitting forms.

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

