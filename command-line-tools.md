次のOpenAMのコマンドラインツールをインストールすることができます。:

> **情報**  
> 次のリストにあるスクリプトツールは、Microsoft Windows上で使用するための.batのバージョンもあります。

- **agentadmin**  
 このツールを使用すると、OpenAMポリシーエージェントのインストールを管理することができます。
 ポリシーエージェントのインストールの一部として、このツールを解凍します。

- **ampassword**  
 このツールを使用すると、OpenAM管理者のパスワードを変更したり、暗号化されたパスワードの値を表示することができます。
 SSOAdminTools-13.0.0.zipを解凍して、これをインストールして下さい。
 
- **amverifyarchive**  
 このツールは改ざんが無いかログアーカイブをチェックします。
 SSOAdminTools-13.0.0.zipを解凍して、これをインストールして下さい。

- **openam-distribution-configurator-13.0.0.jar**  
The sampleconfiguration file provided with the tool is set up with the format for the config.file, and it must be adapted for your environment.
 この実行可能な.jarファイルは、設定ファイルを使用してOpenAMサーバのサイレントインストールを実行できます。
例えば、`java -jar configurator.jar -f config.file`コマンドは、config.fileとconfigurator.jarのアーカイブを結びつけます。
ツールで提供されたsampleconfigurationファイルは、config.fileのフォーマットで設定されており、それが使用している環境に適合させる必要があります。
SSOConfiguratorTools-13.0.0.zipを解凍して、これをインストールして下さい。
 
- **ssoadm**  
 This tool provides a rich command-line interface for the configuration of OpenAM core services.
 In a test environment, you can activate ssoadm.jsp to access the same functionality in your browser. Once active, you can use many features of the ssoadm command by navigating to the ssoadm.jsp URI, in a URL, such as http://openam.example.com:8080/openam/ssoadm.jsp.
 Install this from SSOAdminTools-13.0.0.zip.
 To translate settings applied in OpenAM console to service attributes for use with ssoadm, log in to the OpenAM console as amadmin and access the services page, in a URL, such as http://openam.example.com:8080/openam/services.jsp.

The commands access the OpenAM configuration over HTTP (or HTTPS). When using the administration commands in a site configuration, the commands access the configuration through the front end load balancer.

Sometimes a command cannot access the load balancer because:

- Network routing restrictions prevent the tool from accessing the load balancer.

- For testing purposes, the load balancer uses a self-signed certificate for HTTPS, and the tool does not have a way of trusting the self-signed certificate.

- The load balancer is temporarily unavailable.

In such cases you can work around the problem by adding an option for each node, such as the following to the java command in the tool's script.

Node 1:

> -D"com.iplanet.am.naming.map.site.to.server=https://lb.example.com:443/openam=
> http://server1.example.com:8080/openam"

Node 2:

> -D"com.iplanet.am.naming.map.site.to.server=https://lb.example.com:443/openam=
> http://server2.example.com:8080/openam"

In the above example the load balancer is on the lb host, https://lb.example.com:443/openam is the site name, and the OpenAM servers in the site are on server1 and server2.

The ssoadm command will only use the latest value in the map, so if you have a mapping like:

> -D"com.iplanet.am.naming.map.site.to.server=https://lb.example.com:443/openam=
> http://server1.example.com:8080/openam, https://lb.example.com:443/openam=
> http://server2.example.com:8080/openam"

The ssoadm command will always talk to:

> http://server2.example.com:8080/openam
