OpenAM 13.0.0では以下の認証モジュールが提供されています。

|モジュールタイプ|機能概要|備考|
|---|---|---|
|Active Directory|Active Directoryで管理されたユーザーで認証を行うための認証モジュール。||
|Device Id (Save)|||
|ForgeRock Authenticator (OATH)|||
|HOTP|HMACワンタイムパスワード（HOTP）認証モジュール。データストア認証モジュールと連携して動作し、ユーザーのメールアドレスや電話番号にワンタイムパスワードを送信する。||
|HTTP 基本|HTTPで定義される認証方式の一つであるBasic認証を使用する認証モジュール。認証ダイアログで入力したIDとパスワードが適正であるか、LDAP認証モジュールを使って検証する。||
|JDBC|MySQLやOracle DBなどのRDBMSで管理されたユーザーで認証を行うための認証モジュール。||
|LDAP|OpenLDAPやOpenDJで管理されたユーザーで認証を行うための認証モジュール。||
|MSISDN|Mobile Station Integrated Services Digital Network（MSISDN）認証モジュール。携帯電話などの端末に関連付けられたMSISDN番号を使用して、非対話的な認証を可能にする。||
|OATH|||
|OAuth 2.0|OAuth 2.0のクライアントアプリケーションとなり、FacebookやWindows Liveなどに認証を委譲する。||
|OpenID Connect id_token bearer|||
|RADIUS|RADIUS（Remote Authentication Dial In User Service）サーバによるユーザー認証を実行する認証モジュール。||
|SAE|||
|SAML2|||
|Windows NT|||
|Windows デスクトップ SSO|||
|アダプティブリスク|||
|データストア|||
|メンバーシップ|||
|持続 Cookie|||
|証明書|||
|匿名|||
|連携|||
