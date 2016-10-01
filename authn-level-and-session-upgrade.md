[TODO 作成中]

## 認証レベルとセッションアップグレード

認証モジュールの設定が示すように、認証モジュールには認証レベルが設定されています。この設定では、モジュールに関連付けられているセキュリティレベルを設定します(より強固な認証形態にはより高い認証レベルが割り当てられています)。（または配備が低い認証レベルで強力な認証を定義する場合は、より低い認証レベル。）認証に成功すると、ユーザーのセッションには実現した認証レベルに関する情報が含まれます。

認可ポリシーは、機密リソースへのアクセスのために特定の認証レベルを要求することができます（指定された認証レベル以上または以下）。すでにレルムで認証されたユーザーが、必要な認証レベルを持っていない有効なセッションで、センシティブなリソースにアクセスしようとすると、OpenAMはリソースへのアクセスを拒否します。しかし、OpenAMは、認可決定とともにアドバイスも返します。アドバイスは、必要な認証レベルの必要性を示します。ポリシーエージェントまたはポリシー実施ポイントは、セッションアップグレードのためにOpenAMにユーザーを送信することができます。

During session upgrade the user authenticates with a stronger authentication module. The stronger module is typically part of the same authentication chain that handled the original authentication, though not required for access to less sensitive resources. Upon successful stronger authentication, the user session is upgraded to the new authentication level and modified to include any settings related to the stronger authentication.

If unsuccessful, session upgrade leaves the user session as it was before the attempt at stronger authentication. If session upgrade failed because the login page times out, OpenAM redirects the user's browser to the success URL from the last successful authentication.

OpenAM policy agents generally handle session upgrade without additional configuration, as policy agents are built to handle OpenAM's advices. If you build your own policy enforcement point (PEP), however, take advices and session upgrade into consideration. For RESTful PEPs, see Requesting Policy Decisions, and for indications on how to handle advices and session upgrade see Authentication and Logout.

OpenAM's support for session upgrades requires stateful sessions. Be sure that OpenAM is configured for stateful sessions—the default configuration—before attempting to upgrade OpenAM sessions.
