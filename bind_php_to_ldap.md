#PHP から LDAP への接続

##PHP 関数

* ldap_connect: LDAP サーバへ接続する。引数としてホスト名と接続ポート番号を指定する。
  接続に成功した場合はリンク ID を、エラーの場合は false を返す。
  
* ldap_bind: LDAP ディレクトリへバインドする。バインドは 2 種類ある。
  * 匿名バインド
  * 認証バインド: rdn とパスワードが必要。
  
  成功の場合は true を、エラーの場合は false を返す。
  
* ldap_unbind: LDAP ディレクトリへのバインドを解除する。
* ldap_close: LDAP サーバへのリンクを閉じる。
* ldap_add: LDAP ディレクトリへエントリを追加する。
  成功の場合は true を、エラーの場合は false を返す。
  格納するデータは配列で提供し、属性値を設定する。
  
* ldap_search: LDAP ツリーを探索し、検索結果 ID を返す。エラーの場合は false を返す。
* ldap_get_entries: 検索結果の全てのエントリを取得する。検索結果を多次元配列で返す。
  エラーの場合は false を返す。

