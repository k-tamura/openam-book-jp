## ステートレスセッション

OpenAMは、次の2つのタイプのセッションをサポートしています。

- ステートフルセッション：認証済みのユーザーのセッションが、サーバーのメモリ上に保存される一般的なセッション
- ステートレスセッション：認証済みのユーザーのセッションが、サーバーのメモリ上に保存されず、クライアント(ブラウザなど)に保存されるセッション

この章では、ステートフルセッションとステートレスセッションの違いについて説明し、OpenAMでの設定方法を示します。

### OpenAMのセッションについて

ユーザーが正常に認証されると、OpenAMはリソースへのユーザーのアクセスを管理するためのセッションを作成します。 OpenAMは、ユーザーのログインがまだ有効であるか、またユーザーを再認証する必要があるかを判断するために、セッションに格納された情報を使用しています。

OpenAMのセッションには「ステートレス」または「ステートフル」があり、以下のセクションで詳細に記載されています。

### ステートフルセッション

ステートフルセッションは、OpenAMサーバーのメモリに常駐するセッションで、セッションフェイルオーバーが有効になっている場合は、コアトークンサービスのトークンストアにも保持されます。OpenAMは、OpenAMのメモリ内のセッションへの参照をクライアントに送信しますが、それはセッションの状態に関する情報のいずれも含んでいません。セッションへの参照は、SSOトークンとしても知られています。クライアントがブラウザの場合、OpenAMはセッションへの参照が含まれているCookieをブラウザに設定します。RESTクライアントの場合、OpenAMは認証エンドポイントへの呼び出しへのレスポンスとともに、セッションの参照を返します。

ステートフルセッションは可鍛性です。セッションが存続する間、OpenAMサーバーはユーザーのセッションの様々な値を変更することができます。

### ステートレスセッション

Stateless sessions are sessions in which state information is encoded in OpenAM and sent to clients, but the information from the sessions is not retained in OpenAM's memory. For browser-based clients, OpenAM sets a cookie in the browser that contains the session state. When the browser transmits the cookie back to OpenAM, OpenAM decodes the session state from the cookie.

Stateless sessions are immutable. This means that when OpenAM sets a cookie for a stateless session in a user's browser, it never updates the cookie until the user has logged out of OpenAM, or until the user's session has expired.

### Configuration By Realm

Session statefulness and statelessness are configured at the realm level. OpenAM realms use stateful sessions by default. Sessions for all users authenticating to a given realm are either stateful or stateless, depending on the individual realm's configuration. OpenAM can be deployed with some realms using stateless sessions and so forth using stateful sessions.

There is, however, one exception to the per-realm session state configuration. When the top-level administrator (by default, the amadmin user) authenticates to OpenAM, the session is always stateful, even if the Top Level Realm is configured for stateless sessions.

### Session State During OpenAM Authentication

During authentication, OpenAM maintains the authenticating user's session in its memory regardless of whether you have configured the realm to which the user is authenticating for stateful or stateless sessions.

After authentication has completed, OpenAM deletes in-memory sessions for users authenticating to realms configured for stateless sessions. Sessions for users authenticating to realms configured for stateful sessions remain in OpenAM's memory heap.

### Session Customization

You can store custom information in both stateful and stateless sessions with post authentication plugins. For more information about post authentication plugins, see Section 4.1, "Creating a Post Authentication Plugin" in the OpenAM Developer's Guide.

## Session Cookies

OpenAM writes a cookie in the authenticated user's browser for both stateful and stateless sessions. By default, the cookie's name is iPlanetDirectoryPro. For stateful sessions, the size of this cookie's value is relatively small—approximately 100 bytes—and contains a reference to the stateful session on the OpenAM server and several other pieces of information. For stateless sessions, the iPlanetDirectoryPro cookie is considerably larger—approximately 2000 bytes or more—and contains all the information that would be held in the OpenAM server's memory if the session were stateful.

Stateless session cookies are comprised of two parts. The first part of the cookie is identical to the cookie for stateful sessions, which ensures the compatibility of the cookies regardless of the session type. The second part is a base 64-encoded Java Web Token (JWT), and it contains session information, as illustrated in the figure below.
Figure 9.1. Stateful and Stateless Session Cookies
Stateful and Stateless Session Cookies

The preceding diagram illustrates the difference between stateful and stateless session cookie values. Note that the diagram is not to scale. The iPlanetDirectoryPro cookie for a stateless session is more than ten times larger than for a stateful session.

The size of the stateless session cookie increases when you customize OpenAM to store additional attributes in users' sessions. You are responsible for ensuring that the size of the cookie does not exceed the maximum cookie size allowed by your end users' browsers.

### ステートレスセッションCookieのセキュリティ

When using stateless session cookies, you should configure OpenAM to sign and encrypt the JWT inserted in the iPlanetDirectoryPro cookie.

Configuring stateless session cookies for JWT signing and encryption is discussed in Section 9.8, "Configuring Stateless Session Cookie Security".

#### JWT 署名

OpenAMは、シングルサインオンが要求されるたびに以前の認証の証明として、ユーザーのブラウザにiPlanetDirectoryProクッキーを設定します。 OpenAMは、セッションサービスで設定された署名を検証することにより、Cookieが本物であることを検証します。 OpenAMは、Cookieまたはその署名の内容を改ざんしようとする、または不正な署名でCookieに署名しようとする可能性がある攻撃者を阻止します。

#### JWT 暗号

知識のあるユーザーは、base64でエンコードされたJWTを簡単にデコードすることができます。 OpenAMのセッションには機密と見なされる可能性のある情報が含まれているため、セッションが含まれているJWTを暗号化することで不透明性を確保することにより、その内容を保護します。

JWTの暗号化は、全てのOpenAMセッションの状態を記録することが可能なman-in-the-middle攻撃を防ぎます。暗号化は、エンドユーザーが自分のOpenAMセッション内の情報にアクセスすることができないことも保証します。
