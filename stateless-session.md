## ステートレスセッション

この章では、ステートフルセッションとステートレスセッションの違いについて説明し、OpenAMでの設定方法を示します。

### OpenAMのセッションについて

ユーザーが正常に認証されると、OpenAMはリソースへのユーザーのアクセスを管理するためのセッションを作成します。 OpenAMは、ユーザーのログインがまだ有効であるか、またユーザーを再認証する必要があるかを判断するために、セッションに格納された情報を使用しています。

OpenAMのセッションには「ステートレス」または「ステートフル」があり、以下のセクションで詳細に記載されています。

### ステートフルセッション

ステートフルセッションは、OpenAMサーバーのメモリに常駐するセッションで、セッションフェイルオーバーが有効になっている場合は、コアトークンサービスのトークンストアにも保持されます。OpenAMは、OpenAMのメモリ内のセッションへの参照をクライアントに送信しますが、それはセッションの状態に関する情報のいずれも含んでいません。セッションへの参照は、SSOトークンとしても知られています。クライアントがブラウザの場合、OpenAMはセッションへの参照が含まれているCookieをブラウザに設定します。RESTクライアントの場合、OpenAMは認証エンドポイントへの呼び出しへのレスポンスとともに、セッションの参照を返します。

ステートフルセッションは可鍛性です。セッションが存続する間、OpenAMサーバーはユーザーのセッションの様々な値を変更することができます。

### ステートレスセッション

ステートレスセッションは、状態の情報がOpenAMでエンコードされ、クライアントに送信されるセッションですが、セッション内の情報はOpenAMのメモリに保持されません。ブラウザベースのクライアントの場合、OpenAMはセッションの状態が含まれているCookieをブラウザに設定します。ブラウザがOpenAMにクッキーを送信すると、OpenAMはCookieをもとにセッションの状態をデコードします。

ステートレスセッションは不変です。これは、OpenAMがユーザーのブラウザにステートレスセッションのCookieを設定するときに、ユーザーがOpenAMをログアウトするかセッションが期限切れになるまで、クッキーを更新しないことを意味します。

### レルムでの設定

ステートフルセッションとステートレスセッションはレルム単位で設定されます。デフォルトでステートフルセッションを使用します。指定されたレルムに認証しているすべてのユーザーのセッションは、個々のレルムの設定に応じて、ステートフルまたはステートレスのいずれかとなります。 OpenAMでは、いくつかのレルムではステートフルセッションを使用し、それ以外ではステートレスセッションを使用するといった配備ができます。

しかし、レルムあたりのセッション状態の設定には1つの例外があります。トップレベルの管理者(デフォルトでは、amadminユーザー)がOpenAMに認証されると、トップレベルのレルムがステートレスセッションで設定されている場合でも、セッションは常にステートフルです。

### OpenAM認証中のセッションの状態

OpenAMで認証されている間は、ユーザーが認証しているレルムをステートフルセッションまたはステートレスセッションのどちらに設定したかに関係なく、認証しているユーザーのセッションをメモリ内に保持します。

認証が完了した後、OpenAMはステートレスセッションに設定されたレルムで認証しているユーザーのメモリ内のセッションを削除します。ステートフルセッションに設定されたレルムで認証しているユーザーのセッションは、OpenAMのメモリヒープに残ります。

### セッションのカスタマイズ

ポストの認証プラグインを使用すると、ステートフルセッションとステートレスセッションの両方でカスタム情報を保持することができます。ポスト認証プラグインの詳細については、OpenAM開発者ガイドの「Creating a Post Authentication Plugin」の章を参照してください。

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
