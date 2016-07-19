## OpenAMの監視

この章では、適切なパフォーマンスとサービスの可用性を確保するために、OpenAのサービスを監視する方法を説明します。

### 監視インターフェイス

OpenAMは、Java Management Extensions(JMX)や簡易ネットワーク管理プロトコル(SNMP)で、ウェブページを通してOpenAMを監視することができます。サービスは、JMXに基づいています。

監視サービスを設定するには、OpenAM管理者としてOpenAMコンソールにログインし、設定 > システム > 監視 を参照します。または、ssoadm set-attr-defsコマンドを使用することができます:

```
$ ssoadm \
 set-attr-defs \
 --servicename iPlanetAMMonitoringService \
 --schematype Global \
 --adminid amadmin \
 --password-file /tmp/pwd.txt \
 --attributevalues iplanet-am-monitoring-enabled=true
```

変更を有効(または無効)にするためには、OpenAMを再起動する必要があります。

#### Webベースの監視

http://openam-ter.example.com:8082/のような、コアサーバが実行されるポート8082上で、OpenAMのMBeanのウェブベースのビューにアクセスできるようにOpenAMを設定することができます。コンソールを使用するか、ssoadmコマンドを使用します。

```
$ ssoadm \
 set-attr-defs \
 --servicename iPlanetAMMonitoringService \
 --schematype Global \
 --adminid amadmin \
 --password-file /tmp/pwd.txt \
 --attributevalues iplanet-am-monitoring-http-enabled=true
```

The default authentication file allows you to authenticate over HTTP as user demo, password changeit. The user name and password are kept in the file specified, with the password encrypted:

```
$ cat openam/openam/openam_mon_auth
demo AQICMBCKlwx6G3vzK3TYYRbtTpNYAagVIPNP
```

または:

```
$ cat openam/openam/opensso_mon_auth
demo AQICvSe+tXEg8TUUT8ekzHb8IRzVSvm1Lc2u
```

You can encrypt a new password using the ampassword command. After changing the authentication file, you must restart OpenAM for the changes to take effect.

図. OpenAM MBeans in a Browser

OpenAM MBeans in a Browser

#### JMX Monitoring

You can configure OpenAM to allow you to listen for Java Management eXtension (JMX) clients, by default on port 9999. Either use the OpenAM console page under Configuration > System > Monitoring and make sure both Monitoring Status and Monitoring RMI interface status are both set to Enabled, or use the ssoadm command:

```
$ ssoadm \
 set-attr-defs \
 --servicename iPlanetAMMonitoringService \
 --schematype Global \
 --adminid amadmin \
 --password-file /tmp/pwd.txt \
 --attributevalues iplanet-am-monitoring-enabled=true \
  iplanet-am-monitoring-rmi-enabled=true
```

A number of tools support JMX, including jvisualvm and jconsole. When you use jconsole to browse OpenAM MBeans for example, the default URL for the OpenAM running on the local system is service:jmx:rmi:///jndi/rmi://localhost:9999/server.

```
$ jconsole service:jmx:rmi:///jndi/rmi://localhost:9999/server &
```

You can also browse the MBeans by connecting to your web application container, and browsing to the OpenAM MBeans. By default, JMX monitoring for your container is likely to be accessible only locally, using the process ID.

図. JConsole Browsing OpenAM MBeans

JConsole Browsing OpenAM MBeans

Also see Monitoring and Management Using JMX for instructions on how to connect remotely, how to use SSL, and so forth.
Important

JMX has a limitation in that some Operations and CTS tables cannot be properly serialized from OpenAM to JMX. As a result, only a portion of OpenAM's monitoring information is available through JMX. SNMP is a preferred monitoring option over JMX and exposes all OpenAM tables, especially for CTS, with no serialization limitations.

#### SNMP Monitoring

You can configure OpenAM to allow you to listen on port 8085 for SNMP monitoring. Either use the console, or use the ssoadm command:

```
$ ssoadm \
 set-attr-defs \
 --servicename iPlanetAMMonitoringService \
 --schematype Global \
 --adminid amadmin \
 --password-file /tmp/pwd.txt \
 --attributevalues iplanet-am-monitoring-snmp-enabled=true
```
