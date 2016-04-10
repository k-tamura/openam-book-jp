* アクセス制御  
リソースへのアクセス許可または拒否を制御します。

* アカウントロックアウト  
連続した認証失敗後に、一時的または恒久的にアカウントを非アクティブにする行為。

* アクション  
認可された対象がリソースに何ができるかを示す動詞。ポリシーの一部として定義されます。

* アドバイス  
アクセスを拒否するポリシー決定のコンテキストにおいて、対応策についてのポリシー実施ポイントへのヒントはそれがアクセスを許可する決定をもたらす可能性があります。

* エージェント管理者  
ポリシーエージェントプロファイルの設定情報の読み書きができる権限を持つユーザー。一般的にポリシーエージェントをインストールするユーザーに、ポリシーエージェントプロファイルの作成を委任するために作成されます。

* エージェント認証 (Agent authenticator)  
同じレルム内に定義された複数のエージェントプロファイルへの読み取り専用アクセス権を持つエンティティ;エージェントがWebサービスプロファイルを読み取ることができるようにします。

* アプリケーション (Application)  
一般的には、保護されたリソースを公開するサービス。
OpenAMのポリシーの文脈では、アプリケーションは保護されたリソースへのアクセスを管理するポリシーを制約するテンプレートです。アプリケーションは、0個以上のポリシーを持つことができます。

* アプリケーションタイプ (Application type)  
アプリケーションタイプは、ポリシーアプリケーションを作成するためのテンプレートとして機能します。
アプリケーションタイプは、ポリシーの検索やリソースコンパレータのロジックのようなアクションや機能のロジックのプリセットリストを定義します。
アプリケーションタイプは、アプリケーションのための内部の正規化、索引付けのロジック、コンパレータのロジックを定義します。

* 属性ベースのアクセス制御 (Attribute-based access control: ABAC)  
ユーザーが何才か、またはユーザーがお金を使ってくれる顧客であるか否かというような、ユーザーの属性に基づくアクセス制御。

* 認証 (Authentication)  
主体のアイデンティティを確認する行為。

* 認証連鎖  
正常に認証するために主体が交渉しなければならない一連の認証モジュール群。

* 認証レベル  
認証モジュールに関連付けられた正の整数。通常、特別な保護を必要とするリソースを要求する際に、より厳格な認証施策の成功を必要とするために使用されます。

* 認証モジュール  
資格情報(例えば、IDとパスワード)の取得と検証を行うOpenAMの認証の単位。

* 認可  
リソースへのアクセスを付与するか拒否するかどうかを決定する行為。

* 認可サーバー  
OAuth 2.0 では、リソースオーナーを認証し、保護されたリソースにクライアントがアクセスすることをリソースオーナーが認可することを確認した後、クライアントにアクセストークンを発行します。OAuth 2.0の認可フレームワークにおいて、OpenAMはこの役割を担うことができます。

* 自動フェデレーション  
異なるプロバイダ間で共有される主体のプロファイルの共通の属性値に基づいて、主体のアイデンティティを自動的に連携する仕組み。

* バルクフェデレーション  
サービスプロバイダとアイデンティティプロバイダの両方に存在するユーザー識別子のリストに基づいて、両者間のユーザープロファイルを恒久的に連携するバッチジョブ。

* トラストサークル  
SAML v2.0によるプロバイダフェデレーションに参加するため、相互に信頼することに合意したプロバイダのグループ。少なくとも1つのアイデンティティプロバイダを含む必要がある。

* Client  
In OAuth 2.0, requests protected web resources on behalf of the resource owner given the owner's authorization. OpenAM can play this role in the OAuth 2.0 authorization framework.

* Conditions  
Defined as part of policies, these determine the circumstances under which which a policy applies.  
Environmental conditions reflect circumstances like the client IP address, time of day, how the subject authenticated, or the authentication level achieved.  
Subject conditions reflect characteristics of the subject like whether the subject authenticated, the identity of the subject, or claims in the subject's JWT.

* Configuration datastore  
LDAP directory service holding OpenAM configuration data.

* クロスドメインシングルサインオン (Cross-domain single sign-on: CDSSO)  
OpenAM capability allowing single sign-on across different DNS domains.

* Delegation  
Granting users administrative privileges with OpenAM.

* Entitlement  
Decision that defines which resource names can and cannot be accessed for a given subject in the context of a particular application, which actions are allowed and which are denied, and any related advice and attributes.

* Extended metadata  
Federation configuration information specific to OpenAM.  
Extensible Access Control Markup Language (XACML)  
Standard, XML-based access control policy language, including a processing model for making authorization decisions based on policies.

* Federation  
Standardized means for aggregating identities, sharing authentication and authorization data information between trusted providers, and allowing principals to access services across different providers without authenticating repeatedly.

* Fedlet  
Service provider application capable of participating in a circle of trust and allowing federation without installing all of OpenAM on the service provider side; OpenAM lets you create both .NET and Java Fedlets.

* Hot swappable  
Refers to configuration properties for which changes can take effect without restarting the container where OpenAM runs.

* Identity  
Set of data that uniquely describes a person or a thing such as a device or an application.

* Identity federation  
Linking of a principal's identity across multiple providers.

* Identity provider (IdP)  
Entity that produces assertions about a principal (such as how and when a principal authenticated, or that the principal's profile has a specified attribute value).

* Identity repository  
Data store holding user profiles and group information; different identity repositories can be defined for different realms.

* Java EE ポリシーエージェント  
ポリシーエージェントとして機能するWebコンテナにインストールされるJava Webアプリケーション。アプリケーションリソースのURLに基づいたポリシーで、コンテナ内の他のアプリケーションへのリクエストをフィルタリングします。

* メタデータ  
プロバイダのフェデレーションの設定情報。

* ポリシー  
Set of rules that define who is granted access to a protected resource when, how, and under what conditions.

* ポリシーエージェント  
Agent that intercepts requests for resources, directs principals to OpenAM for authentication, and enforces policy decisions from OpenAM.

* Policy Administration Point (PAP)  
Entity that manages and stores policy definitions.

* Policy Decision Point (PDP)  
Entity that evaluates access rights and then issues authorization decisions.

* Policy Enforcement Point (PEP)  
Entity that intercepts a request for a resource and then enforces policy decisions from a PDP.

* Policy Information Point (PIP)  
Entity that provides extra information, such as user profile attributes that a PDP needs in order to make a decision.

* Principal  
Represents an entity that has been authenticated (such as a user, a device, or an application), and thus is distinguished from other entities.  
When a See also Subject successfully authenticates, OpenAM associates the Subject with the Principal.

* Privilege  
In the context of delegated administration, a set of administrative tasks that can be performed by specified subjects in a given realm.

* Provider federation  
Agreement among providers to participate in a circle of trust.

* Realm  
OpenAM unit for organizing configuration and identity information.  
Realms can be used for example when different parts of an organization have different applications and user data stores, and when different organizations use the same OpenAM deployment.  
Administrators can delegate realm administration. The administrator assigns administrative privileges to users, allowing them to perform administrative tasks within the realm.

* Resource  
Something a user can access over the network such as a web page.  
Defined as part of policies, these can include wildcards in order to match multiple actual resources.

* Resource owner  
In OAuth 2.0, entity who can authorize access to protected web resources, such as an end user.

* Resource server  
In OAuth 2.0, server hosting protected web resources, capable of handling access tokens to respond to requests for such resources.

* Response attributes  
Defined as part of policies, these allow OpenAM to return additional information in the form of "attributes" with the response to a policy decision.

* Role based access control (RBAC)  
Access control that is based on whether a user has been granted a set of permissions (a role).

* セキュリティアサーションマークアップランゲージ (SAML)  
アイデンティティプロバイダとサービスプロバイダの間の認証および認可データを交換するための標準的なXMLベースの言語。

* サービスプロバイダ (SP)  
主体についてのアサーションを消費する(および主体がアクセスしようとしているサービスを提供する)エンティティ。

* セッション  
OpenAMを介したユーザー認証で開始し、ユーザーのログアウト、またはそれを切断して終了するまでの間隔。ブラウザベースのクライアントの場合、OpenAMは、セッションCookieを設定することによって、1つ以上のアプリケーション間のユーザーセッションを管理します。ステートフルセッションとステートレスセッションを参照してください。

* セッションフェイルオーバー (SFO)  
最初に主体を認証したOpenAMサーバーがオフラインになったときに、この機能は別のOpenAMサーバーがセッションを管理することを可能にします。

* セッショントークン  
認証が成功した後、OpenAMによって発行される一意の識別子。ステートフルセッションの場合、セッショントークンは主体のセッションを追跡するために使用されます。

* シングルログアウト (SLO)  
主体が一回でセッションを終了できるようにする機能。これにより、複数のアプリケーション間で主体のセッションが終了します。

* シングルサインオン (SSO)  
主体が一回認証を受けると、再び認証を受けることなく、複数のアプリケーションへのアクセスを獲得することができる機能。

* サイト  
ロードバランサーのレイヤーを介してアクセスするように、同等の設定がされたOpenAMサーバーのグループ。
ロードバランサーは、サービスレベルの可用性を提供するために、フェールオーバーを処理します。サイト内のクロストークを最小限に抑えるために、amlbcookie値に基づいたスティッキーロードバランシングを使用します。
ロードバランサーは、OpenAMサービスを保護するために使用することもできます。

* 標準メタデータ  
他のアクセス管理ソフトウェアと共有することができる標準的なフェデレーションの設定情報。

* ステートフルセッション  
OpenAMサーバーのメモリに常駐する、または(セッションフェイルオーバーが有効になっている場合は)コアトークンサービスのトークンストアに保持されるOpenAMのセッション。OpenAMは、ログアウトやタイムアウトなどのイベントを処理するために、またはセッション制約を可能にするために、またはセッションが終了した際にSSOに関与するアプリケーションに通知するために、ステートフルセッションを追跡します。

* ステートレスセッション  
OpenAMでエンコードされ、クライアントに保存されるOpenAMセッションの状態の情報。セッションの情報は、OpenAMのメモリに保持されません。ブラウザベースのクライアントの場合、OpenAMはセッション情報が含まれているブラウザのCookieに設定します。

* 対象  
リソースへのアクセスを要求するエンティティ  
対象の認証が成功すると、OpenAMは他の対象と区別する主体を対象に関連付けます。対象は、複数の主体に関連付けることができます。

* ユーザーデータストア  
主体のプロファイルを保持するデータストレージサービス。基礎をなすストレージは、LDAPディレクトリサービス、リレーショナルデータベース、またはカスタムIdRepoを実装することができます。

* Web ポリシーエージェント  
WebページのURLに基づいたポリシーとともに、ポリシーエージェントとして動作するWebサーバーにインストールネイティブライブラリ。
