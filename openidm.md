### OpenIDMの概要

OpenIDMは、エンタープライズ、クラウド、ソーシャル、モバイル、レガシーといったさまざまな環境間のシステムにおいて、ユーザー管理やユーザーアクセス管理といった統合的なプロビジョニングソリューションを提供することが可能です。
Bring multiple sources of identity together for policy and workflow based management that puts you in control of the data. 
Consume, transform and feed data to external sources in order to maintain control over identity of users, devices and things.

A modern UI experience that allows you manage your data without writing a single line of code, while standard RESTful interfaces give you the ultimate in flexibility to develop as you see fit.

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

### OpenIDMの特徴

- オープンソース：

### OpenIDMの主な機能

- ロールベースのプロビジョニング

 - ジョブ関数、タイトル、地理、などの組織のニーズと構造に基づいて、ユーザーに割り当てられたロールの作成および管理
 - 迅速に一貫したエンタイトルメントやリソースの割り当てや削除

- 高可用性

 - Enterprise features at Internet scale. This includes role-based provisioning, high availability “out of the box,” workflow synchronization (with delivery guarantees), customizable user interfaces, and password management.
 - Configure nodes for specific tasks, such as reconciliations, or share load and act as backup in the event another node becoming unavailable

- モジュール方式

 - Lightweight, modular Java architecture built on the OSGi framework enables flexible, plug-and-play services
 - Purpose-built for big-data-scale requirements across enterprise and customer-facing systems
 - Provides flexibility to use the database backend system of your choice including Relational Database, NoSQL, NewSQL, etc.
 - Dynamically update services and upgrade connectors without reconfiguration or restarting

- カスタマイズが容易、バックエンドの柔軟性

 - Backend Flexibility. OpenIDM provides big data flexibility for massively scalable provisioning— you choose the backend for your deployment: Relational Database, NoSQL, NewSQL, etc.
 - Provides single, scalable web-app styled UI using Backbone, jQuery, and Handlebars. Easy to customize using standard, out-of the-box templates.
 - Tiny Footprint. OpenIDM is a completely embeddable solution with a tiny footprint – you can even run it off a simple USB drive.

- REST API

 - Simple RESTful interfaces provides a common API for managing all core functions of user administration, synchronization, and reconciliation
 - Pluggable server-side scripting engine provides Javascript and Groovy support out of the box
 - Decoupled UI enables customized solutions and includes sample configurations and scalable user administration

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

 - Easy access to source code
 - Participate in a vibrant community
 - Transparent roadmap information