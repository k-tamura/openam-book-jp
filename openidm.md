### OpenIDMの概要

OpenIDMは、さまざまな環境のシステムに対して、統合的なプロビジョニングソリューションを提供します。

表. OpenIDMの基本情報  

<table>
  <tbody>
    <tr>
      <td>機能</td>
      <td>アイデンティティ情報のプロビジョニングおよびライフサイクル管理</td>
    </tr>
    <tr>
      <td>サポートするOS</td>
      <td>マルチプラットフォーム(Windows、Mac OS X、Linux)</td>
    </tr>
    <tr>
      <td>対応言語</td>
      <td>マルチランゲージ（日本語にも対応）</td>
    </tr>
    <tr>
      <td>実装言語</td>
      <td>メインはJava（C#やJavaScript、Groovyなども含む）</td>
    </tr>
    <tr>
      <td>ライセンス</td>
      <td>CDDL</td>
    </tr>
    <tr>
      <td>Webサイト</td>
      <td>http://openidm.forgerock.org/</td>
    </tr>
  </tbody>
</table>

### OpenIDMの特徴と主な機能

- ロールベースのプロビジョニング

 - ジョブ関数、タイトル、地理、などの組織のニーズと構造に基づいて、ユーザーに割り当てられたロールの作成および管理
 - 迅速に一貫したエンタイトルメントやリソースの割り当てや削除

- 高可用性

 - インターネットスケールでのエンタープライズ機能。ロールベースのプロビジョニング、高可用性のあるワークフローの同期、カスタマイズ可能なユーザーインターフェース、パスワード管理を含む。
 - 特定のタスク(リコンシリエーションなどの)または負荷の共有のためのノードを設定し、別のノードが使用できなくなってきた場合にバックアップとして機能

- モジュール方式

 - OSGiフレームワーク上に構築された軽量なモジュラー型のJavaアーキテクチャは、柔軟でプラグアンドプレイのサービスを可能にする
 - ビッグデータ規模のエンタープライズ要件にも対応可能
 - RDBMSの他に、NoSQL、NewSQLなど、好みのデータベースバックエンドシステムを使用する柔軟性を提供。
 - 再設定または再起動をせず、動的にサービスを更新、コネクタをアップグレード

- 容易なカスタマイズ、柔軟なバックエンド

 - バックエンドの柔軟性。 OpenIDMは、大規模でスケーラブルなプロビジョニングのために、RDBMS、NoSQL、NewSQLなど様々なバックエンドを選択できます。
  - Backbone、jQuery、Handlebarsを使用して、単一で、スケーラブルなWebアプリスタイルのUIを提供します。標準的なテンプレートを使用して簡単かつ迅速にカスタマイズできます。
  - 少ないリソース消費。 OpenIDMは、少ないリソース消費で完全に組み込み可能なソリューションです。例えば、シンプルなUSBドライブでもそれを実行することができます。

- REST API

 - シンプルでRESTfulなインターフェースは、ユーザー管理、同期、リコンシリエーションのすべてのコア機能を管理するための、共通APIを提供します。
 - プラグイン可能なサーバーサイドスクリプトエンジンは、JavascriptとGroovyのサポートを提供しています。
 - 疎結合のUIは、カスタマイズされたソリューションを可能にし、サンプル構成でスケーラブルなユーザ管理を含みます。

- 柔軟なデータモデル

 - Select the data model that best fits your deployment: a data full model for faster access or data sparse model for current data
 - An open object-based model that is not hard-coded – provides the flexibility to define different schema, objects, attributes, and relations

- 組み込みアーキテクチャ

 - Encapsulated implementation that used standard REST based interfaces and Java development tools such as Eclipse, NetBeans and Spring for simple, reproducible deployments

- パスワード管理

 - Enables fine control password management to ensure consistency across all applications and data stores, such as Active Directory and HR systems
 - Password policies enforce access rights with rules that can specify strength, aging, reuse, and attribute validation
 - Ability to intercept and synchronize passwords changed natively on OpenDJ and Active Directory over an encrypted channel

- クラウドサービス

 - Provides simple access to cloud-based systems and resources to provision changes and aggregate data
 - Easy to configure with RESTful APIs alleviating the need for complex, time-consuming customization
 - Supports pre-configured profiles for cloud service providers such as Google Apps and salesforce.com, among others

- 同期とリコンシリエーション

 - Synchronization delivery guarantees enable roll back if one or more remote systems become unavailable
 - Synchronization utilizing either on-demand or scheduled resource comparisons
 - Reconciliation discovers new, changed, deleted, or orphaned accounts to determine user access privileges
 - Detect and synchronize changes to accounts, entitlements, and passwords, and perform user access remediation tasks
 - Allows internal business process to drive custom methods, workflows, or rules

- ロギング、監査、レポート

 - OpenIDM logs all internal activities as well as those within connected systems – account changes, access requests, etc.
 - Easily export data in standard formats for consumption by third-party reporting tools

- ユーザーセルフサービス

 - User self-service significantly reduces helpdesk costs and increases user
 - productivity by automating password reset and enforcing an audit-able centralized password policy
 - Streamlines registration and access requests to external source such as applications, and logs request events for auditing
 - Out-of-the-box end user self-service and registration UI that can be easily customized

- ワークフローエンジン

 - Provides workflow-driven provisioning and deprovisioning activities, whether for self-service actions such as requests for access, or for admin actions such as updating entitlements, on/off boarding, bulk sunrise or sunset enrollments, handling approvals with escalations
 - Embedded Activiti module that includes many different workflow templates and can be used for modeling, testing, and deployment
 - Industry-standard BPMN 2.0 process definition models, can be easily created and edited using any BPMN graphical editor, or execute on any BPMN 2.0-compliant engine

- OpenICFコネクタフレームワーク

 - OpenIDM leverages the new OpenICF 1.4 framework (Open Source Identity Connector Framework) for connector development
 - New OpenICF Cloud connectors include a Generic Scripted Connector (allows integration with anything that Groovy supports; REST, SOAP, JDBC, JSON etc)
 - PowerShell Connector allows you to write and consume PowerShell and PowerShell Cmdlets for simplified integration with Microsoft technologies such as Office 365 and Exchange.

- 100％オープンソース

 - ソースコードへの簡単なアクセス
 - 誰でも参加できる活気のあるコミュニティ
 - 透明なロードマップ
