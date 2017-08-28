# Apache
Apacheは世界中で使われているソフトウェアである。Apacheの主な機能として、以下が挙げられる。  

こうした機能を有するソフトウェアを**Webサーバー**と呼ぶ。  
WebサーバーはOS上でデーモンプロセスとして動作し、HTTPリクエストに対して何かしらのレスポンスを返す。  
主に使用されるWebサーバーは以下の通りである。

- Apache HTTP Server
- Nginx
- Internet Information Service(IIS)

ここではLinuxへApache HTTP Serverをインストールした環境があることを前提とする。  
(Linuxの項を参照)

## デーモンプロセス
全てのアプリケーションは、プロセスというプログラムの実行単位としてOSから認識され、管理される。  
Powerpoint等の一般的なアプリケーションは、開始時にプロセスが起動し、終了時にプロセスが閉じられる。  

一方でWebサーバーはPowerpointとは異なり、常にプロセスが起動し、終了しないことが前提となる。  
Webサーバーは、動作しているサーバー上へのPORT80へのリクエスト(HTTPリクエスト)を検知し、レスポンスを返す必要がある。  
リクエストが来た時にプロセスを起動する場合、リクエストの数が増えた際に時間がかかりすぎてしまう。  
そのため、Webサーバーは常にプロセスを起動し、リクエストが来た時に処理をする、というアプローチを取っている。

Webサーバーのように、起動したら終了しない前提であるプロセスのことを**デーモンプロセス**と呼ぶ。  

## Linuxのディレクトリ構成とApacheのファイル
Linuxのフォルダ構成は以下の通りになっている。

```
/〈ルート〉
/bin
/boot
/dev
/etc
/home
/lib
/lost+found
/media
/misc
/mnt
/opt
/tmp
/usr
/var
```

それぞれのフォルダと、格納されるファイルについて記載する。  

### bin
cpやchmodなどのシステムを管理する上で基本コマンドが入っている
基本コマンド限定で管理するのが普通で、ソフトのインストールに付随するコマンドなどはここにインストールしない。
cp、chmodなど

### boot
ブートに必要なファイル。ブートログやカーネルイメージ（Linuxカーネルを格納して圧縮したファイル）が保存されている。
vmlinuzなど

### dev
デバイスファイルが配置されている。デバイス（装置〈ハードウエア〉）をファイルとして扱うUNIXの設計思想から来ている。

### etc
様々なソフトウェアの設定ファイルが置かれる。  
**httpd.conf**は多くの場合、ここの下の階層に置かれる。  

### home
一般ユーザーのホームディレクトリ

### lib
/binや/sbinのコマンドを実行するのに必要なファイルが配置されている。基本的にユーザーが変更を加えることはない。a

### lost+found
システムのバックアップや復元用のファイルが格納される。

### media
リムーバブルメディアのマウントポイント。

### misc
リムーバブルメディアのマウントポイント。自動でメディアをマウントするデーモンautofsで利用する。

### mnt
一時的なマウントポイント。mediaとの違いは、一時的なファイルシステム（fstabなど）のマウントに使う点。

### tmp
テンポラリデータの保存場所。メモリ上の一時ファイルを保存する。  
再起動時に消去される。また通常はcronで定期的に消去される。

### usr
各ユーザーが共通して利用するプログラム・ライブラリのデータ。ソースからコンパイルしたソフトなどがインストールされる。  
etcに保存されている設定ファイルのシンボルリンクもこちらに貼られることが多い。

### var
ログやキャッシュなど、可変的システムデータ（動的ファイル）。
**access-log**はここに下の階層に置かれる

Apacheで良く参照するファイルは、以下の2種が挙げられる。  

- httpd.conf  
  apacheの設定ファイル。ルートディレクトリのパスや、使用するモジュール等を設定する。  
  などhttp[d]のdはデーモン(daemon)のdを指している。  

- access-log  
  Webサイトへのアクセスログ。このログを解析することで、様々な情報を取得することができる。  

### Exercise
- httpd.confを参照してください。
- HTTPリクエストを送った後に、その情報がどのようにログ出力されているのかについて、access-logを確認してください。

## Virtualhostの設定

![img](http://www.pimschaaf.nl/wp-content/uploads/2016/07/Virtual-Host-Multiple-Websites-Ubuntu-13.10-and-Rackspace-Cloud-Server.png)  
出典元: http://www.pimschaaf.nl/xampp-with-virtual-hosts-configuration/

Apacheを使用してWebサイトを公開する際には、Virtualhostという単位でこれを行なう。  
Virtualhostとは、**単一のApacheで、別々のドメインである別々のWebサイトを公開する**ためにApacheが持つ機能である。  
Apache上で１つのWebサイトのみを公開する場合でもVirtualhostを使うことが多く、デファクト・スタンダードとなっている。

Virtualhostの設定は`httpd.conf`か、`extra`フォルダ上の`httpd-vhosts.conf`上で行なう。  
`httpd.conf`は`Include`句を用いることで、他のconfファイルを追加で読み込むことができる。  
`httpd.conf`はVirtualhost以外にも多数の設定項目を持つため、Virtualhostの設定は、別のconfファイルに外だしすることが望ましい。(というより、ちゃんとしたインフラエンジニアがいた場合、ここで横着すると注意されます)

Virutalhostは、以下のような構文を持つ。
```
<VirtualHost *:8080>
  DocumentRoot myblog.com
  ServerName myblog.com
  ErrorLog logs/host.example.com-error_log
  TransferLog logs/host.example.com-access_log
  <directory />
    options followsymlinks
    allowoverride all
    require all granted
  </directory>
</VirtualHost>
```
以下、Virtualhostを用いてWebサイトを公開する際に必要な項目の一部を記載する。  
なお、詳細については以下のリンクを参照。  
https://httpd.apache.org/docs/2.4/ja/mod/core.html

### Listenディレクティブ
HTTP通信はtcp/ipのポート80を使用する。一般に、通信が来たタイミングで処理するためにあるプロセスが特定のサーバーの特定のポートを監視することを、プロセスがポートXXを`Listen`している、と呼ぶ。  
Apacheはデフォルトでポート80をListenしている。仮想マシンをローカルで複数動かしている場合に多いケースが、ポート80の通信がホストOSのApacheやIISで使用されており、ゲストOSへ転送できないというものである。  
Apacheで80番以外のポートを使ってHTTP通信を行う際は`Listen`ディレクティブを使用する。  

例: `Listen 8080`

### Virtualhostディレクティブ
Virtualhostは、以下の構文を持つ。

`<VirtualHost addr[:port] [addr[:port]] ...> ... </VirtualHost>`

上記のサンプルでは`<VirtualHost *:8080>`と記載した。この`*`はApacheが動作するサーバーが持つIPアドレスすべて、`8080`はポート8080番で、Webサイトを公開するという意味を持つ。  

### Documentrootディレクティブ
以下のHTML,CSS,Javascriptを使用する、簡易的なブログサイトを作成するとする。  

- ドメイン  
  myblog.com

- url
  - 記事一覧: myblog.com/article/index
  - 記事を見る: myblog.com/article/view/<日付>.html

記事を投稿するときは、まず記事のhtmlを追加し、その後記事一覧のhtmlへ、追加したhtmlへのリンクを書くとする。  
Apacheのデフォルト設定では、URLの階層構造は、そのままディレクトリの階層構造とリンクする。  
上記の例では、

```
<ルートディレクトリ>/
 |- welcome.html
 |- article/
    |- index/
       |- index.html
    |- view/
       |- <日付>.html
```
というディレクトリ構成になる。
Apacheの**DocumentRootディレクティブ**は、あるサイトのURLの最上位階層と、サーバー内のディレクトリ階層を紐付ける機能を有する。  
例えば、上記のルートディレクトリを`/var/www/html/myblog/`というディレクトリにした場合

- `http://myblog.com/welcome.html` => `/var/www/html/myblog/welcome.html`
- `http://myblog.com/article/view/20170827.html` => `/var/www/html/myblog/article/view/20170827.html`

という紐付が行われる。

### ServerNameディレクティブ
Virtualhostを使用することで、複数のWebサイトを単一のApacheで公開することができる。  
例えば`http://myblog.com`というURLが存在した場合  

- `http://` => ポートの識別
- `myblog.com` => サーバーの識別

となるため、**どのサーバーのどのポートか**という点については特定されている。  
ここに、**どのサイトか**を識別するために使用するのが**ServerNameディレクティブ**である。  

サンプルでは`ServerName myblog.com`と記載した。これは、**自サーバーへのポート8080のアクセスのうち、myblog.comというドメインでアクセスしているものについてのみ、対応する**という意味である。  
単一のIPアドレスに対して複数のドメインを紐付けることができる。単一のApacheで複数のWebサイトを公開する場合には、以下が必要となる。

- ドメインAとドメインBを、サーバーXへ紐付ける(DNSレコードの登録)
- ドメインAとドメインB毎にVirtualServerを宣言し、ServerNameディレクティブでドメインを指定する

サンプルでは、IPアドレス直打ちによるアクセスをしても、`myblog.com`は参照されないように設定されている。

### Directoryディレクティブ
Work in progress

### Exercise
- 任意のディレクトリをルートディレクトリとし、Webサイトを作成してください。
  - ヒント: http://qiita.com/zaburo/items/b9c3c8c541ffd16797fc
- Apacheは`mod_rewrite`というモジュールを利用することで、URLとサーバー内のディレクトリ階層の紐付けを柔軟に変更することができる。
  - `mod_rewrite`を使用し、`.html`という拡張子をurl内で指定しなくても、当該htmlへアクセスするように設定してください。
  - ヒント: http://combitaro.net/article/136

