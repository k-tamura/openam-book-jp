## 監査ログの設定

OpenAMは、システムセキュリティ、トラブルシューティング、規制遵守にとって重要なキーとなる監査イベントを、キャプチャする包括的な監査ログサービスをサポートしています。

監査ログは、処理とセキュリティデータを追跡するために、認証メカニズム、システムへのアクセス、ユーザーと管理者のアクティビティ、エラーメッセージ、設定変更など、OpenAMの配備内で発生するイベントに関する操作情報を収集します。

監査ログサービスは、OpenAM、OpenIDM、OpenDJ、OpenIGを含むForgeRockスタック全体で共通の一貫性があり、文書化、構造化されたメッセージフォーマットを使用しています。

注意:
OpenAM 13では、次の2つの監査ログサービスをサポートしています。
- レガシーログサービス: Java SDKベースで、OpenAM 13より前のバージョンで使用可能。今後のリリースでは廃止される予定です。
- 新しいRESTベースの共通監査ログサービス: OpenAM 13で利用可能

### 監査ログサービスについて

OpenAMは、自身のインスタンス、ポリシーエージェント、ssoadmツールなどによって、トリガーされる監査イベントから生成されるログメッセージを書き込みます。

次のようにOpenAMの監査ログサービスは、汎用的で豊富な機能セットを提供します:

- グローバル、レルムベースのログ設定
すべてのレルムが継承することを保証する、グローバル監査ログを設定することができます。
レルム毎に異なる監査​​ログを設定することを可能です。

- 監査イベントハンドラ
監査ログサービスは、様々な監査イベントハンドラのサポートしており、複数のシステムにログを書くことができます:
 - CSVハンドラ
 - Syslogハンドラ
 - JDBCハンドラ

- 監査イベントバッファリング  
デフォルトでOpenAMは、生成された各ログメッセージを別々に書き込みます。 
メモリにログメッセージを保持して、事前設定した時間や数の閾値に達した後にバッファをフラッシュする、メッセージバッファリングをサポートします。  

- 改ざん防止用ロギング  
ログに不正な改ざんが行われていないことを確かにするために、監査ログにデジタル署名することができます。
この機能を設定するには、事前設定されたロガーの証明書をデプロイし、/path/to/openam/openam/Logger.jksでそれを保存する必要があります。  

- ログローテーションと保持ポリシー  
デフォルトでOpenAMは、指定された最大サイズに達すると監査ログをローテーションします。
サイズベースのローテーションポリシーを無効にし、時間ベースで設定することができます。
また、ログのローテーションを無効にして、外部のログローテーションツールを使用するオプションもあります。  

- センシティブなフィールドのブラックリスト化  
監査ログサービスはブラックリスト登録をサポートしています。HTTPヘッダ、クエリーパラメータ、クッキー、全体のフィールド値などのセンシティブな値やフィールドを非表示にするフィルタリングできます。  

- DNSルックアップの反転  
監査ログサービスは、ネットワークのトラブルシューティングのために、リバースDNSルックアップ機能をサポートしています。
性能面を考慮して、リバースDNSルックアップはデフォルトで無効になっています。

### Audit Log Topics

OpenAM integrates log messages based on four different audit topics. A topic is a category of audit log event that has an associated one-to-one mapping to a schema type. Topics can be broadly categorized as access details, system activity, authentication operations, and configuration changes. The following table shows the basic event topics and associated audit log files, whose filenames are fixed:
Table 6.1. Audit Log Topics
Event Topic	File Name	Description
Access	access.csv 	

Captures who, what, when, and output for every access request.
Activity	activity.csv 	

Captures state changes to objects that have been created, updated, or deleted by end users (that is, non-administrators). For this release, only session changes are captured in the logs.

Future releases may also record changes to user trusted devices, UMA policies, OAuth 2.0 tokens and others.
Authentication	authentication.csv 	

Captures when and how a subject is authenticated and related events.
Configuration	config.csv 	

Captures configuration changes to the product with a timestamp and by whom. Note that the userId indicating the subject who made the configuration change is not captured in the config.csv but may be tracked using the transactionId in the access.csv.

### Audit Log Location

OpenAM stores its audit logs by default at the following location:

/path/to/openam/openam/log/

You can change the default global location using the OpenAM console by navigating to Realms > Realm Name > Services > Audit Logging > CSV Handler Name. In the Log Directory box, change the default log location.
6.4. Configuring Audit Event Handlers

OpenAM supports three types of audit event handlers: comma-separated values (CSV), syslog, and JDBC. The handlers publish audit events to CSV files, syslog daemons, and relational databases, respectively.
Procedure 6.1. To Configure the CSV Audit Event Handler

OpenAM's default audit event handler is the comma-separated values (CSV) handler, which is already configured for the Global Audit Logging service. The global audit service will be the default audit service for any realm that does not have the audit logging service added to it.
Important

By default, OpenDJ 3.0 does not have audit logging enabled; thus, administrators must manually enable audit logging in the directory server. For more information, see To Enable LDAP CSV Access Logs in the OpenDJ Administration Guide.

    Log in to the OpenAM console as an administrator, and select the global Audit Logging service at Configuration > Global > Audit Logging.

    On the Select Audit Event Handler page, click Global CSV Handler.

    Under General Handler Configuration, verify that the Enabled box is checked.

    Select the topics for your audit logs. For a description of each topic, see Audit Log Topics .

    In the Log Directory box, override the default location of your logs if necessary. The default location is: %BASE_DIR%/%SERVER_URI%/log/.
    Warning

    It is very important that a different log directory be configured for each instance of the CSV audit event handler. If two instances are writing to the same file, it can interfere with log rotation and tamper-evident logs.

    For File Rotation, configure how files are rotated once they reach a specified file size or time interval. Enter the following parameters:

        For Rotation Enabled, keep the Enabled box check-marked. If disabled, OpenAM ignores log rotation and appends to the same file.

        For Maximum File Size, enter the maximum size of an audit file before rotation.

        Default: 100000000 bytes.

        OPTIONAL. For File Rotation Prefix, enter an arbitrary string that will be prefixed to every audit log to identify it. This parameter is used when time-based or size-based rotation is enabled.

        For File Rotation Suffix, enter a timestamp suffix based on the Java SimpleDateFormat that will be added to every audit log. This parameter is used when time-based or size-based log rotation is enabled.

        Default: -MM.dd.yy-kk.mm.

        For Rotation Interval, enter a time interval to trigger audit log file rotation in seconds. A negative or zero value disables this feature.

        Default: -1
        Note

        Any combination of the three rotation policies (maximum file size, periodic duration, and duration since midnight) can be implemented including none at all.

        For Rotation Times, enter a time duration after midnight to trigger file rotation, in seconds. For example, you can provide a value of 3600 to trigger rotation at 1:00 AM.
        Note

        Negative durations are not supported.

    For File Retention, determine how long log files should be retained in your system. Configure the following file retention parameters:

        For Maximum Number of Historical Files, enter a number for allowed backup audit files.

        Default: -1, which indicates an unlimited number of files and disables the pruning of old history files.

        For Maximum Disk Space, enter the maximum amount of disk space that the total number of audit files can store. A negative or zero value indicates that this policy is disabled.

        Default: -1, which indicates an unlimited amount of disk space.

        For Minimum Free Space Required, enter the minimum amount of disk space required to store audit files. A negative or zero value indicates that this policy is disabled.

        Default: -1, which indicates no minimum amount of disk space is required.

    For Buffering, configure if log events should be buffered in memory before they are written to the CSV file:

        For Buffering Enabled, click the Enabled box to start audit event buffering.

        The default buffer size is 5000 bytes.

        When buffering is enabled, all audit events are put into an in-memory buffer (one per handled topic), so that the original thread that generated the event can fulfill the requested operation, rather than wait for I/O to complete. A dedicated thread (one per handled topic) constantly pulls events from the buffer in batches and writes them to the CSV file. If the buffer becomes empty, the dedicated thread goes to sleep until a new item gets added.

        For Flush Each Event Immediately, click Enabled to write all buffered events before flushing.

        When the dedicated thread accesses the buffer, it copies the contents to an array to reduce contention, and then iterates through the array to write to the CSV file. The bytes written to the file can be buffered again in Java classes and the underlying operating system.

        When Flush Each Event Immediately is enabled, OpenAM flushes the bytes after each event is written. If the feature is disabled (default), the Java classes and underlying operation system determine when to flush the bytes.

    For Tamper Evident Configuration, set up the feature to detect any tampering of the audit logs.

    When tamper evident logging is enabled, OpenAM generates an HMAC digest for each audit log event and inserts it into each audit log entry. The digest detects any addition or modification to an entry.

    OpenAM also supports another level of tamper evident security by periodically adding a signature entry to a new line in each CSV file. The entry signs the preceding block of events, so that verification can establish if any of these blocks have been added, removed, or edited by some user.

        Click Is Enabled to turn on the tamper evident feature for CSV logs.

        In the Certificate Store Location field, enter the location of the keystore. You must manually create the keystore and place it in this location. You can use a simple script to create your Java keystore: create-keystore.sh.

        Default: %BASE_DIR%/%SERVER_URI%/Logger.jks

        In the Certificate Store Password field, enter the certificate password.

        In the Certificate Store Password (confirm), re-enter the certificate password.

        In the Signature Interval field, enter a value in seconds for OpenAM to generate and add a new signature to the audit log entry.

        Default: 900 (seconds)

    Click Add to save your changes.

    On the Audit Logging page, click Save.

Procedure 6.2. To Configure the Syslog Audit Event Handler

OpenAM can publish audit events to a syslog server, which is based on a widely-used logging protocol. You can configure your syslog settings on the OpenAM console.
Important

By default, OpenDJ 3.0 does not have audit logging enabled; thus, administrators must manually enable audit logging in the directory server. For more information, see Syslog Audit Event Handler Configuration in the OpenDJ Administration Guide.

    Log in to the OpenAM console as an administrator, and select the global Audit Logging service at Configuration > Global > Audit Logging.

    In the Event Handler Instances section, click New.

    On the Select Audit Event Handler page, click Syslog, and then click Next.

    On the Add Audit Event Handler page, enter a name for your event handler. For example, Syslog Audit Event Handler.

    Under General Handler Configuration, verify that the Enabled box is checked.

    Select the topics for your audit logs. For a description of each topic, see Audit Log Topics .

    In the Server hostname field, enter the hostname or IP address of the receiving syslog server.

    In the Server port field, enter the port of the receiving syslog server.

    Select the Transport protocol for your configuration: TCP or UDP.

    In the Connection timeout field, enter the number of seconds to connect to the syslog server. If the server has not responded in the specified time, a connection timeout occurs.

    For Buffering Enabled, click the Enabled box to start audit event buffering.

    When buffering is enabled, all audit events that get generated are formatted as syslog messsages and put into a queue. A dedicated thread constantly pulls events from the queue in batches and transmits them to the syslog server. If the queue becomes empty, the dedicated thread goes to sleep until a new item gets added. The default queue size is 5000.

    Select the syslog facility.

    A syslog message includes a PRI field that is calculated from the facility and severity values. All topics set the severity to INFORMATIONAL but allow you to choose the facility:
    Table 6.2. Syslog Facilities
    Facility	Description
    AUTH	Security or authorization messages
    AUTHPRIV	Security or authorization messages
    CLOCKD	Clock daemon
    CRON	Scheduling daemon
    DAEMON	System daemons
    FTP	FTP daemon
    KERN	Kernel messages
    LOCAL0	Local use 0 (local0)
    LOCAL1	Local use 1 (local1)
    LOCAL2	Local use 2 (local2)
    LOCAL3	Local use 3 (local3)
    LOCAL4	Local use 4 (local4)
    LOCAL5	Local use 5 (local5)
    LOCAL6	Local use 6 (local6)
    LOCAL7	Local use 7 (local7)
    LOGALERT	Log alert
    LOGAUDT	Log audit
    LPR	Line printer subsystem
    MAIL	Mail system
    NEWS	Network news subsystem
    NTP	Network time protocol
    SYSLOG	Internal messages generated by syslogd
    USER	User-level messages
    UUCP	Unix-to-unix-copy (UUCP) subsystem

    Click Add to save your settings.

Procedure 6.3. To Configure the JDBC Audit Event Handler

OpenAM supports audit logging to databases using the JDBC audit event handler. To ensure that the JDBC driver is accessible to the deployed container, install the JDBC .jar file that contains a JDBC driver class in the /path/to/tomcat/webapps/openam/WEB-INF/lib directory.

Depending on the JDBC driver type, configure OpenAM to write to Oracle, MySQL, or other databases.
Note

You can access database creation scripts for oracle and mysql at the following locations:

    openam/openam-server-only/src/main/webapp/WEB-INF/template/sql/mysql/audit.sql
    openam/openam-server-only/src/main/webapp/WEB-INF/template/sql/oracle/audit.sql

Important

By default, OpenDJ 3.0 does not have audit logging enabled; thus, administrators must manually enable audit logging in the directory server. For more information, see JDBC Audit Event Handler Configuration in the OpenDJ Administration Guide.

    Obtain the JDBC driver from your data base vendor. Place the Oracle or MySQL JDBC driver .zip or .jar file in the container's WEB-INF/lib classpath. For example, place the JDBC driver in /path/to/tomcat/webapps/openam/WEB-INF/lib if you use Apache Tomcat.

    Log in to the OpenAM console as an administrator, and select the global Audit Logging service at Configuration > Global > Audit Logging.

    In the Event Handler Instances section, click New.

    On the Select Audit Event Handler page, click JDBC, and then click Next.

    On the Add Audit Event Handler page, enter a name for your event handler. For example, JDBC Audit Event Handler.

    Under General Handler Configuration, verify that the Enabled box is checked.

    Select the topics for your audit logs. For a description of each topic, see Audit Log Topics .

    For Database Type, click one of the following:
        Oracle
        MySQL
        Other

    For JDBC Database URL, enter the URL for your database server. For example, jdbc:oracle:thin@//host.example.com:1521/ORCL.

    In the Database Driver Name field, enter the classname of the driver to connect to the datbase. For example, oracle.jdbc.driver.OracleDriver or com.mysql.jdbc.Driver.

    In the Database Username field, enter the username to authentcate to the database server.

    In the Database User Password field, enter the password used to authenticate to the database server. Then, re-enter the password in the Datbase User Password (confirm) field.

    In the Connection Timeout (seconds) field, enter the maximum wait time before failing the connection.

    Default: 30 (seconds)

    In the Maximum Connection Idle Timeout (seconds) field, enter the maximum idle time in seconds before the connection is closed.

    Default: 600 (seconds)

    In the Maximum Connection Time (seconds) field, enter the maximum time in seconds for a connection to stay open.

    Default: 1800 (seconds)

    In the Minimum Idle Connections field, enter tne minimum number of idle connections allowed in the connection pool.

    In the Maximum Connections box, enter the maximum number of connections in the connection pools.

    Click Add to save your changes.

    On the Audit Logging page, click Save.

### Configuring Audit Logging

You can easily enable the Audit Logging service on the OpenAM Admin console, either globally or on a per-realm basis.
Procedure 6.4. To Configure Global Audit Logging

    Log in to the OpenAM console as an administrator, and select the global Audit Logging service at Configuration > Global > Audit Logging.

    Make sure you have configured your audit event handler. See Configuring Audit Event Handlers .

    In the Realm Attributes section, click Enabled to turn on Audit Logging.

    For Resolve host name, click Enabled if you want to perform DNS host lookups, which populates the record's host name field in the logs. Note that enabling DNS host lookups may result in an overall performance hit due to the hostname searches.

    For Field exclusion policies, enter any fields or values to exclude from your audit events in the New Value field, and then click Add.

    The purpose of this feature is to allow customers to perform two kinds of filtering: 1) Filter fields from the event. For example, customers with more basic auditing requirements may not be interested in capturing HTTP headers, query parameters, and cookies in the access logs; 2) Filter specific values from within fields that store key-value pairs as JSON. For example, the HTTP headers, query parameters and cookies.

    On the Audit Logging page, click Save.

Procedure 6.5. To Configure Audit Logging per Realm

You can configure the audit logging server on a per realm basis, which allows you to set up different log locations for your realms and different types of handlers for each realm.

If no specific realm is configured, audit logging will be governed by the global settings.

    Log in to the OpenAM console as an administrator, and select the realm from which you want to work.

    Click Services > Add > Audit Loging, and then click Next.

    Make sure you have configured your audit event handler. See Configuring Audit Event Handlers .

    In the Realm Attributes section, click Enabled to turn on Audit Logging.

    For Resolve host name, click Enabled if you want to perform DNS host lookups, which populates the record's host name field in the logs. Note that enabling DNS host lookups may result in an overall performance hit due to the hostname searches.

    For Field exclusion policies, enter any fields or values to exclude from your audit events in the New Value field, and then click Add.

    The purpose of this feature is to allow customers to perform two kinds of filtering: 1) Filter fields from the event. For example, customers with more basic auditing requirements may not be interested in capturing HTTP headers, query parameters, and cookies in the access logs; 2) Filter specific values from within fields that store key-value pairs as JSON. For example, the HTTP headers, query parameters and cookies.

    On the Audit Logging page, click Save.

### Configuring the Legacy Audit Logging

To configure OpenAM logging properties, log in to the OpenAM console as OpenAM administrator, and browse to Configuration > System > Logging.

For more information on the available settings, see Audit Logging reference.
#### Audit Logging to Flat Files

By default, OpenAM audit logs are written to files in the configuration directory for the instance, such as $HOME/openam/log/.

OpenAM sends messages to different log files, each named after the service logging the message, with two different types log files per service: .access and.error. Thus, the current log files for the authentication service are named amAuthentication.access and amAuthentication.error.

For details, see Log Files and Messages.
#### Audit Logging to a Syslog Server

OpenAM supports sending audit log messages to a syslog server for collation.

You can enable syslog audit logging by using the OpenAM console, or the ssoadm command.
Procedure 6.6. Enabling Syslog Audit Logging by Using the OpenAM Console

    Log in to the OpenAM console as OpenAM administrator.

    Browse to Configuration > System > Logging.

    Set the Logging Type option to Syslog.

    Complete the following settings as appropriate for your syslog server:

        Syslog server host

        Syslog server port

        Syslog server protocol

        Syslog facility

        Syslog connection timeout

    For information on these settings, see Audit Logging.

    Save your work.

Procedure 6.7. Enabling Syslog Audit Logging by Using SSOADM

    Create a text file, for example, MySyslogServerSettings.txt containing the settings used when audit logging to a syslog server, as shown below:

      iplanet-am-logging-syslog-port=514
       iplanet-am-logging-syslog-protocol=UDP
       iplanet-am-logging-type=Syslog
       iplanet-am-logging-syslog-connection-timeout=30
       iplanet-am-logging-syslog-host=localhost
       iplanet-am-logging-syslog-facility=local5

     

Use the following SSOADM command to configure audit logging to a syslog server:

    $ ssoadm \
          set-attr-defs \
          --adminid amadmin \
          --password-file /tmp/pwd.txt \
          --servicename iPlanetAMLoggingService \
          --schematype Global \
          --datafile MySyslogServerSettings.txt


          Schema attribute defaults were set.
         

#### Audit Logging in OpenAM Policy Agents

By default, OpenAM Policy Agents log to local files in their configuration directories for debugging. The exact location depends on where you installed the agent.

By default, OpenAM policy agents send log messages remotely to OpenAM when you log auditing information about URL access attempts. To configure audit logging for a centrally managed policy agent, login to the OpenAM console as administrator, and browse to Realms > Realm Name > Agents > Agent Type > Agent Name > Global, and then scroll down to the Audit section.
