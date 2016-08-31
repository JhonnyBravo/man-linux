#syslogd

カーネルやデーモンが出力するログを一括で収集 / 記録する。

##/etc/syslog.conf

```bash
*.info    /var/log/messages
*.info    @loghost
```

どのファイルに何のログをどのように記録するかを設定する。
``#`` から始まる行はコメント行とみなされる。
設定は下記の形式で記述する。

```bash
セレクタ    出力方法
```

セレクタと出力方法はタブ文字で区切る。

###セレクタ

下記の形式で記述する。

```bash
facility.level
```

* facility: ログの出力元。
* level: ログの重要度。

###出力方法

* ファイルへ出力する: /var/log/messages
* デバイスへ出力する: /dev/console
* 名前付きのパイプへ出力する: | /usr/local/bin/log.mess
* 他のホストへ出力する: @loghost
* ユーザ: user1

ネットワーク上の他ホストへ出力する場合は ``@`` に続けてホスト名を指定する。
他ホストへ出力する場合は転送先の /etc/hosts ファイルを下記のように定義しておく。

```bash
192.168.0.2    loghost
```

##syslogd の再起動

```bash
sudo kill -HUP `cat /var/run/syslogd.pid`
```