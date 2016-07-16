
### OpenAMのアーキテクチャ

OpenAM is an open source centralized access management server, securing protected resources across the network and providing authentication, authorization, Web security, and federation services in a single, integrated solution. OpenAM manages access to the protected resources by controlling who has access, when, how long, and under what conditions by centralizing disparate hardware and software services for cloud, enterprise, mobile, and business-to-business (B2B) systems.

図. "OpenAM Architecture" illustrates the OpenAM architecture.

![OpenAM Architecture](images/openam-architecture-dpg.png)

OpenAM features a highly modular and flexible architecture with multiple plugin points to meet any customer deployment. It leverages industry standard protocols, such as HTTP, XML, SOAP, REST, SAML 2.0, OAuth 2.0, OpenID Connect 1.0, and so forth to deliver a high performance, highly scalable, and highly available access management solution over the network. OpenAM services are 100% Java-based, proven across multiple platforms and containers in many production deployments.

OpenAM core server can be deployed and integrated within existing network infrastructures. OpenAM provides the following distribution files:
Table 1.1. OpenAM Distribution Files
Distribution	Description	Deploy ?
Default	OpenAM's default distribution .war file includes the core server code with an embedded OpenDJ directory server, which stores configuration data and simplifies deployments. The distribution includes an administrative graphical user interface (GUI) Web console. During installation, the .war file accesses a bootstrap file to obtain the fully qualified domain name, port, context path, and the location of the configuration folder. 	Y	N
Core Server Only	OpenAM provides a core server-only .war file without the OpenAM Console. This setup is often used in multi-server deployments wherein the deployments is managed using a full server instance using the ssoadm command-line tool. The OpenAM server installs with an embedded OpenDJ directory server. 	Y	N
Identity Provider (IdP) Discovery Service	OpenAM provides an IdP Discovery Profile (SAMLv2 binding profile) for its IdP Discovery service. The profile keeps track of the identity providers for each user. 	Y	N
OpenAM Client Java SDK	OpenAM provides a smaller subset of the OpenAM SDK, providing client-side API to OpenAM services. The Client SDK allows you to write remote standalone or Web applications to access OpenAM and run its core services. 	Y	N

ForgeRock's OpenAM product is built on open-source code. ForgeRock maintains the OpenAM product, providing the community an open-source code repository, issue tracking, mailing lists, Web sites, and continuous integration as well as nightly builds from the latest code. ForgeRock also provides commercial releases of fully tested builds. ForgeRock offers the services you need to deploy OpenAM commercial builds into production, including training, consulting, and support.
