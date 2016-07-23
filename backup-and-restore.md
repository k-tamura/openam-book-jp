## OpenAMの設定のバックアップとリストア

OpenAMは、LDAPディレクトリサーバーとファイルに設定データを保存します。ディレクトリサービスは、ディレクトリサーバー間で設定データをレプリケーションすることで、OpenAMがサイト内のサーバー間で設定データを共有することを可能にします。通常の本番環境での作業中に、OpenAMのサービス設定の複数の現在のコピーを維持するために、ディレクトリサーバーのレプリケーションを利用します。サーバーの損失から、または重大な管理上のエラーから回復するために、ディレクトリデータと設定ファイルをバックアップします。

この章では、ローカルの設定ファイルとローカルの(組み込み)設定ディレクトリサーバーのデータをバックアップとリストアすることによって、OpenAMの設定データをバックアップとリストアする方法を示しています。外部設定ディレクトリサーバーを使用する場合、外部ディレクトリサービスに保存されている設定データをバックアップ、リストアするには、外部ディレクトリサーバーのマニュアルを参照して下さい。

OpenDJをディレクトリサーバーとして使用する場合は、OpenDJの管理ガイドのデータのバックアップとリストアの章でより多くの情報を見つけることができます。

設定ディレクトリのデータがレプリケーションされているOpenAMの配備では、以下の点を考慮する必要があります:

- ディレクトリレプリケーションは、レプリケーションされたデータはどこでも同じであることを保証するために新しい変更を機械的に適用します。古いバックアップデータをリストアすると、ディレクトリレプリケーションは古いデータに新しい変更を適用します。  
これには、管理者の作業ミスによる新しい変更も含まれています。管理者のエラーからリストアするには、エラーを修理するようにレプリケーションされる変更を行うか、エラーの以前の状態にすべてのレプリカをリストアするかのいずれかにより、この現象を回避しなければならりません。

- ディレクトリサーバーのバックアップを準備し、操作をリストアするときは、データレプリケーションのパージ(削除)操作がバックアップするすべてのデータの有効なライフタイムに影響を与えることを認識して下さい。レプリケーションは、発生する競合を解決するために履歴データに基づいています。ディレクトリサーバーが最終的にこの履歴データを削除しなかった場合、すべての利用可能なスペースを埋めまでデータが増大する可能性があります。 ディレクトリサーバーは、したがって、古い履歴データを削除します。 OpenDJは、デフォルトで3日以上経過した履歴データを削除します。  
ディレクトリサーバーは、履歴データのギャップを検出すると、レプリケーション操作を正常に完了できない可能性があります。したがって、バックアップからリストアした全てのデータがレプリケーションパージ遅延よりも古くないことを確認する必要があります。そうしないと、リストア操作が、その間に発生したすべての変更を失い、バックアップから全てのサーバーをリストアする必要があるような、レプリケーションを停止を招く可能性があります。

この章は、バックアップデータの以下の用途をカバーすることを目指しています。

1. サーバー障害からのリカバリ:

    - 全てのサーバー設定データをバックアップする
    - 全てのサーバー設定データは復元する

2. 重大なの管理者エラーからのリカバリ:

    - 設定データのみエクスポートする
    - 重大なエラーの後に設定データを復元する

#### 手順. すべてのサーバー設定データをバックアップする

この手順は、サーバーに保存されている全ての設定データをバックアップします。このバックアップは、障害が発生したサーバーを再構築するときにリストアされます。

次の文が該当する場合、この手順を使用します:

- OpenAM stores configuration data in the embedded OpenDJ directory server.
  The embedded OpenDJ directory server files are co-located with other OpenAM configuration files.

- If OpenAM stores CTS data in the embedded OpenDJ directory server, then the restore operation overwrites current CTS data with older data. After you restore from backup, users authenticate again as necessary.

  If your deployment has long-lived sessions and you do not currently use session failover, you can limit the extent of reauthentication by implementing session failover. For details, see Setting Up OpenAM Session Failover.

If your deployment uses an external configuration directory server, then refer to the documentation for your external directory server or work with your directory server administrator to back up and restore configuration data stored in the external directory service. For OpenDJ directory server you can find more information in the chapter on Backing Up and Restoring Data.

> **重要**  
> Understand the explanation in the introductory paragraphs of this chapter before you proceed. Directory backup validity depends on replication purge delay, which by default for OpenDJ directory server is three days.

Also, do not restore configuration data from a backup of a different release of OpenAM. The structure of configuration data can change from release to release.

Follow these steps for each OpenAM server that you want to back up:

1. Stop OpenAM or the container in which it runs.
2. Back up OpenAM configuration files including those of the configuration directory server but skipping log and lock files.  
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

3. Start OpenAM or the container in which it runs.

#### 手順. 全てのサーバー設定データは復元する

This procedure restores all the configuration data for a server, where the configuration data has been backed up as described in To Back Up All Server Configuration Data . This procedure applies when rebuilding a failed server.

Use this procedure when the following statements are true:

- OpenAM stores configuration data in the embedded OpenDJ directory server.  
  The embedded OpenDJ directory server files are co-located with other OpenAM configuration files.

- If OpenAM stores CTS data in the embedded OpenDJ directory server, then the restore operation overwrites current CTS data with older data. After you restore from backup, users authenticate again as necessary.  
  If your deployment has long-lived sessions and you do not currently use session failover, you can limit the extent of reauthentication by implementing session failover. For details, see Setting Up OpenAM Session Failover.

If your deployment uses an external configuration directory server, then refer to the documentation for your external directory server or work with your directory server administrator to back up and restore configuration data stored in the external directory service. For OpenDJ directory server you can find more information in the chapter on Backing Up and Restoring Data.

> **重要**  
> Understand the explanation in the introductory paragraphs of this chapter before you proceed. Directory backup validity depends on replication purge delay, which by default for OpenDJ directory server is three days.  
Also do not restore configuration data from a backup of a different release of OpenAM. The structure of configuration data can change from release to release.

Follow these steps for each OpenAM server to restore. If you are restoring OpenAM after a failure, make sure you make a copy of any configuration and log files that you need to investigate the problem before restoring OpenAM from backup:

1. Stop OpenAM or the container in which it runs.
2. Restore files in the configuration directory as necessary, making sure that you restore from a valid backup, one that is newer than the replication purge delay:

   ```
   $ cd $HOME
   $ unzip /path/to/backup-2014-12-01-12-00.zip
   Archive:  /path/to/backup-2014-12-01-12-00.zip
   replace openam/.configParam? [y]es, [n]o, [A]ll, [N]one, [r]ename: 
   A
   ...
   ```
3. Start OpenAM or the container in which it runs.

#### 手順. 全てデータのみエクスポートする

LDAP Data Interchange Format (LDIF) is a standard, text-based format for storing LDAP directory data. You can use LDIF excerpts to make changes to directory data.

This procedure takes an LDIF backup of OpenAM configuration data only. Use this LDIF data when recovering from a serious configuration error:

1. Make sure that OpenAM's configuration is in correct working order before exporting configuration data.
2. Use the OpenDJ export-ldif command to run a task that exports only configuration data, not CTS data.  
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

#### 手順. 重大なエラーの後に設定データを復元する

A serious configuration error is an error that you cannot easily repair by using OpenAM configuration tools, such as OpenAM console or the ssoadm command.

Use this procedure to recover from a serious configuration error by manually restoring configuration data to an earlier state. This procedure depends on LDIF data that you exported as described in To Export Only Configuration Data .

1. Read the OpenDJ change log to determine the LDAP changes that caused the configuration problem.  
   The OpenDJ change log provides an external change log mechanism that allows you to read changes made to directory data for replicated directory servers.  
   For instructions on reading the change log, see the OpenDJ Administration Guide section on Change Notification For Your Applications .

2. Based on the data in the change log, determine what changes would reverse the configuration error.  
   For changes that resulted in one attribute value being replaced by another, you can recover the information from the change log alone.

3. For deleted content not contained in the change log, use the LDIF resulting from To Export Only Configuration Data to determine a prior, working state of the configuration entry before the configuration error.

4. Prepare LDIF to modify configuration data in a way that repairs the error by restoring the state of directory entries before the administrative error.

5. Use the OpenDJ ldapmodify command to apply the modification.

6. For instructions on making changes to directory data see the section on Updating the Directory in the OpenDJ Directory Server Developer's Guide.
