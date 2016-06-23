### OpenDJの概要

OpenDJは、Sun Microsystems社により開発されたOpenDSやSun Java Directory Serverの後継となるオープンソースのディレクトリサーバーです。
エンタープライズ環境での利用に耐えうる性能、可用性、スケーラビリティおよびセキュリティを実現するための優れた機能を備えています。
OpenDJは、設定データストアとしてOpenAMの組み込まれており、ユーザーデータストアなどにも利用できます。

表. OpenDJの基本情報  

<table>
  <tbody>
    <tr>
      <td>機能</td>
      <td>ディレクトリサービス</td>
    </tr>
    <tr>
      <td>サポートするOS</td>
      <td>マルチプラットフォーム(Windows、Mac OS X、Linux)</td>
    </tr>
    <tr>
      <td>対応言語</td>
      <td>マルチランゲージ（日本語にも対応）</td>
    </tr>
    <tr>
      <td>実装言語</td>
      <td>Java</td>
    </tr>
    <tr>
      <td>ライセンス</td>
      <td>CDDL</td>
    </tr>
    <tr>
      <td>Webサイト</td>
      <td>http://www.forgerock.com/opendj.html</td>
    </tr>
  </tbody>
</table>

### OpenDJの特徴

- オープンソース：今日の市場で利用可能な唯一の100％商用オープンソースLDAPディレクトリサーバーです。
- マルチプロトコル対応：柔軟なデータモデルにより、LDAPだけでなくREST、SCIM、Webサービスでアクセスすることができます。
- 高い信頼性と性能：最も要求の厳しいSLA環境でも、高いスループットと短い応答時間を実現します。

### OpenDJの主な機能

- 性能、スケーラビリティ、高可用性

 - OpenDJ provides industry-leading performance with sub-millisecond read/write response times and low latency throughput, up to hundreds of thousands of operations per second
 - Supports HA deployments with N-way multi-master replication, including data centers with geographic separation for managing failover and disaster recovery
 - Meets the most rigorous SLA requirements, from telco subscriber systems to mission-critical enterprise environments

- Speaks your language!

 - Provides access through REST API, SCIM, LDAP, and Web Services (DSMLv2) to ensure maximum interoperability with client application
 - OpenDJ SDK for Java provides a library of classes and interfaces for accessing and implementing LDAP Directory Services

- Replication

 - N-way multi-master replication ensures high-availability and disaster recovery capabilities
 - Assured replication can guarantee data availability in the event of server failure
 - Supports WAN-optimized replication for increased bandwidth efficiency

- Security
- Pass-Through Authentication
- User and Account Management
- Easy Setup and Administration
- Monitoring and Alerts
- Logging and Auditing
- Localization
- Supported Standards
- 100% Java-based architecture
- 100% open source

