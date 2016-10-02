[TODO 作成中]

## 認証レベルについて

ユーザーが正常に認証すると、OpenAMはセッションを作成し、それによりOpenAMはリソースへのユーザーのアクセスを管理することができます。セッションには認証レベルが割り当てられており、認証が成功した認証モジュールの最も高い認証レベルであると計算されます。ユーザーのセッションが適切な認証レベルを持っていない場合は、ユーザーが要求されたリソースにアクセスするために、より高い認証レベルで再度認証を要求される場合があります。

認証連鎖に、「必須」または「必要」の認証モジュールが含まれており、それらの前に「十分」の認証モジュールが存在するため実行されなかった場合、セッションの認証レベルは、次のどちらか大きい方で計算されます。
- 通過した認証モジュールの中で最も高い認証レベル
- 実行されされなかった「必須」または「必要」の認証モジュールの最も高い認証レベル

You can modify OpenAM's default behavior, so that a session's authentication level is always the highest authentication level of any authentication module that passed, even if there are requisite or required modules in the authentication chain that were not executed.

To modify the default behavior, set the org.forgerock.openam.authLevel.excludeRequiredOrRequisite property to true under Deployment > Servers > Server Name > Advanced and restart the OpenAM server.

In some deployments, you need to limit how many active sessions a user can have at a given time. For example, you might want to prevent a user from using more than two devices at once. See Configuring Session Quotas for instructions.

### 認証レベルとセッションアップグレード

管理コンソールで認証モジュールの設定画面を見ると、全ての認証モジュールには認証レベルが設定されていることが分かります。

図. 認証レベルの設定

![図. 認証レベルの設定](images/authentication/authn-level.png)

認証レベルには、モジュールに関連付けられているセキュリティレベルを設定します(より強固な認証形態にはより高い認証レベルが割り当てられています)。（または配備が低い認証レベルで強力な認証を定義する場合は、より低い認証レベル。）認証に成功すると、ユーザーのセッションには実現した認証レベルに関する情報が含まれます。

認可ポリシーは、機密リソースへのアクセスのために特定の認証レベルを要求することができます（指定された認証レベル以上または以下）。すでにレルムで認証されたユーザーが、必要な認証レベルを持っていない有効なセッションで、センシティブなリソースにアクセスしようとすると、OpenAMはリソースへのアクセスを拒否します。しかし、OpenAMは、認可決定とともにアドバイスも返します。アドバイスは、必要な認証レベルの必要性を示します。ポリシーエージェントまたはポリシー実施ポイントは、セッションアップグレードのためにOpenAMにユーザーを送信することができます。

セッションアップグレード中に、ユーザーはより強力な認証モジュールで認証します。強力なモジュールは、一般的に元の認証を処理したモジュールと同じ認証連鎖の一部として構成します(センシティブではない、リソースにアクセスするために必要ではないが)。より強力な認証が成功すると、ユーザーセッションは新しい認証レベルにアップグレードし、より強力な認証に関連するすべての設定を含むように修正されます。

失敗した場合、セッションアップグレードはユーザーセッションをより強力な認証での試みがあった以前のままにします。ログインページのタイムアウトのためセッションのアップグレードが失敗した場合、最後に成功した認証からOpenAMは成功URLにユーザーのブラウザをリダイレクトします。

OpenAMのポリシーエージェントは、OpenAMのアドバイスを処理するようにビルドされているため、一般的に追加設定無しでセッションアップグレードを処理できます。独自のポリシー実施ポイント（PEP）を構築した場合は、アドバイスやセッションアップグレードを考慮に入れて下さい。RESTfulなPEPについては、ポリシー決定の要求を参照してください。アドバイスやセッションアップグレードを処理する方法については、認証とログアウトを参照してください。

セッションアップグレードのOpenAMのサポートは、ステートフルセッションを必要とします。OpenAMのセッションをアップグレードしようとする前に、OpenAMがステートフルセッション用に設定されていることを確認してください(デフォルト設定)。
