## 前提条件と事前準備

OpenAMのインストールは、基本的にTomcatなどのサーブレットコンテナにwarファイルをデプロイして初期設定するだけですが、いくつかの注意すべき点があります。

### 前提条件

OpenAMをインストールする前に、インストールできる環境であるか確認が必要です。OpenAMのリリースノートの「Before You Install OpenAM Software」の章に、OpenAMを稼働する上で必要なOSやサーブレットコンテナについて記載されています。まずはこの条件を確認して下さい。

> **情報**  
> 前提条件について記載されているのは、インストールガイドではなくリリースノートです。
> この中にForgeRock社で動作確認がされているものが記載されていますが、これはForgeRock社で動作検証をしたものであり、それ以外の環境でも正常に動作する可能性はあります。

### インストール前の事前設定

#### FQDNの準備

OpenAMをセットアップする際に、完全修飾ドメイン名（FQDN）を指定する必要があります。セットアップの前に、openam.example.comのようなFQDNでサーバーにアクセスできることを確認して下さい。評価目的でOpenAMを使用する場合は、/etc/hostsファイル(Linux)や%SystemRoot%\system32\drivers\etc\hostsファイル(Windows)に、FQDNが適切に割り当てられていることと、/etc/sysconfig/networkのHOSTNAMEもFQDNになっているを確認して下さい。

```
$ vi /etc/hosts
 
# 以下を追加
192.168.1.1 openam.example.com
```
 
```
$ vi /etc/sysconfig/network
HOSTNAME=openam.example.com
```

> **注意**  
> テスト目的のためであっても、localhostドメインを使用しないで下さい。 OpenAMの動作は、ドメイン名に基づいて返されるブラウザのクッキーに依存しています。基本的には、少なくとも2つの「.」（ドット）を含むドメイン名を使用していることを確認して下さい。
> 例) openam.example.com

> **情報**  
> 一般的に、従来からあるCookieを使用したSSOでは、SSOの適用範囲をCookieが共有可能な同一ドメイン内に限定します。そのため、上記のようなドメインの設定に関する制限事項を守る必要があります。

#### ファイルディスクリプタの最大値の設定

組み込みのOpenDJを使用する場合は、必ずファイルディスクリプタが十分であることを確認して下さい。Linuxでは多くの場合、ユーザーあたり1024に制限されていますが、OpenDJにはこの値は低すぎます。以下のコマンドで確認します。

```	
$ ulimit -n
1024
```

> **情報**  
> CentOSの場合、CentOS 5まではデフォルトで無制限だったようですが、CentOS 6からは1024になっています。

OpenDJには、少なくとも64Kのファイルディスクリプタを使用できるようにしておいた方がいいです。組み込みOpenDJは、OpenAMプロセス空間内で実行されるため、OpenAMの実行ユーザーに対する設定値を変更します。ユーザー制限を設定するために/etc/security/limits.confが使用されている場合は、次の行を追加することで、ソフト制限とハード制限を設定できます。

```
openam soft nofile 65536
openam hard nofile 131072
```

Linuxシステム全体の最大値を確認することができます。

```
$ cat /proc/sys/fs/file-max
204252
```

全体的な最大値が低すぎる場合は、次のようにして値を増やして下さい。スーパーユーザーとして、/etc/sysctl.confを編集し、カーネルパラメータfs.file-maxを高い最大値に設定します。/etc/sysctl.confの設定をリロードするには次のコマンドを実行します。

```
sysctl -p
```

#### ファイアウォールとSELinuxの設定

テスト目的でOpenAMをLinux上にインストールするのであれば、ファイアウォールとSELinuxをオフにしておいた方が簡単です。以下を設定した後、設定を反映させるためにOSを再起動します。

```
$ service iptables stop
$ chkconfig iptables off
 
$ vi /etc/sysconfig/selinux
#SELINUX=enforcing
SELINUX=disabled
```

> **情報**  
> OpenAMが使用するポートについては、OpenAMのリファレンス以下に記載されています。
