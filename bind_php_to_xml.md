#PHP から XML を利用する方法

##PHP 関数

* xml_parser_create(文字エンコーディング): XML パーサを新規作成する。
  引数は XML 文書の文字エンコーディングを指定する。
  戻り値として XML 文書パーサを示すパーサ番号を返す。
  
* xml_set_element_handler(パーサ番号, 開始タグハンドラ, 終了タグハンドラ):
  XML 要素の開始 / 終了用のイベントハンドラ(処理プログラム)を定義する。
  開始タグハンドラには開始タグを処理する関数の名前を、
  終了タグハンドラには終了タグを処理する関数の名前を指定する。
  戻り値として定義動作の可否を返す。
  
* xml_set_character_data_handler(パーサ番号, 文字データハンドラ):
  文字データ用のイベントハンドラを定義する。文字データハンドラには文字データを処理する関数の名前を指定する。
  
* xml_set_processing_instruction_handler(パーサ番号, PI ハンドラ):
  PI タグ(処理用命令)の処理を定義する。
  
* xml_set_default_handler(パーサ番号, デフォルトハンドラ):
  他のハンドラで規定されない処理をデフォルトハンドラで実行する。

* xml_parse(パーサ番号, 文字データ, 終了マーク):
  XML 文書の処理を開始する。パーサで処理すべき文字データを引数へ渡す。
  終了マークへ True を指定すると解析を終了する。
  解析エラーが無ければ True を、エラーが発生した場合は False を返す。
  
* xml_get_error_code(パーサ番号): XML パーサのエラーコードを取得する。
* xml_error_string(エラーコード): XML パーサのエラー文字列を取得する。
* xml_get_current_line_number(パーサ番号): XML パーサが現在解析中の行番号を取得する。
* xml_get_current_column_number(パーサ番号): XML パーサが現在解析中の列番号を取得する。
* xml_get_current_byte_index(パーサ番号):
  XML パーサが現在解析中のバイトインデックス(ファイルの先頭から数えて何バイト目か)を取得する。
  
* xml_parser_free(パーサ番号): XML パーサを解放する。