### OpenID Connectとは

OpenID Connect 1.0は、認可を目的としたプロトコルであるOAuth 2.0をベースに、認証やセッション管理なども実現できるように拡張したプロトコルです。

```
OAuth 2.0＋（認証、セッション管理など）＝OpenID Connect 1.0
```

OpenID Connectは、「making simple things simple and complicated things possible（簡単なことは簡単に、複雑なことも可能に）」を設計目標としており、開発者が実装すべき部分はできる限り少なくしながらも、さまざまなタイプのアプリケーションと連携できるような考慮がされています。

一般的なWebアプリケーションだけでなく、ブラウザーベースのJavaScriptやネイティブモバイルアプリケーションとも連携し、SSOができます。また、SAMLがSOAP/XMLを使用した複雑なプロトコルであるのに対して、OpenID ConnectはREST/JSONを使用した開発者にやさしいプロトコルとなっています。

OpenID Connect のアクターには次の3種類があります。

・エンドユーザー：

アプリケーションを利用するユーザーです。

・OpenID Provider（OP）：

エンドユーザーを認証し、そのユーザーの情報を提供するサーバーです。OpenID Connectをサポートする OAuth 2.0認可サーバーといい換えることもできます（以下、「OP」と略します）。

・Relying Party（RP）：

OPにエンドユーザーの認証を委譲するアプリケーションです。Webアプリケーションだけでなく、モバイルアプリケーションなども含まれます。OpenID Connectを利用するOAuth 2.0クライアントと言い換えることもできます（以下、「RP」と略します）。

RPがWebアプリケーションの場合の、OpenID Connectの典型的なフローは、以下のようになります。

図. OpenID Connectのフロー(ベーシックフロー)

まず、エンドユーザーはWebアプリケーションにアクセスします（1）。Webアプリケーションは「○○（OP）でログイン」というようなボタンのある画面を表示します。エンドユーザーがこのボタンをクリックすることで、OpenID Connectで規定されたフローが始まります。WebアプリケーションはエンドユーザーをOPの認可エンドポイント（注2）へとリダイレクトします（認証要求：2）。

注2：エンドポイント（Endpoint）：サービスを提供するURIを意味します。OpenID ConnectのOPは、認証・認可を実行するための認可エンドポイントや、エンドユーザーの情報を取得するためのユーザー情報エンドポイントなどを公開します。
OPはログイン画面を表示し（3）、それに対してエンドユーザーはログインを試行します（4）。このとき、どのような方式でログインさせるか、どのような条件で認証成功とするかはOpenID Connectの仕様では規定されていないので、例えば、OPはエンドユーザーに生体認証を求めることも、Yubikey認証を求めることもできます。認証が成功したら、OPは認可画面を表示し、Webアプリケーションがユーザー情報へアクセスすることへの許可をエンドユーザーに求めます（5）。それに対してエンドユーザーは許可または拒否します（6）。許可を得ると、OPはエンドユーザーをもとのWebアプリケーションへとリダイレクトします（認証応答：7）。認証応答には認可コードが含まれており、これを使ってトークンエンドポイントにアクセスし、アクセストークンとIDトークン（注3）を取得します（8）。必要であれば、アクセストークンを使用して、ユーザー情報エンドポイントからエンドユーザーの属性情報を取得します（9）。

注3：アクセストークンとIDトークン：アクセストークンと同時にIDトークンを受け取ることが、OAuth 2.0と大きく異なる点です。IDトークンには、エンドユーザーごとに一意な値が含まれており、これによりエンドユーザーが認証済みであるかどうかを判定できるようになっています。OpenID Connectでは、性能面やセキュリティを考慮した結果として、2つのトークンを使い分けるようになっています。
一般的にRPがWebアプリケーションの場合は、図2で説明した「認可コードフロー」というフローに従います。RPがネイティブモバイルアプリケーションの場合は、「インプリシットフロー」というフローに従います。ただし、厳密にはアプリケーションがパスワードを秘密にできるかどうか、またOPとRPが直接通信できるかどうかといった観点でどちらを選択するか決定します。

両者のフローには若干の違いがありますが、基本的な部分は同じです。いずれのフローも、OpenID Connect Core 1.0で規定されていますので、詳細に関して知りたい方はそちらを確認してください。

また、このフローでは省略しましたが、OPはエンドユーザーのセッションの有無の確認や、シングルログアウトを行うためのエンドポイントも公開します。これらの仕様はOpenID Connect Session Management 1.0にまとめられています（2014年5月の時点では仕様策定中です）。

### OpenAMのOpenID Connectへの対応

2013年11月8日、OpenID Connect 1.0の最終承認（2014年2月27日）に先立って、OpenID Connectに対応したOpenAM 11.0.0がリリースされました。これにより、OpenAMはOpenID ConnectのOPとして機能できるようになりました。

さらに、現在開発中のOpenAM 12.0.0では、OpenID ConnectのRPサイドの機能である「OpenID Connect IDトークン認証」モジュールを提供する予定になっています（注4）。この機能は、OPで認証されたエンドユーザーを、OpenAMと連携するアプリケーションへSSOできるようにします。デフォルトでGoogleがOPとして設定されています。

注4：「OpenID Connect IDトークン認証」モジュールは、RPの仕様を完全に実装しているわけではありません。機能概要については、後述する「表2 OpenAMのOAuth 2.0 / OpenID Connect対応状況」の中で説明します。
OpenAMにおける、OpenID ConnectおよびそのベースとなっているOAuth 2.0の対応状況は、次の通りです。

|プロトコル|役割|機能概要|対応バージョン|
|---|---|---|---|
|OAuth 2.0|クライアント|OAuth 2.0クライアント認証モジュールを設定することで、OpenAMがOAuth 2.0のクライアントとなり、FacebookやWindows Liveなどにログインを委譲できるようになる。|10.0.0|
||認可サーバー|OpenAMがOAuth 2.0の認可サーバーとなり、OAuth 2.0のクライアントであるWebアプリケーションなどに、ユーザー情報へのアクセス権限を付与できるようになる。|10.1.0|
|OpenID Connect 1.0|RP（Relying Party）|OpenID Connect IDトークン認証モジュールを設定すると、OpenAMに対するログインリクエストのヘッダーに、OPから取得したIDトークンが含まれているかをチェックするようになる。IDトークンが含まれていると、その値を検証し、妥当と判断した場合はエンドユーザーをOpenAMにログイン済みの状態にする。|12.0.0|
||OP（OpenID Provider）|OpenAM 11.0.0からは、OpenAMにOAuth 2.0認可サーバーの設定を行うことで、OpenID Connect 1.0のOPとしても機能するようになる。これにより、RPの認証をOpenAMに委譲することが可能。|11.0.0|

表. OpenAMのOAuth 2.0 / OpenID Connect対応状況

#### OpenID Connect OPとしてのOpenAMの設定

OpenAMにOPとしての役割を持たせるための設定はとても簡単です。まずOpenAMの管理コンソールに管理者ユーザーでログインし、トップページ（共通タスクページ）にある「OAuth2の設定」をクリックします。

表示された「OAuth 2.0の設定」画面で「作成」ボタンをクリックします。これだけで、最低限の機能を持ったOPとしてOpenAMが機能するようになります。

OAuth 2.0 認可サーバーの設定しかしていませんが、前述した通り、OpenID ConnectをサポートするOAuth 2.0 認可サーバーはOPであり、OpenAMの場合はこの設定だけでOPとしても機能するようになります。


作成したOAuth 2.0認可サーバーの設定は、「アクセス制御」→「レルム名」→「サービス」→「OAuth 2.0 プロバイダ」の順で画面遷移し、表示された「OAuth 2.0 プロバイダ」の編集画面で確認できます。必要に応じて各値を変更してください。

実際にOPとして動作していることは、http://[OpenAMサーバーのURL] /.well-known/openid-configuration にアクセスすることで確認できます。このURLは、RPがOPの各エンドポイントなどの情報を検索するためのURLで、OpenID Connect Discovery 1.0の仕様によって規定されています。

ブラウザーなどでアクセスしてみると、以下のようなJSON形式のレスポンスが返ってきます。

```json
{ "authorization_endpoint" : "https://openam.ossc.com/sso/oauth2/authorize",
  "check_session_iframe" : "https://openam.ossc.com/sso/oauth2/connect/checkSession",
  "claims_supported" : [ "phone", "email", "address", "openid", "profile" ],
  "end_session_endpoint" : "https://openam.ossc.com/sso/oauth2/connect/endSession",
  "id_token_signing_alg_values_supported" : 
[ "HmacSHA256", "HmacSHA512", "HmacSHA384" ],
  "issuer" : "https://openam.ossc.com/sso",
  "jwks_uri" : "",
  "registration_endpoint" : "https://openam.ossc.com/sso/oauth2/connect/register",
  "response_types_supported" : [ "token id_token", "code token", 
"code token id_token", "token", "code id_token", "code", "id_token" ],
  "subject_types_supported" : [ "public" ],
  "token_endpoint" : "https://openam.ossc.com/sso/oauth2/access_token",
  "userinfo_endpoint" : "https://openam.ossc.com/sso/oauth2/userinfo",
  "version" : "3.0"
}
```

認可エンドポイント（authorization_endpoint）やトークンエンドポイント（token_endpoint）、サポートしているレスポンスの種類（response_types_supported）などが確認できると思います。

#### OpenID Connect RPのプロファイルの登録

OPとしての設定ができましたが、これだけではRPと連携することはできません。RPのプロファイル（RPに関する情報、例えば、RPの名前やリダイレクトURIなど）をOPに登録する必要があります。OpenAMでは、RPのプロファイルを事前に登録することも動的に登録することもできます（注5）。後者の実装はOpenID Connect Dynamic Client Registration 1.0で規定された仕様に従います。

注5：動的登録：RPのプロファイルを事前にOPに登録しておくのではなく、前述の.well-known/openid-configurationから取得した登録エンドポイント（"registration_endpoint"の値）にアクセスして、そこにRPの情報を登録することをいいます。どのOPを選択するか、エンドユーザー自身が決定できるようにする目的で、このような仕様が策定されています。
ここでは、前者の方法について説明します。OpenAMの管理コンソールに管理者ユーザーでログインし、「アクセス制御」→「レルム名」→「エージェント」→「OAuth 2.0 クライアント」の順で画面遷移し、表示された「OAuth 2.0 クライアント」画面にあるエージェントの「新規」ボタンをクリックします。名前とパスワードを入力して「作成」ボタンをクリックすると、OAuth 2.0クライアントが作成されます。

完了したら一覧にあるそのRPを選択し、「OAuth 2.0クライアント」の編集画面で、リダイレクトURIやスコープの値を実際のRPと一致するように修正します。

これにより、OpenID ConnectをサポートするOAuth 2.0 クライアント（つまりRP）のプロファイルが作成されます。

OpenAMが提供するOpenID Connect機能を実際に動作確認することができるRPのサンプルが提供されています。フォージロックのMark Craig氏のGitHubに公開されています。
このサンプルによって、以下が確認できます。

- OpenID Connect認可コードフロー
- OpenID Connectインプリシットフロー
- OpenID Connect動的クライアント登録  

詳細な手順に関しては、OpenAM 11.0.0 管理者ガイドの「Chapter 14. Managing OpenID Connect 1.0 Authorization」を参照してください。
