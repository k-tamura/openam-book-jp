## OpenAMの設定のバックアップとリストア

OpenAMはLDAPディレクトリサーバーとファイルに設定データを保存します。ディレクトリサービスは、ディレクトリサーバー間で設定データをレプリケーションすることで、OpenAMがサイト内のサーバー間で設定データを共有することを可能にします。通常の本番環境での作業中に、OpenAMのサービス設定の複数の現在のコピーを維持するために、ディレクトリサーバーのレプリケーションを利用します。サーバーの損失から、または重大な管理上のエラーから回復するために、ディレクトリデータと設定ファイルをバックアップします。

この章では、ローカルの設定ファイルとローカルの(組み込み)設定ディレクトリサーバーのデータをバックアップとリストアすることによって、OpenAMの設定データをバックアップとリストアする方法を示しています。外部設定ディレクトリサーバーを使用する場合、外部ディレクトリサービスに保存されている設定データをバックアップ、リストアするには、外部ディレクトリサーバーのマニュアルを参照して下さい。

OpenDJをディレクトリサーバーとして使用する場合は、OpenDJの管理ガイドのデータのバックアップとリストアの章でより多くの情報を見つけることができます。

設定ディレクトリデータがレプリケーションされているOpenAMの配備では、以下の点を考慮する必要があります:

- Directory replication mechanically applies new changes to ensure that replicated data is the same everywhere. When you restore older backup data, directory replication applies newer changes to the older data.  
This includes new changes that the administrator sees as mistakes. To recover from administrative error, you must work around this behavior either by performing a change to be replicated that repairs the error or by restoring all replicas to a state prior to the error.

- When preparing directory server backup and restore operations, also know that data replication purge operations affect the useful lifetime of any data that you back up.  
Replication relies on historical data to resolve any conflicts that arise. If directory servers did not eventually purge this historical data, the data would continue to grow until it filled all available space. Directory servers therefore purge older historical data. OpenDJ purges historical data older than 3 days by default.  
When the directory server encounters a gap in historical data it cannot correctly complete replication operations. You must make sure, therefore, that any data you restore from backup is not older than the replication purge delay. Otherwise your restoration operation could break replication with the likely result that you must restore all servers from backup, losing any changes that occurred in the meantime.

This chapter aims to cover the following uses of backup data.

1. Recovery from server failure:

    - To Back Up All Server Configuration Data
    - To Restore All Server Configuration Data

2. Recovery from serious administrative error:

    - To Export Only Configuration Data
    - To Restore Configuration Data After Serious Error

#### 手順. To Back Up All Server Configuration Data

This procedure backs up all the configuration data stored with the server. This backup is to be restored when rebuilding a failed server.

Use this procedure when the following statements are true:

    OpenAM stores configuration data in the embedded OpenDJ directory server.

    The embedded OpenDJ directory server files are co-located with other OpenAM configuration files.

    If OpenAM stores CTS data in the embedded OpenDJ directory server, then the restore operation overwrites current CTS data with older data. After you restore from backup, users authenticate again as necessary.

    If your deployment has long-lived sessions and you do not currently use session failover, you can limit the extent of reauthentication by implementing session failover. For details, see Setting Up OpenAM Session Failover.

If your deployment uses an external configuration directory server, then refer to the documentation for your external directory server or work with your directory server administrator to back up and restore configuration data stored in the external directory service. For OpenDJ directory server you can find more information in the chapter on Backing Up and Restoring Data .

> **重要**  
> Understand the explanation in the introductory paragraphs of this chapter before you proceed. Directory backup validity depends on replication purge delay, which by default for OpenDJ directory server is three days.

Also, do not restore configuration data from a backup of a different release of OpenAM. The structure of configuration data can change from release to release.

Follow these steps for each OpenAM server that you want to back up:

    Stop OpenAM or the container in which it runs.

    Back up OpenAM configuration files including those of the configuration directory server but skipping log and lock files.

    The following example uses the default configuration location. $HOME is the home directory of the user who runs the web container where OpenAM is deployed, and OpenAM is deployed in Apache Tomcat under openam:

    ```
    $ cd $HOME
    $ zip \
     --recurse-paths \
     /path/to/backup-`date -u +i5%F-%m-%S`.zip \
     openam .openamcfg/AMConfig_path_to_tomcat_webapps_openam_ \
     --exclude openam/openam/debug/* openam/openam/log/* openam/openam/stats* \
      openam/opends/logs/* openam/opends/locks/*
    ...
    $ ls /path/to/backup-2014-12-01-12-00.zip
    /path/to/backup-2014-12-01-12-00.zip
       
    ```

    The backup is valid until the end of the purge delay.

    Start OpenAM or the container in which it runs.

#### 手順. To Restore All Server Configuration Data

This procedure restores all the configuration data for a server, where the configuration data has been backed up as described in To Back Up All Server Configuration Data . This procedure applies when rebuilding a failed server.

Use this procedure when the following statements are true:

    OpenAM stores configuration data in the embedded OpenDJ directory server.

    The embedded OpenDJ directory server files are co-located with other OpenAM configuration files.

    If OpenAM stores CTS data in the embedded OpenDJ directory server, then the restore operation overwrites current CTS data with older data. After you restore from backup, users authenticate again as necessary.

    If your deployment has long-lived sessions and you do not currently use session failover, you can limit the extent of reauthentication by implementing session failover. For details, see Setting Up OpenAM Session Failover.

If your deployment uses an external configuration directory server, then refer to the documentation for your external directory server or work with your directory server administrator to back up and restore configuration data stored in the external directory service. For OpenDJ directory server you can find more information in the chapter on Backing Up and Restoring Data .
Important

Understand the explanation in the introductory paragraphs of this chapter before you proceed. Directory backup validity depends on replication purge delay, which by default for OpenDJ directory server is three days.

Also do not restore configuration data from a backup of a different release of OpenAM. The structure of configuration data can change from release to release.

Follow these steps for each OpenAM server to restore. If you are restoring OpenAM after a failure, make sure you make a copy of any configuration and log files that you need to investigate the problem before restoring OpenAM from backup:

    Stop OpenAM or the container in which it runs.

    Restore files in the configuration directory as necessary, making sure that you restore from a valid backup, one that is newer than the replication purge delay:

```
$ cd $HOME
$ unzip /path/to/backup-2014-12-01-12-00.zip
Archive:  /path/to/backup-2014-12-01-12-00.zip
replace openam/.configParam? [y]es, [n]o, [A]ll, [N]one, [r]ename: 
A
...
```

    Start OpenAM or the container in which it runs.

#### 手順. To Export Only Configuration Data

LDAP Data Interchange Format (LDIF) is a standard, text-based format for storing LDAP directory data. You can use LDIF excerpts to make changes to directory data.

This procedure takes an LDIF backup of OpenAM configuration data only. Use this LDIF data when recovering from a serious configuration error:

    Make sure that OpenAM's configuration is in correct working order before exporting configuration data.

    Use the OpenDJ export-ldif command to run a task that exports only configuration data, not CTS data.

    You can run this command without stopping OpenAM.

    Find OpenDJ commands under the file system directory that contains OpenAM configuration files.

    The bind password for Directory Manager is the same as the password for the OpenAM global administrator (amadmin):

```
$ $HOME/openam/opends/bin/export-ldif \
 --port 4444 \
 --hostname openam.example.com \
 --bindDN "cn=Directory Manager" \
 --bindPassword password \
 --backendID userRoot \
 --includeBranch dc=openam,dc=forgerock,dc=org \
 --excludeBranch ou=tokens,dc=openam,dc=forgerock,dc=org \
 --ldifFile /path/to/backup-`date -u +%F-%m-%s`.ldif \
 --start 0 \
 --trustAll
Export task 20141208113331302 scheduled to start Dec 8, 2014 11:33:31 AM CET
```

When the task completes, the LDIF file is at the expected location:

```
$ ls /path/to/*.ldif
/path/to/backup-2014-12-08-12-1418034808.ldif
```

#### 手順. To Restore Configuration Data After Serious Error

A serious configuration error is an error that you cannot easily repair by using OpenAM configuration tools, such as OpenAM console or the ssoadm command.

Use this procedure to recover from a serious configuration error by manually restoring configuration data to an earlier state. This procedure depends on LDIF data that you exported as described in To Export Only Configuration Data .

    Read the OpenDJ change log to determine the LDAP changes that caused the configuration problem.

    The OpenDJ change log provides an external change log mechanism that allows you to read changes made to directory data for replicated directory servers.

    For instructions on reading the change log, see the OpenDJ Administration Guide section on Change Notification For Your Applications .

    Based on the data in the change log, determine what changes would reverse the configuration error.

    For changes that resulted in one attribute value being replaced by another, you can recover the information from the change log alone.

    For deleted content not contained in the change log, use the LDIF resulting from To Export Only Configuration Data to determine a prior, working state of the configuration entry before the configuration error.

    Prepare LDIF to modify configuration data in a way that repairs the error by restoring the state of directory entries before the administrative error.

    Use the OpenDJ ldapmodify command to apply the modification.

    For instructions on making changes to directory data see the section on Updating the Directory in the OpenDJ Directory Server Developer's Guide.
