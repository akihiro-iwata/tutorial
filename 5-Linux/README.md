# Linux
LinuxはWindowsと並び、世界中のコンピューター上で動作しているOSである。  

Linuxの特徴としては
- オープンソースソフトウェアである
- 主にサーバーで動作している
- CommandLine Interfaceで操作する
が挙げられる。  

以下、Oracle Virtual Boxを用いてUbuntuサーバーを立て、これを操作することで、Linuxへの理解の導入を行なう。

## 仮想マシンとは
Linuxについて触れる前に、仮想マシンについて理解する必要がある。  

![img](http://www.networld.co.jp/files/3414/3937/0302/sol1_image03.jpg)

PCやスマホなど、日常的に使われるコンピューターは、1台につき1つのOSがインストールされている。  
しかし、ハイパーバイザーと呼ばれるソフトウェアを用いることで、例えばWindowsのPCの中にLinuxのサーバーを立てることができる。  
今日のサーバーは、1台の物理的な筐体の上で1つのサーバーが動作するケースは減少しており、クラウドに代表されるような、ハイパーバイザーを用いたサーバーが動作するケースが増えている。   
このように、ハイパーバイザーを用いてサーバーを動かすことを、サーバーの**仮想化**と呼び、このサーバーを**仮想マシン**と呼ぶ。  

## 仮想マシンでLinuxを立てる
Windows PCで主に使用されるハイパーバイザーは、以下の3種類が挙げられる。

1.  VMWare
2.  Oracle Virtual Box
3.  Hyper-V

それぞれ一長一短があるが、ここでは2のOracle Virtual Boxを使用する。

### Exercise
- Oracle Virtual Boxを用いて、Ubuntuサーバーを立ててください。
  - 参照リンク: http://qiita.com/ykawakami/items/4bae371932110b2e25e3

- LinuxにはUbuntu以外にも様々な種類(ディストリビューション)がある。  
  - Ubuntu以外のディストリビューションを3つ調べてください。  
  - これらのLinuxディストリビューションに共通する要素を挙げてください。

## コマンドラインでLinuxを操作する
LinuxはWindowsと異なり、Grafic User Interface(GUI)ではなく、Command Line Interface(CLI)での操作が中心となる。  
特に多いのが、SSHクライアント(teraterm, PuTTY)を使用してLinuxへ接続し、ファイル操作やコマンド実行を行なうケースである。  

### Exercise
- 以下のサイトを参照し、Linuxのコマンドライン操作を理解してください。
  - http://qiita.com/lrf141/items/4dadd107c1ac778260e5

- `ls`コマンドを使用して、ファイルのパーミッション、グループ名、ユーザー名、ファイルサイズ、更新日時、ファイル名を出力してください。
  - ヒント: `man ls`で`ls`コマンドのオプションが見られます

- `ls`コマンドを使用して出力される結果のうち、`Micky`という文字列をファイル名に含むものだけを抽出してください。
  - ヒント: `|`(パイプ)を使用することで、複数のコマンドを併せて使うことができます。

## LinuxへApacheをインストールする
HTTP(S)を使用したアプリケーションのことをWebアプリケーションと呼ぶ。  
Webアプリケーションを使用する端末(クライアント)には、ブラウザーやネイティブアプリ、クラサバアプリなどある程度の種類が存在するが、  
Webアプリケーションを公開するためにはWebサーバーが必須となる。  

主に使用されるWebサーバーには、以下が挙げられる。

1.  Apache HTTP Server
2.  Nginx
3.  Microsoft Internet Information Service

それぞれ一長一短があるが、ここでは1のApache HTTP Serverを使用する。

### Exercise
- Oracle Virtual Box上で立てたUbuntuサーバーへ、Apacheをインストールしてください。  
  - 参照リンク: http://d.hatena.ne.jp/bicycle1885/20110508/1304871358

- インストールが完了したら、Windows側のインターネットブラウザーから、Apacheへアクセスできるようにしてください。  
