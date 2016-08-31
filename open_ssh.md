#Open SSH

##設定ファイル

* ~/.ssh/config: ユーザ毎の設定。
* /etc/ssh_config: システム全体の設定。
* /usr/local/etc/ssh_config: システム全体の設定。

優先順位は下記の通り。

1. コマンドラインのオプション
2. ~/.ssh/config
3. /etc/ssh_config, /usr/local/etc/ssh_config

ユーザ毎の設定ファイルとシステム全体の設定ファイルは双方共に同じ規則で記述する。

```bash
Host domain_name
  attribute value

Host domain_name
  attribute value
```

設定ファイルは複数の Host 指定で区切ることができ、各ホストに対する設定を別々に記述することができる。
Host の指定には ``*`` や ``?`` 等のワイルドカードを使用することができる。

##ssh コマンド

```bash
ssh [option] host_name [remote_command]
```

###オプション

* l user_name: ユーザ名を指定してリモートホストへ接続する。
  オプションを指定しない場合はローカルホストでログインしているユーザ名でリモートホストへ接続する。
* n: ssh の入力を /dev/null へリダイレクトする。
* p port_number: ssh 接続に使用するポート番号を指定する。
* C: 転送データが全て圧縮される。
* 2: SSH バージョン 2 のプロトコルを使用して接続する。
* 4: IPv4 を使用して接続する。
* 6: IPv6 を使用して接続する。
* V: ssh のバージョン番号を表示する。
* a: 認証エージェントを転送しない。(設定ファイルへ ``ForwardAgent no`` を指定する場合と同じ)
* A: 認証エージェントを転送する。(設定ファイルへ ``ForwardAgent yes`` を指定する場合と同じ)
* c algorithm_name: データ転送の際に使用する暗号化アルゴリズムを指定する。使用可能なアルゴリズム:
  * 3des(既定値)
  * blowfish
  
  ssh バージョン 2 では下記を同時に指定することができる。
  * 3des-cbc
  * blowfish-cbc
  * arcfour
  * cast128-cbc
  
  同時に指定する場合は ``,`` 区切りで並記する。
  
  ```bash
  3des-cbc,blowfish-cbc,arcfour,cast128-cbc
  ```
  
* e character: エスケープ文字を指定する。既定値は ``~`` 。
  エスケープ文字に続いて ``.`` を入力すると ssh 接続を解除する。
  エスケープ文字に続いて ``^Z`` を入力すると ssh 接続を中断する。
  ``-e none`` を指定した場合はエスケープ機能が無効となる。
  
* i key_file: RSA 認証用の秘密鍵ファイルを指定する。既定値は ``~/.ssh/identity``
  複数の i オプションを指定して複数の鍵ファイルを指定することもできる。
  
* v: デバッグメッセージを表示する。
* f: 接続認証後にバックグラウンドへフォークする。
* o config_file_option: 設定ファイルと同じ記法でオプションを指定することができる。
* P: 特権ポートの使用しない。
* q: 警告メッセージを表示しない。
* t: 疑似 tty を用意する。
* T: 疑似 tty を用意しない。 SSH バージョン 2 プロトコルのみで有効。
* x: X11 接続の転送をしない。
* X: X11 接続の転送を行う。
* L listen_port:host:port ローカルポートをリモートへ転送する。
* R listen_port:host:port リモートポートをローカルへ転送する。
* g: リモートホストからローカルへ転送されたポートへの接続を許可する。

##ssh-agent

* ssh-agent: 認証鍵をメモリ上に保管する。
* ssh-add: ssh-agent へ認証鍵を登録する。

```bash
ssh-agent [-cs] [-k] [command] [args]
```

ssh-agent を起動すると下記の環境変数が設定される。

* SSH_AGENT_PID: ssh-agent の PID 。
* SSH_AUTH_SOCK: ssh-add との通信に使用するソケット。
  ソケットのパーミッションは ssh-agent を起動したユーザ以外からは読み書きできない。

ssh-add は上記の環境変数を調べてユーザが入力した認証鍵を ssh-agent へ登録する。
認証鍵の登録後、パスフレーズなしでリモートホストへ接続できるようになる。

ssh-agent をサブシェルで起動する場合は下記を実行する。

```bash
ssh-agent /bin/sh
ssh-add
ssh domain_name
```

ssh-agent はサブシェルを終了すると同時に終了し、 ssh-agent が終了した時点でメモリに記憶された認証鍵は破棄される。

ssh-agent をフォークして常駐させる場合は下記を実行する。

```bash
eval `ssh-agent -c`
ssh-add
ssh domain_name
```

常駐している ssh-agent を終了する場合は下記を実行する。

```bash
eval `ssh-agent -c -k`
```

###ssh-agent オプション

* c: ssh-agent を起動し、環境変数を設定する。
* s: ssh-agent を sh を使用して起動し、環境変数を設定する。
* k: ssh-agent を終了し、環境変数の設定を解除する。

##scp

```bash
scp [option] [user@host1:] file1 [user@host2:] file2
```

SSH プロトコルを使用してファイルのコピーを行う。
file1 がコピー元、 file2 がコピー先となる。

###scp オプション

* c cipher: 暗号化アルゴリズムを指定する。
* i file: RSA 認証鍵ファイルを指定する。
* p: コピー後もファイルの mtime, atime 時刻とモードを維持する。
* r: ディレクトリの内容を再帰的にコピーする。
* v: デバッグメッセージを表示する。
* B: バッチモードを指定する。
* q: コピーの進捗を非表示にする。
* C: 転送データを圧縮する。
* P port: ssh の接続ポートを指定する。
* 4: IPv4 を使用する。
* 6: IPv6 を使用する。

##ポート転送

```bash
# ローカルからリモートへ転送する。
ssh -L 転送先ポート番号:転送元ホスト名:転送元ポート番号 ユーザ名@ホスト名

# リモートからローカルへ転送する。
ssh -R 転送先ポート番号:転送元ホスト名:転送元ポート番号 ユーザ名@ホスト名
```

1024 番以下のポートは特権ポートである為、特権ポートに対しての転送は root のみに制限されている。