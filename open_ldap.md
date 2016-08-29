#Open LDAP

##インストールされるプログラム

* slapd: LDAP サーバの本体。
* slurpd: マスターの slapd サーバの情報をスレーブの slapd サーバへ複製する。
* ldapadd: LDIF ファイルを使用して LDAP サーバへ情報を追加する。
* ldapmodify: LDAP サーバに記録されている情報を変更する。
* ldapdelete: LDAP サーバに記録されている情報を削除する。
* ldapsearch: LDAP サーバの情報を検索する。
* slappasswd: slapd.conf の rootpw で指定するパスワードの暗号化文字列を生成する。
  生成可能な形式は:
  * CRYPT
  * MD5
  * SMD5
  * SSHA(既定値)
  * SHA

* rcpt500: メールシステムを使用して LDAP の情報を検索する。
* go500: Gopher クライアントに対してのゲートウェイを提供する。

##ログファイルの設定

/etc/syslog.conf へ下記を追記する。

```bash
local4.*    /var/adm/ldaplog
```

**Note:**

``local4.*`` と ``/var/adm/ldaplog`` はタブで区切る。
ログファイルのパスは環境に合わせて適宜変更する。

設定を有効にするには syslogd の PID へ HUP シグナルを送信する。