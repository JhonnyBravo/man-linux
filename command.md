#コマンド覚書

```bash
alias alias_name=command
```

コマンドのエイリアスを設定する。

```bash
cat [-nbv] [file_path]
```

指定したファイルの内容を標準出力へ表示する。

**オプション:**

* n: 行番号を付けて出力する。
* b: 行番号を付けて出力する。空白行をカウントしない。
* v: 表示できない文字をエスケープシーケンスの表記法に置換する。

```bash
wc [-clw] [file_path]
```

ファイルの行数・単語数・文字数をカウントする。

**オプション:**

* c: 文字数をカウントする。
* l: 行数をカウントする。
* w: 単語数をカウントする。

オプションを省略した場合は文字数・行数・単語数が全て出力される。

```bash
tr [search_strings] [replaced_strings] [file_path]
```

文字列を置換する。

```bash
man [-s] [section_number] [command_name]
man [-k] [key_word]
```

マニュアルを表示する。

```bash
grep [-cilnv] [pattern] [file_path]
```

pattern に合致する文字列を抽出して表示する。

**オプション:**

* l: ファイル名だけを出力する。
* c: pattern を含む行の数を出力する。
* i: 大文字・小文字を問わずに pattern を適用する。
* v: pattern に合致しない行を出力する。

```bash
`command`
```

バッククオートに囲まれたコマンドは実行結果と置換される。

```bash
tee [-a] [file_path]
```

標準入力をファイルと標準出力の双方へ出力する。

**オプション:**

* a: 追記モードでファイルへ出力する。

```bash
sort [-u] [-t separation] [-k sort_key] [file_name]
```

入力を並べ替える。

**オプション:**

* u: 重複行を削除して並べ替えを実行する。
* k: 並べ替えのキーにするフィールドを指定する。
* t: フィールドの区切り文字を指定する。

```bash
kill [-signal] pid
kill -l
```

プロセスへシグナルを送信する。
signal を指定しない場合は TERM(終了)シグナルを送信する。

**オプション:**

* l: 送信可能なシグナルの一覧を表示する。

```bash
for [var_name] in [list];
do
  command
done
```

for ループ。

```bash
while [condition];
do
  command
done
```

while ループ。

```bash
until [condition];
do
  command
done
```

until ループ。

```bash
if [condition]; then
  command
elif [condition]; then
  command
else
  command
fi
```

if 条件分岐。

```bash
case [var_name] in
[pattern])
  command
  ;;
esac
```

case 条件分岐。

```bash
function name(){
  command
  return value
}
```

関数を定義する。

```bash
trap [command] [signal]
```

指定した signal が発生した時に command を実行する。

```bash
rsync [option] source destination
```

リモートマシンとの間でファイルのコピーを行う。
リモートとローカルの双方に rsync のインストールが必要となる。
コピー元とコピー先に同名ファイルが存在する場合は差分のみをコピーする。
[ssh](open_ssh.md) と併用されることが多い。

**引数:**

* source: コピー元
* destination: コピー先

ファイル名やパスを直接記述することができ、 ``user_name@host_name:path`` の形式で記述することもできる。