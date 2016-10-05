[TODO 作成中]

## 認可ポリシーの定義

認可は、リソースへのユーザーアクセスを付与したり、拒否するかどうかを決定することです。ポリシーは、アクセスを許可または拒否するかどうかを判断する方法を定義します。この章では、OpenAMで認可ポリシーを設定する方法について説明します。

### OpenAMでの認可について

認証(誰がリソースにアクセスしようとしているかを決定すること)と認可(アクセスを許可または拒否するかどうかを決定すること)に分解されるアクセス管理に対して、アプリケーションはOpenAMに依存しています。
一般的にアクセスが許可されたかどうかは、次の条件に依存しているからです。

- アクセスについてのポリシーがどのようなものであるか
- 誰がアクセス権を取得しようとしているか
- アクセス自体は安全なチャネルを介している必要があるかどうか
- アクセスがいつであるか　など

To return to the international airport example from the discussion on authentication the policy might be that passengers with valid passports and visas presenting valid plane tickets are allowed through to the gate where the plane is waiting to take off, but only under the condition that the plane is going to leave soon. (You cannot expect to get to the gate today with a scheduled departure for three months from now.)

#### OpenAMのリソースタイプ、ポリシーセット、ポリシー

OpenAMでは、リソースへの対象のアクセスを許可するかどうかを決定できるように認可ポリシーを定義します。

ポリシーは、次のように定義しています:

**リソース (resources)**  
リソースには、Webページや搭乗エリアへのアクセスのような、どのリソースにポリシーが適用されるかを示す制約を定義します。

**アクション (actions)**  
アクションは、Webページを読み込む、Webフォームをサブミットする、搭乗エリアにアクセスするといった、ポリシーによりユーザーがリソースに行うことができることを説明する動詞です。

**対象条件 (subject conditions)**  
対象条件は、ポリシーが適用される人(すべての認証済みユーザー、管理者のみ、すぐに出る飛行機の有効なチケットを持つ乗客のみというような)を制約します。

**環境条件 (environment conditions)**  
環境条件は、ポリシーが適用される状況下(勤務時間中のみ、特定のIPアドレスからのアクセスのみ、4時間以内に出発するフライトが予定されている場合のみというような)を設定します。

**応答属性 (response attributes)**  
OpenAMがポリシー決定に従い応答に付加する属性情報(名前、メールアドレス、マイレージプログラムのステータスなど)を定義します。

保護されたリソースにユーザーをアクセスできるようにするかどうかについて照会すると、OpenAMは、ポリシーの決定において、以下に説明するように適用可能なポリシーに基づいてアクセスを認可するかどうかを決定します。OpenAMは、アクセス管理のためにOpenAMを使用しているアプリケーションにその決定を伝えます。一般的なケースでは、これは、アプリケーションが実行されるサーバーにインストールされたポリシーエージェントです。エージェントは、OpenAMからの認可決定を実施します。

図. レルム、ポリシー、ポリシーセットの関係

![レルム、ポリシー、ポリシーセットの関係](images/realm-app-policy-overview.png)

ポリシーの作成を支援するために、OpenAMは、リソースタイプとポリシーセットを使用します。

**リソースタイプ (Resource types)**  
リソースタイプは、ポリシーを適用するリソースとそれらのリソース上で実行することができるアクションについてのテンプレートを定義します。

例えば、OpenAMにデフォルトで含まれているURLリソースタイプは、Webページやアプリケーションを保護するためのテンプレートとして機能します。 リソースパターン(*://*:*/*?*のような)が含まれ、ポリシーで使用されるときには、より具体的にすることができます。リソースがサポートするアクションは、次のようにも定義されます:

- GET
- POST
- PUT
- HEAD
- PATCH
- DELETE
- OPTIONS

OpenAMには、 https://*:*/*?* とCRUDPAQアクションを含むパターンで、RESTエンドポイントを保護するためのリソースタイプも含まれています:

- CREATE
- READ
- UPDATE
- DELETE
- PATCH
- ACTION
- QUERY

**ポリシーセット (Policy Sets)**  
ポリシーセットは、リソースタイプのセットに関連付けられ、それが提供するテンプレートに基づいて1つ以上のポリシーを含んでいます。

例えば、Example.comの人事サービスのためのアプリケーションは、HTTP GETとPOSTアクションのみhttp*://example.com/hr* と http*://example.com/hr*?* の下にURLリソースタイプを適用するすべてのポリシーを制約する、リソースタイプが含まれている場合があります。

管理コンソールの レルム > レルム名 > 認可 でポリシーセット、ポリシー、リソースタイプを設定します。

図. 管理コンソールのポリシーセット

![管理コンソールのポリシーセット](images/authorization/policy-set.png)

ポリシーおよびリソースタイプの閲覧、作成、編集の詳細については、リソースタイプ、ポリシーセット、ポリシーの設定を参照してください。

#### OpenAMのポリシー決定

OpenAM relies on policies to reach authorization decisions, such as whether to grant or to deny access to a resource. OpenAM acts as the policy decision point (PDP), whereas OpenAM policy agents act as policy enforcement points (PEP). In other words, a policy agent or other PEP takes responsibility only for enforcing a policy decision rendered by OpenAM. When you configured applications and their policies in OpenAM, you used OpenAM as a policy administration point (PAP).

Concretely speaking, when a PEP requests a policy decision from OpenAM it specifies the target resource(s), the policy set (default: iPlanetAMWebAgentService), and information about the subject and the environment. OpenAM as the PDP retrieves policies within the specified policy set that apply to the target resource(s). OpenAM then evaluates those policies to make a decision based on the conditions matching those of the subject and environment. When multiple policies apply for a particular resource, the default logic for combining decisions is that the first evaluation resulting in a decision to deny access takes precedence over all other evaluations. OpenAM only allows access if all applicable policies evaluate to a decision to allow access.

OpenAM communicates the policy decision to the PEP. The concrete decision, applying policy for a subject under the specified conditions, is called an entitlement.

The entitlement indicates the resource(s) it applies to, the actions permitted and denied for each resource, and optionally response attributes and advice.

When OpenAM denies a request due to a failed condition, OpenAM can send advice to the PEP, and the PEP can then take remedial action. For instance, suppose a user comes to a web site having authenticated with an email address and password, which is configured as authentication level 0. Had the user authenticated using a one-time password, the user would have had authentication level 1 in their session. Yet, because they have authentication level 0, they currently cannot access the desired page, as the policy governing access requires authentication level 1. OpenAM sends advice, prompting the PEP to have the user re-authenticate using a one-time password module, gaining authentication level 1, and thus having OpenAM grant access to the protected page.

#### Example Authorization
Consider the case where OpenAM protects a user profile web page. An OpenAM policy agent installed in the web server intercepts client requests to enforce policy. The policy says that only authenticated users can access the page to view and to update their profiles.

When a user browses to the profile page, the OpenAM policy agent intercepts the request. The policy agent notices that the request is to access a protected resource, but the request is coming from a user who has not yet logged in and consequently has no authorization to visit the page. The policy agent therefore redirects the user's browser to OpenAM to authenticate.

OpenAM receives the redirected user, serving a login page that collects the user's email and password. With the email and password credentials, OpenAM authenticates the user, and creates a session for the user. OpenAM then redirects the user to the policy agent, which gets the policy decision from OpenAM for the page to access, and grants access to the page.

While the user has a valid session with OpenAM, the user can go away to another page in the browser, come back to the profile page, and gain access without having to enter their email and password again.

Notice how OpenAM and the policy agent handle the access in the example. The web site developer can offer a profile page, but the web site developer never has to manage login, or handle who can access a page. As OpenAM administrator, you can change authentication and authorization independently of updates to the web site. You might need to agree with web site developers on how OpenAM identifies users so web developers can identify users by their own names when they log in. By using OpenAM and policy agents for authentication and authorization, your organization no longer needs to update web applications when you want to add external access to your Intranet for roaming users, open some of your sites to partners, only let managers access certain pages of your HR web site, or allow users already logged in to their desktops to visit protected sites without having to type their credentials again.

### How OpenAM Reaches Policy Decisions
OpenAM has to match policies to resources to take policy decisions. For a policy to match, the resource has to match one of the resource patterns defined in the policy. The user making the request has to match a subject. Furthermore, at least one condition for each condition type has to be satisfied.

If more than one policy matches, OpenAM has to reconcile differences. When multiple policies match, the order in which OpenAM uses them to make a policy decision is not deterministic. However, a deny decision overrides an allow decision, and so by default once OpenAM reaches a deny decision it stops checking further policies. If you want OpenAM to continue checking despite the deny, navigate to Configure > Global Services, click Policy Configuration, and then enable Continue Evaluation on Deny Decision.
