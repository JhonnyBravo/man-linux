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