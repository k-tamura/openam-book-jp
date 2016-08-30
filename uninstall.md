### OpenAMのアンインストール

この章では、OpenAMのコアソフトウェアとポリシーエージェントをアンインストールする方法を示します。

#### OpenAMのアンインストール

手順. OpenAMをアンインストールする

OpenAMのコアサービスをデプロイし、設定した後、OpenAMのファイルがシステム上に4か所に保持されていているかもしれません。

以下の手順で、OpenAMおよび内部設定ストアを削除します。外部の設定ストアを使用した場合は、すべてのソフトウェアを削除した後でOpenAM設定データを削除することができます。

1. OpenAMをデプロイしているWebアプリケーションコンテナをシャットダウンします。

    ```bash
    $ /etc/init.d/tomcat stop
    Password:
    Using CATALINA_BASE:   /path/to/tomcat
    Using CATALINA_HOME:   /path/to/tomcat
    Using CATALINA_TMPDIR: /path/to/tomcat/temp
    Using JRE_HOME:        /path/to/jdk/jre
    Using CLASSPATH:       /path/to/tomcat/bin/bootstrap.jar:
     /path/to/tomcat/bin/tomcat-juli.jar
    ```
    
2. Webアプリケーションコンテナを実行しているユーザーの$HOMEディレクトリにある設定ファイルを削除することで、OpenAMの設定を解除します。

    OpenAMのコアサービスと設定ファイルの完全なアンインストールは、以下のディレクトリを削除で成り立っています:
    - The configuration directory, by default $HOME/openam. If you did not use the default configuration location, check the value of the Base installation directory property under Deployment > Servers > Server Name > General > System.
    - The hidden directory that holds a file pointing to the configuration directory. For example, if you are using Apache Tomcat as the web container, this file could be $HOME/.openamcfg/AMConfig_path_to_tomcat_webapps_openam_ OR $HOME/.openssocfg/AMConfig_path_to_tomcat_webapps_openam_. 

    ```bash
    $ rm -rf $HOME/openam $HOME/.openamcfg
    ```
    
    または:

    ```bash
    $ rm -rf $HOME/openam $HOME/.openssocfg
    ```
    
    If you used an external configuration store, you must remove the configuration manually from your external directory server. The default base DN for the OpenAM configuration is dc=openam,dc=forgerock,dc=org.
    > **Note**  
    > At this point, you can restart the web container and configure OpenAM anew if you only want to start over with a clean configuration rather than removing OpenAM completely.

3. Undeploy the OpenAM web application.  
    For example, if you are using Apache Tomcat as the web container, remove the .war file and expanded web application from the container.

    ```bash
    $ cd /path/to/tomcat/webapps/
    $ rm -rf openam.war openam/
    ```

#### ポリシーエージェントのアンインストール

