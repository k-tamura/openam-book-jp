## ステートレスセッション

この章では、ステートフルセッションとステートレスセッションの違いについて説明し、OpenAMでの設定方法を示します。

### OpenAMのセッションについて

ユーザーが正常に認証されると、OpenAMはリソースへのユーザーのアクセスを管理するためのセッションを作成します。 OpenAMは、ユーザーのログインがまだ有効であるか、またユーザーを再認証する必要があるかを判断するために、セッションに格納された情報を使用しています。

OpenAMのセッションには「ステートレス」または「ステートフル」があり、以下のセクションで詳細に記載されています。

### ステートフルセッション

ステートフルセッションは、OpenAMサーバーのメモリに常駐するセッションで、セッションフェイルオーバーが有効になっている場合は、コアトークンサービスのトークンストアにも保持されます。OpenAMは、OpenAMのメモリ内のセッションへの参照(SSOトークン)をクライアントに送信しますが、それにはセッションの状態に関する情報はいっさい含まれていません。クライアントがブラウザの場合、OpenAMはセッションへの参照が含まれているCookieをブラウザに設定します。RESTクライアントの場合、OpenAMは認証エンドポイントへの呼び出しのレスポンスとともに、セッションの参照を返します。

ステートフルセッションは可変です。セッションが存続する間、OpenAMサーバーはユーザーのセッションの様々な値を変更することができます。

### ステートレスセッション

ステートレスセッションは、セッションの状態に関する情報がOpenAMでエンコードされ、クライアントに送信されるセッションですが、セッション内の情報はOpenAMのメモリに保持されません。ブラウザベースのクライアントの場合、OpenAMはセッションの状態が含まれているCookieをブラウザに設定します。ブラウザがOpenAMにクッキーを送信すると、OpenAMはCookieをもとにセッションの状態をデコードします。

ステートレスセッションは不変です。これは、OpenAMがユーザーのブラウザにステートレスセッションのCookieを設定するときに、ユーザーがOpenAMをログアウトするかセッションが期限切れになるまで、クッキーを更新しないことを意味します。

### レルムでの設定

ステートフルセッションとステートレスセッションはレルム単位で設定できます。デフォルトではステートフルセッションが設定されています。指定されたレルムに認証しているすべてのユーザーのセッションは、個々のレルムの設定に応じて、ステートフルまたはステートレスのいずれかとなります。 OpenAMでは、いくつかのレルムではステートフルセッションを使用し、それ以外ではステートレスセッションを使用するといった配備ができます。

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

### コアトークンサービスの使用方法

OpenAMは、ステートフルセッションとステートレスセッションで異なるコアトークンサービスを使用します。

ステートフルセッションでは、セッションフェイルオーバーが有効になっている場合、OpenAMはユーザーセッションを保存するために、コアトークンサービスのトークンストアを使用します。OpenAMサーバーで障害が発生した場合は、1つ以上のバックアップサーバーは、セッションのフェイルオーバー中にユーザーのログインセッションを再確立するために、コアトークンサービスのトークンストアからのセッションを取得することができます。

ステートレスセッションでは、OpenAMはCoreトークンサービスのトークンストア内のユーザーセッションを保存しません。その代わりに、OpenAMは、ユーザーのブラウザ上の「iPlanetDirectoryPro」Cookieのセッションを保存します。OpenAMサーバーに障害が発生した場合、ユーザーのリクエストを処理する別のサーバーは、単に「iPlanetDirectoryPro」Cookieからステートレスセッションを読み込みます。他のサーバーに対してセッションを読むことができるように、セッションフェイルオーバーを有効にする必要はありません。

セッションブラックリストは、コアトークンサービスのトークンストアにログアウトしたステートレスセッションのリストを維持するオプション機能です。次のセクションでは、ステートレスセッションのセッションブラックリストを含むセッションログアウトについて説明します。

### Sessionの終了

OpenAMは、認証されたユーザーがOpenAMの制御下のシステムリソースにアクセスしようとしたときにシングルサインオンを可能にする、アクティブなセッションを管理します。

設定されたタイムアウトに到達したとき、またはユーザーがセッション終了の原因となるアクションを実行したときに、ユーザーのセッションが終了されることが保証されます。セッションの終了は、OpenAMによって保護されているすべてのシステムから、効果的にユーザーをログアウトします。

ステートフルセッションでは、OpenAMは4つの状況でセッションを終了します。

- ユーザーが明示的にログアウトしたとき
- セッションを監視する管理者が明示的にセッションを終了するとき
- セッションが最大存続時間を超えたとき
- ユーザーが最大セッションアイドル時間を超過してアイドル状態であるとき

このような状況下で、OpenAMは、セッションが存在するOpenAMサーバーのメモリヒープから、および(セッションフェイルオーバーが有効になっている場合は)コアトークンサービスのトークンストアから、ステートフルセッションを削除することによって応答します。メモリ内のユーザーのステートフルセッションがなくなると、OpenAMは、OpenAMによって保護されたリソースへの追加のアクセスの試行するユーザーに再認証を強制します。

ユーザーが明示的にOpenAMのログアウトすると、OpenAMはまた、無効なセッションIDや有効期限の切れたクッキーを指定したSet-Cookieヘッダを送信することにより、ユーザーのブラウザの「iPlanetDirectoryPro」Cookieを無効化しようとします。管理者のセッション終了とセッションタイムアウトの場合、OpenAMは、ユーザーがOpenAMにアクセスする次の時間まで「iPlanetDirectoryPro」Cookieを無効にすることはできません。

セッションの終了は、ステートレスセッションごとに異なります。ステートレスセッションがOpenAMのメモリに保持されていないので、管理者はステートレスセッションを監視または終了することができません。認証後に、OpenAMはステートレスセッション用の「iPlanetDirectoryPro」Cookieを変更しないので、セッションアイドル時間はCookie内に維持されていません。したがって、OpenAMはアイドルタイムアウトを超過したステートレスセッションを自動的には終了しません。

ステートフルセッションと同じように、ユーザーがログアウトするときにOpenAMはユーザーのブラウザの「iPlanetDirectoryPro」Cookieを無効化しようとします。最大セッション時間を超過すると、OpenAMはユーザーが次にOpenAMにアクセスする時間でユーザーのブラウザの「iPlanetDirectoryPro」Cookieを無効化しようともします。

OpenAMがCookieの無効化を保証することはできないことを理解することは重要です。例えば、Set-Cookieヘッダーを含むHTTPレスポンスが失われる可能性があります。これはステートフルセッションの問題ではなく、(ログアウトされたステートフルセッションはもはやOpenAMのメモリ内に存在しないため、)以前にログアウトした後でOpenAMを再アクセスしようとするユーザーは、再認証を強制されます。

ただし、Cookieの無効化の保証の欠如はステートレスセッションでの展開における問題です。ログアウトしたユーザーが「iPlanetDirectoryPro」Cookieを保持する可能性はあります。OpenAMは、ユーザーが以前にログアウトしていることを決定することができません。したがって、OpenAMはユーザーがステートレスセッションからログアウトするときに、追加のアクションを実行する機能をサポートしています。 OpenAMは、コアトークンサービスのトークンストア内のセッションブラックリストに、ログアウトしたステートレスセッションのリストを維持することができます。ユーザーはステートレスセッションでOpenAMにアクセスしようとするたび、ユーザーが(実際には)ログアウトしていないか検証するため、OpenAMはセッションのブラックリストをチェックします。

セッションブラックリストのオプションの詳細については、Configuring Session Blacklisting を参照して下さい。

### ステートフルおよびステートレスセッションののどちらを選択するか

ステートフルセッションを使用すると、OpenAMは特定のサーバーへユーザーのセッションを結び付けます。サーバーはOpenAMのサイトに追加することができますが、サーバーが追加されるにつれて、短いセッションの生存期間を想定し、全体的なワークロードは徐々にバランスを取ります。 OpenAMサーバーに障害が発生した場合、セッションはコアトークンサービスのトークンストアから取得されます。したがって、パフォーマンスが回復するまでに時間がかかることがあります。ユーザーがユーザーのホームサーバーではないOpenAMサーバに到着するたびに、クロストーク(高価な操作)が発生します。より多くのサーバーがサイトに追加されるとサーバー間の調整はより複雑になるので、OpenAMのサイトへのサーバーの追加は、水平方向にスケーラブルな方法でパフォーマンスを向上しません。

ステートレスセッションは、次のような利点を提供します:

- イラスティシー(Elasticity)と水平方向のスケーラビリティー
 ステートレスセッションを使用すると、OpenAMサーバーをサイトに追加および削除でき、セッションの負荷は水平にバランスをとることができます。ピーク時と通常時のシステム負荷の間に有意な差があるとき、非常に大きなユーザー数を持つクラウドの配備のためにイラスティシーが重要です。

- よりシンプルなロードバランシングの設定
 ステートレスセッションは、最適なパフォーマンスを実現するために、スティッキーセションが有効なロードバランサーを使用する必要がありません(より簡単に複数のサーバー上にOpenAMの配備を行うことができます)。

ステートフルセッションは、次のような利点を提供します:

- 同等のホストとより高速なパフォーマンス  
 ステートレスセッションの場合、OpenAMサーバーにより大きなサイズのCookieを送信して、Cookie内のJWTを復号化する必要があります。復号化操作はホスト毎の秒間のセッションの検証数を削減し、OpenAMサーバーのパフォーマンスに大幅に影響に改善できます。

 ステートレスセッションを使用すると水平方向のスケーラビリティが向上するので、OpenAMの配備に複数のホストを追加することによって、ホスト上の全体的なパフォーマンスを簡単に改善できます。

- 機能のフルサポート  
ステートフルセッションは、すべてOpenAMの機能をサポートしていますが、ステートレスセッションはそうではありません。ステートレスセッションとOpenAMの使用の制限については、「ステートレスセッションを使用する場合の制限事項」を参照して下さい。

- セッション情報がブラウザのクッキーに存在しない  
 ステートフルセッションでは、セッションに関するすべての情報はOpenAMサーバー上に存在します。ステートレスセッションでは、セッション情報はブラウザのクッキー内に保持されています。この情報は非常に長い有効期間にできます。

表. ステートフルおよびステートレスセッションを使用したOpenAMの配備での影響

|配備方法|ステートフルセッション|ステートレスセッション|
|---|---|---|
|ハードウェア|高いRAM消費量|高いCPU消費量|
|論理ホスト|ホストの数が少ない|ホストの数が多いまたは可変|
|セッション監視|利用可能|利用できない|
|セッションの保存先|OpenAMサーバのメモリヒープ内|ユーザーのブラウザのクッキー内|
|セッションフェイルオーバー|ロードバランサのスティッキーセッション設定が必要|スティッキーセッション設定は不要|
|コアトークンサービスの使用|セッションフェイルオーバーをサポート|ログアウトしたセッションのためにセッションブラックリストをサポート|
|コアトークンサービスの需要|重い|軽い|
|セッションのセキュリティ|OpenAMサーバー上のメモリ内に存在するため、セッションはユーザーにはアクセスできません|セッションは、署名および暗号化する必要がある|
|ポリシーエージェント|ポリシーエージェントにキャッシュされたセッションは、変更通知を受け取ることができる|ポリシーエージェントにキャッシュされたセッションは、変更通知を受信できない|

### ステートレスセッションのインストール計画

セッションブラックリストは、ログアウト処理中にコアトークンサービスのトークンストアを使用します。コアトークンサービスを配備する方法の詳細については、インストールガイドのコアトークンサービスの設定を参照してください。

また、OpenAMが使用するトラストストアに必要な証明書がインストールされていることを確認してください。

- 証明書は、ステートレスセッションを含むJWTを暗号化するために必要とされます。
- RS256の署名を使用している場合、証明書はJWTに署名するために必要です(HMAC署名は共有シークレットを使用します)。

同証明書は、OpenAMサイトに参加している全てのサーバーに格納される必要があります。

### ステートレスセッションの設定

レルムにステートレスセッションを設定するには、次の手順を実行します:

**手順. レルム内でステートレスセッションを有効にする**

1. レルム > レルム名 > 認証 > 設定 > 一般 をクリック。
2. 「ステートレスセッションを使用する」のチェックボックスをチェックします。
3. 保存ボタンをクリック。

管理者以外のユーザーがレルムに認証される時にOpenAMがステートレスセッションを作成していることを確認するには、次の手順を実行します:

**手順. ステートレスセッションが有効になっていることを確認する**

1. トップレベルの管理者(デフォルトでは、amadmin)としてOpenAMコンソールにログインします。トップレベルの管理者のOpenAMセッションは常にステートフルであることに注意して下さい。
2. 「セッション」タブを選択します。
3. amadminのセッションが存在していることを確認します。
4. ブラウザ内の、デフォルトではiPlanetDirectoryProで名付けられているOpenAMのCookieを調べます。Cookieの値をテキストファイルにコピーアンドペーストし、そのサイズに注意して下さい。
5. amadminの「iPlanetDirectoryPro」Cookieにアクセスすることができないプライベートブラウザセッションを起動します:
 - Chromeでは、シークレットウィンドウを開きます。
 - Internet Explorerでは、プライベートブラウジングを開始します。
 - Firefoxでは、新しいプライベートウィンドウを開きます。
 - Safariでは、新しいプライベートウィンドウを開きます。
6. ステートレスセッションを有効にしたレルムに対して、管理者以外のユーザーとしてOpenAMにログインします。この時、管理者ユーザーとしてログインしないように注意して下さい。
7. ブラウザ内の、「iPlanetDirectoryPro」Cookieを調べます。Cookieの値を2番目のテキストファイルにコピーアンドペーストし、そのサイズに注意して下さい。ステートレスセッションCookieの値の大きさは、amadminユーザーのためのステートフルセッションCookieの値のサイズよりもかなり大きいはずです。クッキーが大きくない場合は、正しくステートレス・セッションが有効になっていません。
8. 管理コンソールが表示されている元のブラウザウィンドウに戻ります。
9. 「セッション」画面を表示しているウィンドウをリフレッシュします。
10. amadminのセッションがまだ表示されていて、ステートレスセッションが有効になっているレルムの管理者以外のユーザーのセッションが表示されないことを確認します。

### ステートレスセッションCookieのセキュリティの設定

ステートレスセッションを使用するときは、「iPlanetDirectoryPro」CookieにJWTを署名し、暗号化する必要があります。

ステートレスセッションCookieのセキュリティを設定する前に、必要に応じて証明書を配備していることを確認してください。OpenAMの証明書の管理の詳細については、証明書の管理を参照してください。

ステートレスセッションCookieのJWTのセキュリティを確保するには、JWTに署名を設定し、JWT全体を暗号化します。次のセクションでは、ステートレスセッションCookieのセキュリティを設定するための詳細な手順を提供します。

ステートレスセッションCookieのセキュリティの詳細については、ステートレスセッションCookieのセキュリティを参照してください

> **重要**  
> OpenAMサイト内に複数のOpenAMサーバーを配備する場合、すべてのサーバーが同じセキュリティ設定になっている必要があります。共有シークレットとセキュリティキーは同一でなければなりません。共有シークレットまたはキーを変更する場合は、サイト上のすべてのサーバーに変更を加える必要があります。

#### JWTの署名の設定

ステートレスセッションCookieの悪質な改ざんを防止するためには、JWTの署名の設定が必要です。

JWTの署名を設定するには、次の手順を実行します:

**手順. JWTの署名の設定する**

1. 設定 > グローバル > セッション に移動し、ステートレスセッションのセクションを見つけます。
2. 署名アルゴリズムのタイプを指定します。デフォルト値はHS256です。
3. HMAC署名アルゴリズムを指定した場合は、署名HMAC共有秘密のフィールドの値を変更します(生成されたデフォルト値を使用したくない場合)。
4. RS256の署名アルゴリズムを指定した場合は、JWTの署名の署名に使用する署名RSA証明書エイリアスのフィールドに値を指定します。
5. 保存ボタンをクリックします。

セッションサービスの設定属性の詳細については、OpenAM Referenceの「セッション」のエントリを参照してください。

#### JWTの暗号化の設定

man-in-the-middleの攻撃者がユーザーのセッションの詳細へのアクセスすことや、エンドユーザーがJWTのコンテンツを調べることを防止するに、JWTの暗号化を設定します。

JWTを暗号化するには、次の手順を実行します:

**手順. JWTの暗号化を設定する**

Navigate to Configuration > Global > Session and then locate the Stateless Sessions section.

Specify the Encryption Algorithm Type as a value other than NONE.

Specify a value in the Encryption RSA Certificate Alias to use for encrypting the JWT signature.

Click Save.

Ensure that the JWT signature configuration is identical on every OpenAM server in your OpenAM site.

For detailed information about Session Service configuration attributes, see the entries for Session.

### Configuring Session Blacklisting

Session blacklisting ensures that users who have logged out of stateless sessions cannot achieve single sign-on without reauthenticating to OpenAM.

Perform the following steps to configure session blacklisting:

**手順. To Configure OpenAM for Session Blacklisting**

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

次のOpenAMの機能は、ステートレスセッションを使用するレルムでサポートされていません。

- 認証レベルとセッションアップグレード
- セッション割り当ての設定
- 現在のセッションプロパティを参照する条件を含む認可ポリシー
- クロスドメインシングルサインオン
- SAML v2.0シングルサインオンとシングルログアウト
- SAML v1.xシングルサインオン
- SNMPセッション監視
- OpenAMコンソールを使用したセッション管理
- セッション通知
