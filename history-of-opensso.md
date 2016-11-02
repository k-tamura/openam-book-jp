### OpenSSO以前

　2000年初め、Sun Microsystems社(以下Sun)は、Webシングルサインオン(SSO)の問題を解決するため、ディレクトリサーバーのアクセス管理エディションを開発するプロジェクトを開始しました。当初は、独自のプロトコルを使用し、SSOの一部として認証と認可を提供することだけが目的でしたが、年月が経過し、企業のリソースを保護するためのセキュリティ機能を包括したJavaアプリケーションへと進化しました。そして、多くの機能を実装した後、OpenSSOという製品名のソフトウェアに生まれ変わります。  

　当時、Sunはソフトウェア事業戦略の一環として、商用エンタープライズソフトウェアのほとんどをオープンソース化していました。OpenSSOも同様に、Sunのアクセス管理製品(Sun Java System Access Manager、Sun Java System Federation Manger、Sun Java System SAML v2プラグイン)を統合し、アクセス、フェデレーション、Webサービスの機能を合併することを目標とするソフトウェアとして、オープンソース化されます。そして、SunがOpenSSO Enterprise 8.0をリリースしたときに、この目標は達成されました。

　オープンソースイニシアチブの活動の一環として、製品の展望を変更するような多くのコードのリファクタリングと機能の整備の必要性が生じました。これにより、ビルドやデプロイに必要とされていたすべてのネイティブの依存関係が削除されました。OpenSSOは、任意のJavaサーブレットコンテナ上にWebアーカイブをデプロイすることで起動できる、純粋なJavaの Webアプリケーションとなりす。そして、オープンソースコミュニティによる品質に貢献を期待して、ビルドとチェックインのプロセスは簡略化されました。

　OpenSSOは、ナイトリー、エクスプレス、エンタープライズのいずれかビルドを選択することで、顧客の環境で新機能やバグ修正が簡単に実装できる、非常に柔軟なリリースモデルを持っていました。OpenSSO Enterprise 8.0は、オープンソースのブランチから構築されたSunによるメジャーリリースでした。このリリースの後、2つの異なるエクスプレスリリースがありました。これらにはセキュアトークンサービス(STS)とOAuthの機能が導入され、非常に豊富な機能を備えていました。エクスプレスビルド9は、Oracleからバイナリ形式でリリースされませんでしたが、ソースコードはオープンソースコミュニティに利用可能とされていました。

　2010年の初めに起きたOracle社によるSunの買収に伴い、OpenSSOのリリースおよびサポートのモデルは変更されました。Oracleは、自社の既存製品を優先し、OpenSSOなどのいくつかのSunのソフトウェアを非戦略的な位置づけとして、重点的な投資の対象外としました。OpenSSOの企業の既存顧客に対するサポートを継続しましたが、OpenSSOコミュニティの存続に関して不安の声があがりました。

表. OpenSSOのタイムライン  

<table>
  <tbody>
    <tr>
      <td>2001年</td>
      <td>Sun Microsystems社(以下Sun)が、iPlanet Directory Serverのアクセス管理エディションをリリース。</td>
    </tr>
    <tr>
      <td>2003年</td>
      <td>Sunが、iPlanet Directory Serverアクセス管理エディションの製品名をSun ONE Identity Serverを変更。</td>
    </tr>
    <tr>
      <td></td>
      <td>SunがWavesetを買収。</td>
    </tr>
    <tr>
      <td>2004年</td>
      <td>Sunが、Sun Java Enterprise Systemをリリース。Waveset Lighthouseは、Sun Java System Identity Managerに、Sun ONE Identity ServerはSun Java System Access Managerに製品名を変更される。両製品は、Sun Java Enterprise Systemの構成要素として含まれる。</td>
    </tr>
    <tr>
      <td>2005年</td>
      <td>Sunは、Sun Java System Access ManagerをベースにしたオープンソースプロジェクトOpenSSOを発表。</td>
    </tr>
    <tr>
      <td>2008年</td>
      <td>Sunは、OpenSSOビルド6コミュニティオープンソース版、OpenSSO Enterprise 8.0商用エンタープライズ版をリリース。</td>
    </tr>
    <tr>
      <td>2009年</td>
      <td>Sunは、OpenSSOビルド7および8リリース。</td>
    </tr>
    <tr>
      <td>2010年</td>
      <td>Oracle社(以下Oracle)がSunを買収。Oracleは、今後のOpenSSOの製品サポートを計画せず、開発は中断される。</td>
    </tr>
  </tbody>
</table>

