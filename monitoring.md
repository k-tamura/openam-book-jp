## Monitoring OpenAM Services

This chapter covers how to monitor OpenAM services to ensure appropriate performance and service availability.

### Monitoring Interfaces

OpenAM lets you monitor OpenAM over protocol through web pages, Java Management Extensions (JMX), or Simple Network Management Protocol (SNMP). The services are based on JMX.

To configure monitoring services, login to OpenAM console as OpenAM administrator, and browse to Configuration > System > Monitoring. Alternatively, you can use the ssoadm set-attr-defs command:

```
$ ssoadm \
 set-attr-defs \
 --servicename iPlanetAMMonitoringService \
 --schematype Global \
 --adminid amadmin \
 --password-file /tmp/pwd.txt \
 --attributevalues iplanet-am-monitoring-enabled=true
```

Restart OpenAM for the changes to take effect. You must also restart OpenAM if you disable monitoring.

#### Web-Based Monitoring

You can configure OpenAM to allow you to access a web based view of OpenAM MBeans on port 8082 where the core server runs, such as http://openam-ter.example.com:8082/. Either use the console, or use the ssoadm command:

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

Or:

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
