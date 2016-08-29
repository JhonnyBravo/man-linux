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
* slapcat: LDAP データベースの内容を LDIF へダンプする。
* slapindex: slapd.conf を変更した時など、索引の再編成が必要な場合に使用する。

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

##slapd.conf

* include: 属性型やオブジェクトクラスの定義ファイルを指定する。
  core.schema は必須。必要に応じて他のファイルを追加する。
  
* suffix, rootdn: 任意の値を記述する。組織と国の組合せや、ドメイン名に準じた表記が利用されることが多い。
  例) ``"dc=linuxjp,dc=org"``
  
* rootpw: パスワード文字列を指定する。セキュリティの観点から平文ではなく、暗号化文字列を指定することが望ましい。
  ``slappasswd -s secret -h '{SSHA}'`` を使用して暗号化文字列を生成することができる。
  
##slapd オプション
  
* f file_path: slapd.conf のパスを指定する。既定値は /usr/local/slapd.conf, /etc/openldap/slapd.conf。
* n service_name: syslog に記録されるサービス名を定義する。
  ポート番号を変えて slapd を複数起動する場合に使用する。
* l syslog_local_user: syslog のファシリティを指定する。既定値は LOCAL4 。
* d level: デバッグのレベルを指定する。

##slapd の停止

```bash
sudo kill -INT `cat /usr/local/var/slapd.pid`
```

**Note:**

slapd の PID は /usr/local/var/slapd.conf に記録される。

##LDIF(LDAP Data Interchange Format)

LDAP の情報を管理する為に使用するレコードファイル。
一つのレコードに一つの dn が存在し、レコードは空行で区切る。
行の先頭が空白またはタブから始まる場合は前行からの継続とみなされる。

```bash
dn: <識別子>
<属性記述子>: <属性値>
<属性記述子>: <属性値>
...
```

* dn(Distinguished Name): 全レコードを通じて一意の識別子。
* objectClass: LDIF 定義の中で属性の使用許可を得る為に使用する。

**Note:**

下記に該当する場合は属性記述子に続けて ``:`` を二つ置き、 base64 表記でエンコードした値を記述しなければならない。

* 属性値に印字できない文字が含まれている。
* 属性値がスペース, ``:`` , ``<`` から始まる。