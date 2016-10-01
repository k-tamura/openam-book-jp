[TODO 作成中]

## 認証レベルについて

When a user successfully authenticates, OpenAM creates a session, which allows OpenAM to manage the user's access to resources. The session is assigned an authentication level, which is calculated to be the highest authentication level of any authentication module that passed. If the user's session does not have the appropriate authentication level, then the user may need to re-authenticate again at a higher authentication level to access the requested resource.

If an authentication chain contains requisite or required modules that were not executed due to the presence of a passing sufficient module in front of them, the session's authentication level is calculated to be whichever is greater: the highest authentication level of any authentication module that passed, or the highest authentication level of requisite or required modules that were not executed.

You can modify OpenAM's default behavior, so that a session's authentication level is always the highest authentication level of any authentication module that passed, even if there are requisite or required modules in the authentication chain that were not executed.

To modify the default behavior, set the org.forgerock.openam.authLevel.excludeRequiredOrRequisite property to true under Deployment > Servers > Server Name > Advanced and restart the OpenAM server.

In some deployments, you need to limit how many active sessions a user can have at a given time. For example, you might want to prevent a user from using more than two devices at once. See Configuring Session Quotas for instructions.

### 認証レベルとセッションアップグレード

管理コンソールで認証モジュールの設定画面を見ると、認証モジュールには認証レベルが設定されていることが分かります。認証レベルには、モジュールに関連付けられているセキュリティレベルを設定します(より強固な認証形態にはより高い認証レベルが割り当てられています)。（または配備が低い認証レベルで強力な認証を定義する場合は、より低い認証レベル。）認証に成功すると、ユーザーのセッションには実現した認証レベルに関する情報が含まれます。

認可ポリシーは、機密リソースへのアクセスのために特定の認証レベルを要求することができます（指定された認証レベル以上または以下）。すでにレルムで認証されたユーザーが、必要な認証レベルを持っていない有効なセッションで、センシティブなリソースにアクセスしようとすると、OpenAMはリソースへのアクセスを拒否します。しかし、OpenAMは、認可決定とともにアドバイスも返します。アドバイスは、必要な認証レベルの必要性を示します。ポリシーエージェントまたはポリシー実施ポイントは、セッションアップグレードのためにOpenAMにユーザーを送信することができます。

セッションアップグレード中に、ユーザーはより強力な認証モジュールで認証します。強力なモジュールは、一般的に元の認証を処理したモジュールと同じ認証連鎖の一部として構成します(センシティブではない、リソースにアクセスするために必要ではないが)。より強力な認証が成功すると、ユーザーセッションは新しい認証レベルにアップグレードし、より強力な認証に関連するすべての設定を含むように修正されます。

失敗した場合、セッションアップグレードはユーザーセッションをより強力な認証での試みがあった以前のままにします。ログインページのタイムアウトのためセッションのアップグレードが失敗した場合、最後に成功した認証からOpenAMは成功URLにユーザーのブラウザをリダイレクトします。

OpenAMのポリシーエージェントは、OpenAMのアドバイスを処理するようにビルドされているため、一般的に追加設定無しでセッションアップグレードを処理できます。独自のポリシー実施ポイント（PEP）を構築した場合は、アドバイスやセッションアップグレードを考慮に入れて下さい。RESTfulなPEPについては、ポリシー決定の要求を参照してください。アドバイスやセッションアップグレードを処理する方法については、認証とログアウトを参照してください。

セッションアップグレードのOpenAMのサポートは、ステートフルセッションを必要とします。OpenAMのセッションをアップグレードしようとする前に、OpenAMがステートフルセッション用に設定されていることを確認してください(デフォルト設定)。
