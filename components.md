[TODO 作成中]

OpenAMは、もともとantでビルドされていましたが、バージョン11.0.0からMavenに移行しています。これにより、マルチプロジェクト構成のMavenプロジェクトとなり、「OpenAM Project」というトッププロジェクトの配下に以下のようなサブプロジェクトを持っています。

|ファイル名|プロジェクト名|説明|
|---|---|---|
|openam-annotations|OpenAM Annotations|OpenAM Annotations|
|openam-audit|OpenAM Audit|OpenAM Audit Modules|
|openam-audit\openam-audit-configuration|OpenAM Audit Configuration||
|openam-audit\openam-audit-context|OpenAM Audit Context||
|openam-audit\openam-audit-core|OpenAM Audit Core||
|openam-audit\openam-audit-rest|OpenAM Audit REST||
|openam-authentication|OpenAM Authentication|OpenAM Authentication Modules|
|openam-authentication\deviceprint|OpenAM Auth Device Print|OpenAM Authentication Device Print|
|openam-authentication\deviceprint\module|OpenAM Auth Device Print Module||
|openam-authentication\deviceprint\scripts|OpenAM Auth Device Print Module Scripts||
|openam-authentication\openam-auth-ad|OpenAM Auth AD|OpenAM Authentication AD|
|openam-authentication\openam-auth-adaptive|OpenAM Auth Adaptive|OpenAM Authentication Adaptive|
|openam-authentication\openam-auth-anonymous|OpenAM Auth Anonymous|OpenAM Authentication Anonymous|
|openam-authentication\openam-auth-application|OpenAM Auth Application|OpenAM Authentication Application|
|openam-authentication\openam-auth-cert|OpenAM Auth Cert|OpenAM Authentication Cert|
|openam-authentication\openam-auth-common|OpenAM Auth Common|OpenAM Authentication Common Utilities|
|openam-authentication\openam-auth-datastore|OpenAM Auth Datastore|OpenAM Authentication Datastore|
|openam-authentication\openam-auth-device-id|OpenAM Auth Device Id|OpenAM Authentication Device Id|
|openam-authentication\openam-auth-fr-oath|OpenAM Auth OATH|OpenAM Authentication OATH|
|openam-authentication\openam-auth-hotp|OpenAM Auth HOTP|OpenAM Authentication HOTP|
|openam-authentication\openam-auth-httpbasic|OpenAM Auth HttpBasic|OpenAM Authentication HTTP Basic |
|openam-authentication\openam-auth-jdbc|OpenAM Auth JDBC|OpenAM Authentication JDBC|
|openam-authentication\openam-auth-ldap|OpenAM Auth LDAP|OpenAM Authentication LDAP|
|openam-authentication\openam-auth-membership|OpenAM Auth Membership|OpenAM Authentication Membership|
|openam-authentication\openam-auth-msisdn|OpenAM Auth MSISDN|OpenAM Authentication MSISDN|
|openam-authentication\openam-auth-nt|OpenAM Auth NT|OpenAM Authentication NT|
|openam-authentication\openam-auth-oath|OpenAM Auth OATH|OpenAM Authentication OATH|
|openam-authentication\openam-auth-oauth2|OpenAM Auth OAUTH2|OpenAM Authentication OAUTH2|
|openam-authentication\openam-auth-oidc|OpenAM AuthN OpenId Connect|OpenAM authentication module which consumes the jaspi OpenId Connect authentication module domain logic|
|openam-authentication\openam-auth-persistentcookie|OpenAM Auth Persistent Cookie|OpenAM Authentication Persistent|
|openam-authentication\openam-auth-radius|OpenAM Auth Radius|OpenAM Authentication Radius|
|openam-authentication\openam-auth-saml2|OpenAM Auth SAML2|OpenAM Authentication SAML2|
|openam-authentication\openam-auth-scripted|OpenAM Auth Scripted|OpenAM Authentication Scripted|
|openam-authentication\openam-auth-securid|OpenAM Auth SecurID|OpenAM Authentication SecurID|
|openam-authentication\openam-auth-windowsdesktopsso|OpenAM Auth WindowsDesktopSSO|OpenAM Authentication WindowsDesktopSSO|
|openam-cli|OpenAM CLI Base|OpenAM Command Line Interface|
|openam-cli\openam-cli-definitions|OpenAM CLI Definitions|OpenAM Command Line Definitions|
|openam-cli\openam-cli-impl|OpenAM CLI Implementation|OpenAM Command Line Implementation|
|openam-clientsdk|OpenAM Client SDK|OpenAM Java Client SDK|
|openam-common-auth-ui|OpenAM Common Auth UI|OpenAM Common Auth UI|
|openam-console|OpenAM Admin Console|OpenAM Admin Console|
|openam-core|OpenAM Core|OpenAM Core Components|
|openam-core-rest|OpenAM Core REST|OpenAM Core REST|
|openam-coretoken|OpenAM Core Token|OpenAM CoreToken Services|
|openam-dashboard|OpenAM Dashboard|OpenAM Dashboard Services|
|openam-datastore|OpenAM User Data Store|OpenAM User Data Store|
|openam-distribution|OpenAM Distribution Packaging|OpenAM Final Module Distribution Package.|
|openam-distribution\openam-distribution-diagnostics|OpenAM Distribution Diagnostics|OpenAM Distribution Diagnostics|
|openam-distribution\openam-distribution-fedlet-unconfigured|OpenAM Distribution Fedlet UnConfigured|OpenAM Distribution Fedlet UnConfigured.|
|openam-distribution\openam-distribution-kit|OpenAM Distribution Kit|OpenAM Distribution Kit, containing all distributable artifacts.|
|openam-distribution\openam-distribution-ssoadmintools|OpenAM Distribution ssoAdminTools|OpenAM Distribution SSO Admin Tools Kit.|
|openam-distribution\openam-distribution-ssoconfiguratortools|OpenAM Distribution ssoConfiguratorTools|OpenAM Distribution SSO Configurator Tools Kit.|
|openam-documentation|OpenAM Documentation||
|openam-documentation\openam-doc-log-message-ref|OpenAM Log Message Reference||
|openam-documentation\openam-doc-ssoadm-ref|OpenAM ssoadm Reference|Tools for creating/generating the core documentation.|
|openam-entitlements|OpenAM Entitlements|OpenAM Entitlements|
|openam-examples|OpenAM Example Projects|OpenAM Example Projects|
|openam-examples\openam-example-clientsdk-cli|OpenAM Example ClientSDK CLI|OpenAM Example ClientSDK CLI module|
|openam-examples\openam-example-clientsdk-war|OpenAM Example ClientSDK WAR|OpenAM Example ClientSDK WAR|
|openam-federation|OpenAM Federation|OpenAM Federation|
|openam-federation\openam-federation-library|OpenAM Federation Library|OpenAM Federation Library Components|
|openam-federation\openam-idpdiscovery|OpenAM IdP Discovery|OpenAM IdP Discovery|
|openam-federation\openam-idpdiscovery-war|OpenAM IdP Discovery WAR|OpenAM IdP Discovery WAR|
|openam-federation\OpenFM|OpenFM|OpenAM Federation|
|openam-http|OpenAM HTTP|OpenAM HTTP integration|
|openam-http-client|OpenAM HTTP client|Common HTTP client implementation used by ForgeRock OpenAM custom scripting modules|
|openam-ldap-utils|OpenAM LDAP Utilities|OpenAM LDAP Utilites|
|openam-oauth|OpenAM OAuth|OpenAM OAuth Modules|
|openam-oauth2|OpenAM OAuth2|OpenAM OAuth2 integration|
|openam-oauth2-common|OpenAM OAuth2 Common|OAuth2 Commons library|
|openam-oauth2-common\oauth2-core|OAuth2 Core|OAuth2 Core library|
|openam-oauth2-common\oauth2-restlet|OAuth2 Restlet Integration|OAuth2 Restlet library to provide HTTP integration|
|openam-oauth2-common\openid-connect-core|OpenID Connect Core||
|openam-oauth2-common\openid-connect-restlet|OpenId Connect Restlet Integration||
|openam-oauth2-saml2|OpenAM OAuth2 SAML2 Grant Flow|OpenAM OAuth2 SAML2 integration|
|openam-plugins|OpenAM Plugins|OpenAM plugins|
|openam-plugins\openam-auth-postauthentication|OpenAM PostAuthN plugins|OpenAM Post AuthN Plugin|
|openam-radius|OpenAM RADIUS|OpenAM RADIUS server and common RADIUS libraries|
|openam-radius\openam-radius-common|OpenAM RADIUS common library.|OpenAM Radius common library. Used by the openam-radius-server and openam-auth-radius modules|
|openam-radius\openam-radius-server|OpenAM RADIUS Server|Provides a RADIUS server that runs as a service inside OpenAM and uses OpenAM |
|openam-rest|OpenAM Rest|OpenAM REST Services|
|openam-restlet|OpenAM Restlet|OpenAM Restlet customisation|
|openam-schema|OpenAM Schemata|OpenAM Schemata Components|
|openam-schema\openam-diagnostics-schema|OpenAM Diagnostics Schema|OpenAM Schemata Components|
|openam-schema\openam-dtd-schema|OpenAM DTD Schema|OpenAM Identity Services DTD Components|
|openam-schema\openam-idsvcs-schema|OpenAM Identity Services Schema|OpenAM Identity Services Schema Components|
|openam-schema\openam-jaxrpc-schema|OpenAM JAXRPC Schema|OpenAM JAXRPC Schema Components|
|openam-schema\openam-liberty-schema|OpenAM Liberty Schema|OpenAM Liberty Schema Components|
|openam-schema\openam-mib-schema|OpenAM MIB Schema|OpenAM MIB Schema Components|
|openam-schema\openam-saml2-schema|OpenAM SAML2 Schema|OpenAM SAML2 Schema Components|
|openam-schema\openam-wsfederation-schema|OpenAM WS Federation Schema|OpenAM Schemata Components|
|openam-schema\openam-xacml3-schema|OpenAM XACML3 Schema|OpenAM XACML3 Schemata|
|openam-scripting|OpenAM Scripting|Scripting support for OpenAM auth modules|
|openam-selfservice|OpenAM Self Service|Home for OpenAM self service integration|
|openam-server|OpenAM Server|OpenAM Server Component|
|openam-server-auth-ui|OpenAM Server Auth UI|OpenAM Server Auth UI|
|openam-server-only|OpenAM Server Only|OpenAM Server Only Component|
|openam-shared|OpenAM Shared|OpenAM Shared Components and Modules|
|openam-slf4j|OpenAM slf4j binding||
|openam-sts|OpenAM STS||
|openam-sts\openam-client-sts|OpenAM STS Client Classes|Classes necessary to programmatically publish rest-sts instances and to consume them|
|openam-sts\openam-common-sts|OpenAM STS Common|Classes common to the REST and SOAP STS|
|openam-sts\openam-publish-sts|OpenAM STS Publish Service|OpenAM implementation of the service which allows soap and rest sts instances to be published|
|openam-sts\openam-rest-sts|OpenAM REST STS|OpenAM implementation of RESTful SecureTokenService|
|openam-sts\openam-soap-sts|OpenAM Soap STS||
|openam-sts\openam-soap-sts\openam-soap-sts-client|OpenAM SOAP STS Client|OpenAM WS-Trust SecureTokenService Client|
|openam-sts\openam-soap-sts\openam-soap-sts-server|OpenAM SOAP STS Server|OpenAM implementation of WS-Trust SecureTokenService|
|openam-sts\openam-token-service-sts|OpenAM STS Token Service|Token generation and validation service consumed by the STS|
|openam-test|OpenAM Common Test Suite|OpenAM Common TEST Suite|
|openam-tools|OpenAM Tools|OpenAM Tool Components|
|openam-tools\build-helper-plugin|OpenAM Build Helper Maven Plugin||
|openam-tools\openam-build-tools|OpenAM Build Tools|OpenAM Build Tools|
|openam-tools\openam-configurator-tool|OpenAM Configurator Tool|OpenAM Configurator Tool|
|openam-tools\openam-diagnostics|OpenAM Diagnostics|OpenAM Diagnostic Components|
|openam-tools\openam-diagnostics\openam-diagnostics-base|OpenAM Diagnostics Base|OpenAM Diagnostic Base Components|
|openam-tools\openam-diagnostics\openam-diagnostics-plugins|OpenAM Diagnostics Plugins|OpenAM Diagnostic Plugin Components|
|openam-tools\openam-installer-utils|OpenAM Installer Utils|OpenAM Installer Utils|
|openam-tools\openam-installtools|OpenAM Install Tools|OpenAM Install Tool Components|
|openam-tools\openam-installtools-launcher|OpenAM Install Tools Launcher|OpenAM Install Tools Launcher|
|openam-tools\openam-license-core|OpenAM License Core|OpenAM License Core|
|openam-tools\openam-license-manager-cli|OpenAM CLI License Manager|OpenAM Command Line Interface License Manger|
|openam-tools\openam-license-servlet|OpenAM ServletContext License Locator|OpenAM ServletContext License Locator|
|openam-tools\openam-upgrade-tool|OpenAM Upgrade Tool|OpenAM Upgrade Tool|
|openam-ui|OpenAM UI Parent||
|openam-ui\openam-ui-ria|OpenAM RIA Web UI||
|openam-uma|OpenAM UMA|OpenAM User Managed Access|
|openam-upgrade|OpenAM Upgrade|Upgrade support for OpenAM|
