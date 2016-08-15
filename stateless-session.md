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

## セッションCookie

OpenAMは、ステートフルとステートレスの両方のセッションのために、認証されたユーザーのブラウザにCookieを書き込みます。デフォルトで、クッキーの名前はiPlanetDirectoryProです。ステートフルセッションの場合、このCookieの値の大きさが比較的小さく(約100バイト)、OpenAMサーバー上のステートフルセッションへの参照とその他いくつかの情報のが含まれています。ステートレスセッションの場合、「iPlanetDirectoryPro」Cookieはかなり大きく(約2000バイト以上)、セッションがステートフルであったとすればOpenAMサーバーのメモリに保持されるすべての情報が含まれています。

ステートレスセッションCookieは、2つの部分から構成されています。クッキーの最初の部分はステートフルセッションのCookieと同じで、セッションタイプに関係なくクッキーの互換性が保証されます。2番目の部は、ベース64エンコードされたJSON Webトークン（JWT）であり、以下の図に示すように、セッション情報が含まれています。

![図. ステートフルセッションクッキーとステートレスセッションクッキー](images/site-and-sfo/iPlanetDirectoryProCookie.png)

上の図は、ステートフルセッションとステートレスセッションのCookie値の差を示しています。図は縮尺通りではないことに注意して下さい。ステートレスセッションの「iPlanetDirectoryPro」Cookieは、ステートフルセッションよりも10倍以上大きいです。

ユーザーのセッションに追加属性を保持するようにOpenAMをカスタマイズする場合、ステートレスセッションのCookieのサイズが大きくなります。Cookieのサイズが、エンドユーザーのブラウザで許可される最大Cookieサイズを超えないことを確認にする必要があります。

### ステートレスセッションCookieのセキュリティ

ステートレスセッションCookieを使用する場合は、Cookie「iPlanetDirectoryPro」に挿入されたJWTを署名し、暗号化するようにに、OpenAMを設定する必要があります。

JWTの署名と暗号化のためのステートレスセッションCookieの設定は、「ステートレスセッションCookieセキュリティの設定」の9.8項で説明されています。

#### JWT 署名

OpenAMは、シングルサインオンが要求されるたびに以前の認証の証明として、ユーザーのブラウザにiPlanetDirectoryProクッキーを設定します。 OpenAMは、セッションサービスで設定された署名を検証することにより、Cookieが本物であることを検証します。 OpenAMは、Cookieまたはその署名の内容を改ざんしようとする、または不正な署名でCookieに署名しようとする可能性がある攻撃者を阻止します。

#### JWT 暗号

知識のあるユーザーは、base64でエンコードされたJWTを簡単にデコードすることができます。 OpenAMのセッションには機密と見なされる可能性のある情報が含まれているため、セッションが含まれているJWTを暗号化することで不透明性を確保することにより、その内容を保護します。

JWTの暗号化は、全てのOpenAMセッションの状態を記録することが可能なman-in-the-middle攻撃を防ぎます。暗号化は、エンドユーザーが自分のOpenAMセッション内の情報にアクセスすることができないことも保証します。

### Core Token Service Usage

OpenAM uses the Core Token Service differently for stateful and stateless sessions.

For stateful sessions, OpenAM uses the Core Token Service's token store to save user sessions when session failover is enabled. In the event of the failure of an OpenAM server, one or more backup servers can retrieve the sessions from the Core Token Service's token store to reestablish users login sessions during session failover.

With stateless sessions, OpenAM does not store user sessions in the Core Token Service's token store. Instead, OpenAM stores sessions in the iPlanetDirectoryPro cookie on the user's browser. If an OpenAM server fails, another server handling the user's request simply reads the stateless session from the iPlanetDirectoryPro cookie. Session failover need not be enabled for the other server to be able to read the session.

Session blacklisting is an optional feature that maintains a list of logged out stateless sessions in the Core Token Service's token store. The next section describes session logout, including session blacklisting for stateless sessions.

### Sessionの終了

OpenAM manages active sessions, allowing single sign-on when authenticated users attempt to access system resources in OpenAM's control.

OpenAM ensures that user sessions are terminated when a configured timeout is reached, or when OpenAM users perform actions that cause session termination. Session termination effectively logs the user out of all systems protected by OpenAM.

With stateful sessions, OpenAM terminates sessions in four situations:

- When a user explicitly logs out
- When an administrator monitoring sessions explicitly terminates a session
- When a session exceeds the maximum time-to-live
- When a user is idle for longer than the maximum session idle time

Under these circumstances, OpenAM responds by removing stateful sessions from the memory heap of the OpenAM server on which the session resides, and from the Core Token Service's token store (if session failover is enabled). With the user's stateful session no longer in memory, OpenAM forces the user to reauthenticate on subsequent attempts to access resources protected by OpenAM.

When a user explicitly logs out of OpenAM, OpenAM also attempts to invalidate the iPlanetDirectoryPro cookie in users' browsers by sending a Set-Cookie header with an invalid session ID and a cookie expiration time that is in the past. In the case of administrator session termination and session timeout, OpenAM cannot invalidate the iPlanetDirectoryPro cookie until the next time the user accesses OpenAM.

Session termination differs for stateless sessions. Since stateless sessions are not maintained in OpenAM's memory, administrators cannot monitor or terminate stateless sessions. Because OpenAM does not modify the iPlanetDirectoryPro cookie for stateless sessions after authentication, the session idle time is not maintained in the cookie. Therefore, OpenAM does not automatically terminate stateless sessions that have exceeded the idle timeout.

As with stateful sessions, OpenAM attempts to invalidate the iPlanetDirectoryPro cookie from a user's browser when the user logs out. When the maximum session time is exceeded, OpenAM also attempts to invalidate the iPlanetDirectoryPro cookie in the user's browser the next time the user accesses OpenAM.

It is important to understand that OpenAM cannot guarantee cookie invalidation. For example, the HTTP response containing the Set-Cookie header might be lost. This is not an issue for stateful sessions, because a logged out stateful session no longer exists in OpenAM memory, and a user who attempts to reaccess OpenAM after previously logging out will be forced to reauthenticate.

However, the lack of a guarantee of cookie invalidation is an issue for deployments with stateless sessions. It could be possible for a logged out user to have an iPlanetDirectoryPro cookie. OpenAM could not determine that the user previously logged out. Therefore, OpenAM supports a feature that takes additional action when users log out of stateless sessions. OpenAM can maintain a list of logged out stateless sessions in a session blacklist in the Core Token Service's token store. Whenever users attempt to access OpenAM with stateless sessions, OpenAM checks the session blacklist to validate that the user has not, in fact, logged out.

For more information about session blacklist options, see Configuring Session Blacklisting .

### Choosing Between Stateful and Stateless Sessions

With stateful sessions, OpenAM ties users' sessions to specific servers. Servers can be added to OpenAM sites, but as servers are added, the overall workload balances gradually, assuming a short session lifetime. If an OpenAM server fails, sessions are retrieved from the Core Token Service's token store, and performance can take some time to recover. Crosstalk, an expensive operation, is incurred whenever a user arrives at an OpenAM server that is not the user's home server. Adding servers to OpenAM sites does not improve performance in a horizontally scalable manner; as more servers are added to a site, coordination among the servers becomes more complex.

Stateless sessions provide the following advantages:

Elasticity and horizontal scalability
With stateless sessions you can add and remove OpenAM servers to a site and the session load should balance horizontally. Elasticity is important for cloud deployments with very large numbers of users when there are significant differences between peak and normal system loads.

Simpler load balancing configuration
Stateless sessions do not require the use of a load balancer with session stickiness to achieve optimal performance, making deployment of OpenAM on multiple servers simpler.

Stateful sessions provide the following advantages:

Faster performance with equivalent hosts
Stateless sessions must send a larger cookie to the OpenAM server, and the JWT in the stateless session cookie must be decrypted. The decryption operation can significantly impact OpenAM server performance, reducing the number of session validations per second per host.

Because using stateless sessions provides horizontal scalability, overall performance on hosts using stateless sessions can be easily improved by adding more hosts to the OpenAM deployment.

Full feature support
Stateful sessions support all OpenAM features. Stateless sessions do not. For information about restrictions on OpenAM usage with stateless sessions, see Limitations When Using Stateless Sessions .

Session information is not resident in browser cookies
With stateful sessions, all the information about the session resides on the OpenAM server. With stateless sessions, session information is held in browser cookies. This information could be very long-lived.

The following table contrasts the impact of using stateful and stateless sessions in an OpenAM deployment:

Table 9.1. Impact of Deploying OpenAM Using Stateful and Stateless Sessions
Deployment Area	Stateful Session Deployment	Stateless Session Deployment
Hardware	Higher RAM consumption	Higher CPU consumption
Logical Hosts	Smaller number of hosts	Variable or large number of hosts
Session Monitoring	Available	Not available
Session Location	In OpenAM server memory heap	In a cookie in the user's browser
Session Failover	Requires session stickiness to be configured in the load balancer	Does not require session stickiness
Core Token Service Usage	Supports session failover	Supports session blacklisting for logged out sessions
Core Token Service Demand	Heavier	Lighter
Session Security	Sessions are not accessible to users because they reside in memory on the OpenAM server.	Sessions should be signed and encrypted.
Policy Agents	Sessions cached in the Policy Agent can receive change notification.	Sessions cached in the Policy Agent cannot receive change notification.

### Installation Planning for Stateless Sessions

Session blacklisting uses the Core Token Service's token store during the logout process. For more information about deploying the Core Token Service, see Configuring the Core Token Service.

Also, ensure the trust store used by OpenAM has the necessary certificates installed:

A certificate is required for encrypting JWTs containing stateless sessions.

If you are using RS256 signing, then a certificate is required to sign JWTs. (HMAC signing uses a shared secret.)

The same certificates must be stored on all servers participating in an OpenAM site.

### Configuring OpenAM for Stateless Sessions

To configure stateless sessions for a realm, follow these steps:

Procedure 9.1. Enable Stateless Sessions in a Realm
Navigate to Realms > Realm Name > Authentication > Settings > General.

Select the "Use Stateless Sessions" check box.

Click Save.

To verify that OpenAM creates a stateless session when non-administrative users authenticate to the realm, follow these steps:

Procedure 9.2. Verify that Stateless Sessions Are Enabled
Authenticate to the OpenAM console as the top-level administrator (by default, the amadmin user). Note that the amadmin user's session will be stateful, because OpenAM sessions for the top-level administrator are always stateful.

Select the Sessions tab.

Verify that a session is present for the amadmin user.

In your browser, examine the OpenAM cookie, named iPlanetDirectoryPro by default. Copy and paste the cookie's value into a text file and note its size.

Start up a private browser session that will not have access to the iPlanetDirectoryPro cookie for the amadmin user:

On Chrome, open an incognito window.

On Internet Explorer, start InPrivate browsing.

On Firefox, open a new private window.

On Safari, open a new private window.

Authenticate to OpenAM as a non-administrative user in the realm for which you enabled stateless sessions. Be sure not to authenticate as the amadmin user this time.

In your browser, examine the iPlanetDirectoryPro cookie. Copy and paste the cookie's value into a second text file and note its size. The size of the stateless session cookie's value should be considerably larger than the size of the stateful session cookie's value for the amadmin user. If the cookie is not larger, you have not enabled stateless sessions correctly.

Return to the original browser window in which the OpenAM console appears.

Refresh the window containing the Sessions tab.

Verify that a session still appears for the amadmin user, but that no session appears for the non-administrative user in the realm with stateless sessions enabled.

### Configuring Stateless Session Cookie Security

When using stateless sessions, you should sign and encrypt JWTs in the iPlanetDirectoryPro cookie.

Prior to configuring stateless session cookie security, ensure that you have deployed certificates as needed. For more information about managing certificates for OpenAM, see Managing Certificates .

To ensure security of stateless session cookie JWTs, configure a JWT signature and encrypt the entire JWT. The sections that follow provide detailed steps for configuring stateless session cookie security.

For more information about stateless session cookie security, see Stateless Session Cookie Security .

Important
When deploying multiple OpenAM servers in an OpenAM site, every server must have the same security configuration. Shared secrets and security keys must be identical. If you modify shared secrets or keys, you must make the modifications to all the servers on the site.

#### Configuring the JWT Signature

Configure a JWT signature to prevent malicious tampering of stateless session cookies.

Perform the following steps to configure the JWT signature:

Procedure 9.3. To Configure the JWT Signature
Navigate to Configuration > Global > Session and then locate the Stateless Sessions section.

Specify the Signing Algorithm Type. The default value is HS256.

If you specified an HMAC signing algorithm, change the value in the Signing HMAC Shared Secret field if you do not want to use the generated default value.

If you specified the RS256 signing algorithm, specify a value in the Signing RSA Certificate Alias field to use for signing the JWT signature.

Click Save.

For detailed information about Session Service configuration attributes, see the entries for Session.

#### Configuring JWT Encryption

Configure JWT encryption to prevent man-in-the-middle attackers from accessing users' session details, and to prevent end users from examining the content in the JWT.

Perform the following steps to encrypt the JWT:

Procedure 9.4. To Configure JWT Encryption
Navigate to Configuration > Global > Session and then locate the Stateless Sessions section.

Specify the Encryption Algorithm Type as a value other than NONE.

Specify a value in the Encryption RSA Certificate Alias to use for encrypting the JWT signature.

Click Save.

Ensure that the JWT signature configuration is identical on every OpenAM server in your OpenAM site.

For detailed information about Session Service configuration attributes, see the entries for Session.

### Configuring Session Blacklisting

Session blacklisting ensures that users who have logged out of stateless sessions cannot achieve single sign-on without reauthenticating to OpenAM.

Perform the following steps to configure session blacklisting:

Procedure 9.5. To Configure OpenAM for Session Blacklisting
Make sure that you deployed the Core Token Service during OpenAM installation. The session blacklist is stored in the Core Token Service's token store.

Navigate to Configuration > Global > Session and then locate the Stateless Sessions section.

Select the Enable Session Blacklisting option to enable session blacklisting for stateless sessions. When you configure one or more OpenAM realms for stateless sessions, you should enable session blacklisting in order to track session logouts across multiple OpenAM servers.

Configure the Session Blacklist Cache Size property.

OpenAM maintains a cache of logged out stateless sessions. The cache size should be around the number of logouts expected in the maximum session time. Change the default value of 10,000 when the expected number of logouts during the maximum session time is an order of magnitude greater than 10,000. An underconfigured session blacklist cache causes OpenAM to read blacklist entries from the Core Token Service store instead of obtaining them from cache, which results in a small performance degradation.

Configure the Blacklist Poll Interval property.

OpenAM polls the Core Token Service for changes to logged out sessions if session blacklisting is enabled. By default, the polling interval is 60 seconds. The longer the polling interval, the more time a malicious user has to connect to other OpenAM servers in a cluster and make use of a stolen session cookie. Shortening the polling interval improves the security for logged out sessions, but might incur a minimal decrease in overall OpenAM performance due to increased network activity.

Configure the Blacklist Purge Delay property.

When session blacklisting is enabled, OpenAM tracks each logged out session for the maximum session time plus the blacklist purge delay. For example, if a session has a maximum time of 120 minutes and the blacklist purge delay is one minute, then OpenAM tracks the session for 121 minutes. Increase the blacklist purge delay if you expect system clock skews in a cluster of OpenAM servers to be greater than one minute. There is no need to increase the blacklist purge delay for servers running a clock synchronization protocol, such as Network Time Protocol.

Click Save.

For detailed information about Session Service configuration attributes, see the entries for Session.

### ステートレスセッションを使用する場合の制限事項

次OpenAMの機能は、ステートレスセッションを使用するレルムでサポートされていません。

- 認証レベルとセッションアップグレード
- セッション割り当ての設定
- 現在のセッションプロパティを参照する条件を含む認可ポリシー
- クロスドメインシングルサインオン
- SAML v2.0シングルサインオンとシングルログアウト
- SAML v1.xシングルサインオン
- SNMPセッション監視
- OpenAMコンソールを使用したセッション管理
- セッション通知
