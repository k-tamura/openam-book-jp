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
    - 設定ディレクトリ(デフォルトでは$HOME/openam)。デフォルト設定の場所を使用しなかった場合は、Deployment > Servers > Server Name > General > System 以下のベースインストールディレクトリの値をチェックします。
    - 設定ディレクトリを指すファイルを保持している隠しディレクトリ。WebコンテナとしてApache Tomcatを使用している場合、このファイルは例えば、$HOME/.openamcfg/AMConfig_path_to_tomcat_webapps_openam_や$HOME/.openssocfg/AMConfig_path_to_tomcat_webapps_openam_である可能性があります。

    ```bash
    $ rm -rf $HOME/openam $HOME/.openamcfg
    ```
    
    または:

    ```bash
    $ rm -rf $HOME/openam $HOME/.openssocfg
    ```
    
    外部設定ストアを使用した場合は、外部ディレクトリサーバーから手動で設定を削除する必要があります。 OpenAMの設定のベースDNは、デフォルトでdc=openam,dc=forgerock,dc=orgです。
    > **注意**  
    > OpenAMを完全に削除するのではなく、クリーンな設定で最初からやり直したい場合は、この時点でWebコンテナを再起動して、新たにOpenAMを設定することができます。

3. OpenAM Webアプリケーションをアンデプロイします。
    WebコンテナとしてApache Tomcatを使用している場合、コンテナから.warファイルと展開したWebアプリケーションを削除します。

    ```bash
    $ cd /path/to/tomcat/webapps/
    $ rm -rf openam.war openam/
    ```

#### ポリシーエージェントのアンインストール

