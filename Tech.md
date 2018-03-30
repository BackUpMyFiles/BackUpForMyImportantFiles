# 目次
- Database  
1.1. データベースの種類(仕組み別)
1.2. データベースの種類(アプリケーション別)

- Programming Language  
2.1. Go
2.2. JavaScipt

- Protocol
3.1. SSH

- Linux Command
4.1. netstat
4.2. find

- Git
5.1. Gitの仕組み
5.2. Command

- CacheとCookie
6.1. Cache
6.2. Cookie


# 1. Database
## 1.1. データベースの種類(仕組み別)
### 1.1.1. リレーショナルデータベース
#### INDEX
B-treeインデックスについて。
ヘッダーブロック（最上層）、ブランチブロック、リーフブロック（最下層）に分けて管理されていて、
それぞれ近隣のブロックへのポインターも持っている。

URL: http://www.hi-ho.ne.jp/tsumiki/doc_1.html

## 1.2. データベースの種類(アプリケーション別)
### 1.2.1. MySQL
#### コマンド集
1. mysqlにrootユーザーで接続
=> $ mysql -uroot

2. データベース一覧表示
=> show databases;

3. データベースを使う
=> use (database name);

4. テーブル作成(use (database name);後)
=> CREATE TABLE (table name)((col name 1) (data type1), (col name2) (data type 2) , ...)

NULL を許可するか設定する場合、
=> CREATE TABLE (table name)((col name 1) (data type1) NULL, (col name2) (data type 2) NOT NULL, ...)

AUTO INCREMENT の設定をする場合、
=> CREATE TABLE (table name)((col name 1) (data type1) AUTO_INCREMENT, (col name2) (data type 2), ..., INDEX(col name 1))

5. カラムの取得
=> SHOW COLUMNS FROM (table name)

6. カラムの変更
=> ALTER TABLE (table name) CHANGE (old column name) (new column name) (column definition);
ex) ALTER TABLE student CHANGE id student_id NOT NULL;

7. データの追加
=> INSERT INTO (table name)((col name 1), (col name 2), ...) VALUES ((value1), (value2), ...);

8. データの取得
全て取得
=> SELECT * FROM (table name);

9. データの削除
=> DELETE FROM (table name) WHERE (condition);
ex) DELETE FROM student WHERE id = 10;


# 2. Programming Language
## 2.1. Go
### 2.1.1. スライスについて
・スライスに渡した、部分列の要素は元の配列とメモリが共有される。
### 2.1.2. Deferについて
・deferは、その関数が終了した時に呼び出されるべき処理を示す。
・deferとしてメモリの解放の処理などを指定しておくと、メモリの解放漏れなどがなくてすむ。

## 2.2. JavaScript
### 2.2.1. Ajax通信
Ajax通信とは、JavaScriptとXMLを使って、画面の一部のみを取得することによって画面遷移をすることなしにページを閲覧できる。
画面の一部の情報をサーバーに送信してそれを受け取り反映させる仕組みなので、レスポンスがなくても処理が継続できる。
例) Google Map

Ajax通信を支える仕組みは、以下の4つである。
(1) XMLHttpRequest
(2) JavaScript
(3) DOM
(4) XML

#### XMLHttpRequest
クライアント側で、クライアントとサーバー間でデータを伝送する機能を提供するAPI。
XMLHttpRequestはJavaScriptの組み込みオブジェクトのため、JavaScriptでなければならない。

#### DOM
Document Object Modelの略。HTML、XMLドキュメントのためのAPI。
Ajaxでは、動的なWebページを作成するときにHTML、XMLのどの要素を変更するか指定する。
そこで、DOMはHTMLやXMLをツリー構造として展開し、アプリケーション側に文章の情報を伝えて加工や変更をしやすくする。

全体の流れとしては、以下のようになっている。
(1) ページ上でイベントが発生し、JavaScript + XMLHttpRequestによってサーバに対してリクエスト送信(非同期通信)
(2) サーバは受け取った情報を処理し、JSONやXMLで応答
(3) DOMを使ってサーバーからのレスポンスを処理し、指定したページの部分のみを更新。（そのため、画面遷移のために画面が白くなったりしない）

サンプルコードは以下。
URL: https://qiita.com/hisamura333/items/e3ea6ae549eb09b7efb9

### 2.2.2. JQuery
#### 画面の要素の座標を獲得する
URL: https://lab.syncer.jp/Web/JavaScript/Snippet/10/


# 3. Protocol
## 3.1. SSH
OSI参照モデルでいうアプリケーション層に存在するプロトコル(HTTPなどもそう)。SSHを使うことによって、リモートサーバーに安全にアクセスしたり、ファイルを安全に送信したりする。
「Secure SHell」の略称で、公開鍵認証を用いてセキュアな通信を実現している。

SSHには以下の2種類のログイン認証が存在する。
(1) パスワード認証
(2) 公開鍵認証

### 3.1.1. ~/.ssh/known_hosts
一度接続したことのあるサーバーのSSHの証明書はこのファイル内に格納される。

### 3.1.2. 公開鍵認証について
鍵の生成は以下のコマンドをターミナルに打ち込む。
```
$ ssh-keygen
```

これにより、~/.sshに、id_rsaとid_rsa.pubが設置される。

<hr>
###### 補足: 鍵の種類について
rsa ... 鍵の長さが4096bitと長め。(最新式)
dsa ... SSH2で使える。鍵が1024bitと短め。
rsa1 ... SSH1で使える。しかし、SSH1には脆弱性が存在する。

<hr>
###### 補足: パスフレーズについて
秘密鍵を使うために必要なもの。これにより、秘密鍵が漏洩したりしてしまっても大丈夫なようになっている。
パスフレーズの変更は以下のコマンドで行える。
```
$ ssh-keygen -p
```

<hr>

次に、公開鍵をサーバー側に設置する。
まず、SCPを用いてセキュアにファイルをコピーする。
以下のコマンドを打ち込む。

```
$ scp ~/.ssh/id_rsa.pub (Remote User Name)@(Server Address):~
```

ここからはサーバー側で操作を行う。
以下のコマンドを用いる。

```
\# SSHログイン
$ ssh (Remote User Name)@(Server Address)

\# 公開鍵がコピーされていることを確認
$ cd ~
$ cat id_rsa.pub

\# .sshディレクトリを作成 & パーミッションを変更
$ mkdir ~/.ssh
$ chmod 700 .ssh

\# catリダイレクト(">>")を用いて、.ssh/authorized_keysに内容を追加
$ cat ~/id_rsa.pub >> .ssh/authorized_keys
```

### 3.1.3. sshd
SSHは、他のサーバーに接続するための設定。
SSHDは、他のサーバーから接続される時の設定。
SSHDはデーモンであるが、デーモンとはメモリ上で待機している常駐プログラムのUNIX OS系での呼び名。

# 4. Linux Command
## 4.1. netstat
開いているポートを確認できる。通常は以下のように用いるのが普通である。

```
$ netstat -an | grep (Port Number)
```

Local Addressとは、自分のIPアドレスと開いているポート番号が示される。
Foreign Addressとは、自分に接続している相手のIPアドレスと、そのポート番号が示される。
(state)には、自分と相手の状態が示される。

LISTENING ... 接続待ち
ESTABLISHED ... 接続
CLOSE_WAIT ... 切断中

を示す。

## 4.2. find
現在いるディレクトリより下からファイルを探す。

```
$ find / -name (File Name)
```


# 5. Git
バージョン管理システム。
ファイルをスナップショットとして.gitファイル(ローカルのデータベース)に保存することによってバージョン管理を行う。

Q: なぜローカルが良いのか?
=> A:
Q: なぜスナップショットなのか?
=> A:

## 5.1. Gitの仕組み
### 5.1.1. 概要
まず、ファイルは以下の2種類に分けられる。
(1) 追跡されるもの
(2) 追跡されないもの

追跡されるファイルには、以下の3つの種類の状態が存在する
(1) Unmodified ... 一番新しい状態
(2) Modified ... 修正された状態
(3) Staged ... Commitするためにステージにあげられた状態

### 5.1.2. .gitignore
最初に作っておくことが進められる。
追跡せず、その表示もしなくなる。

書き方は以下を参照
URL: https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E5%9F%BA%E6%9C%AC-%E5%A4%89%E6%9B%B4%E5%86%85%E5%AE%B9%E3%81%AE%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA%E3%81%B8%E3%81%AE%E8%A8%98%E9%8C%B2

### 5.1.3. リモートリポジトリ
複数人と共同作業をするときに用いる。
インターネット上に存在するプロジェクトのこと。複数のリモートリポジトリを持つこともできる。
リモートリポジトリの管理には、「リモートリポジトリの追加」「不要になったリモートリポジトリの削除」「リモートブランチの管理や、追跡対象、対象外の設定」
など、様々なものが含まれる。

### 5.1.4. ブランチについて
ブランチとは、本流から分離し、邪魔をすることなく開発を続けることができる機能である。
他のバージョン管理アプリと比べると、Gitのブランチは圧倒的に軽量でありブランチの切り替えも容易であるため、
ブランチを頻繁に作成し、切り替える開発を推奨している。

Commitを行うと、
(1) Commitオブジェクト ... メタデータ、その前のCommitオブジェクトへのポインタやルートツリーへのポインタなどを格納
(2) Treeオブジェクト ... BlobデータへのポインタとCommitオブジェクトへのポインタを格納
(3) Blobオブジェクト ... ファイル１つ１つがデータベースに格納されうるデータ型に変換されたもの。(Binary Large Objectの略)

※(2), (3)をまとめてスナップショットと呼ぶ

ブランチとは、親Commitオブジェクトに対して（１つ前のCommitオブジェクト)に対して、新しいポインタを作るだけ。
(実際には、特定のコミットを示すSHA-1チェックサムを保持している)
また、現在作業しているブランチはHEADオブジェクトという特殊なオブジェクトによって管理されている。

ブランチの仕組み上、ブランチを切って古いブランチに切り替えた場合、
ファイルの内容も元に戻ることに注意！

URL: https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E3%83%96%E3%83%A9%E3%83%B3%E3%83%81%E6%A9%9F%E8%83%BD-%E3%83%96%E3%83%A9%E3%83%B3%E3%83%81%E3%81%A8%E3%81%AF

Q: ブランチをきる理由は?
=> A: Webサイトを運用していたとする。新しい開発をするときにブランチを切ることによって、運用している段階のものには変更が反映されない。仮に、実運用のものに変更をしてほしいと要請があった場合、元のブランチに戻れば良い。


## 5.2. Command
### 5.2.1. git add
追跡されていないファイルや、変更されたファイルをステージ(次のCommitに含める)にあげるためのコマンド。

How to Use:
```
$ git add (ファイル名)
```

```
# 全てのファイルをステージに
$ git add *
```

### 5.2.2. git status
ファイルの状態を表示するためのコマンド。

How to Use:
```
$ git status
```

```
# 簡略化して表示
$ git status -s
```
ステージングエリアに追加されたファイルはA, 変更されたファイルはM, 追跡されていないものは??と表示される。

### 5.2.3. git diff
ステージされていない変更内容を表示する。
直前のコミットと、ステージされているものの違いを確認したいときは、--stagedコマンドをつける。

How to Use:
```
$ git diff
```

```
$ git diff --staged
```

### 5.2.4. git commit
ステージされているものをコミットする(スナップショットとしてローカルのデータベースに保存する)。
インラインコメントを-mオプションによってつけることができる。
また、git addを省略したいときは、-aコマンドをつける。

```
$ git commit -m "(コメント)"
```

```
$ git commit -a -m "(コメント)"
```

### 5.2.5. git rm
Gitリポジトリからファイルを削除する方法。追跡も解除し、ハードディスク上からも消す。(Git上でファイルを消すにはこのコマンドを用いた方がいい)
ハードディスクから消したくない場合は、--cachedオプションを使用する。

```
$ git rm (ファイル名)
```

```
$ git rm --cached (ファイル名)
```

### 5.2.6. git remote関連
リモートリポジトリの追加。

```
$ git remote add (Short Name) (URL)
# Ex) $ git remote add pb https://github.com/paulboone/ticgit
```

これにより、自分のリポジトリにないものでも、pbにあるものは以下のコマンドで持ってくることができる。

```
$ git fetch pb
```

このコマンドの実行後は、そのリモートリポジトリにあるブランチを自分のものにマージしたり、
チェックアウトして中身を調べたりすることができる。

また、どのリポジトリをリモートリポジトリに設定しているかは、以下のコマンドで調べることができる。

```
$ git remote -v
```

また、リモートリポジトリの詳細を調べたい場合は、以下のコマンドを用いる。

```
$ git remote show (Remote Repository Name)
# Ex) $ git remote show origin
```

originとは、git cloneしてきたときにリモートリポジトリに対してデフォルトでつけられる名前である。

### 5.2.7. git log
このコマンドは、コミットの履歴を表示するものである。
多様なオプションがあるので、見たいものを見ることができる。

URL: https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E5%9F%BA%E6%9C%AC-%E3%82%B3%E3%83%9F%E3%83%83%E3%83%88%E5%B1%A5%E6%AD%B4%E3%81%AE%E9%96%B2%E8%A6%A7

切ったブランチがどのCommitオブジェクトへのポインタを保持しているかを表示するには以下のコマンドを用いる。
```
$ git log --oneline --decorate
```

### 5.2.8. git branch
新しいブランチを切るときに使うコマンド。

```
$ git branch (ブランチ名)
```

ブランチの削除は以下のように、-dオプションをつける。

```
$ git branch -d (ブランチ名)
```

### 5.2.9. git checkout
ブランチを切り替えるときに使うコマンド。

```
$ git checkout (ブランチ名)
```

ブランチの切り替えと作成を同時に行うときは、以下の-bオプションを使用する。
```
$ git checkout -b (ブランチ名)
```

### 5.2.10. git merge
ブランチをマージする。
(1) Fast-forwardマージ
(2) マージ

これらの違いについては以下のリンクを参照
URL: https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E3%83%96%E3%83%A9%E3%83%B3%E3%83%81%E6%A9%9F%E8%83%BD-%E3%83%96%E3%83%A9%E3%83%B3%E3%83%81%E3%81%A8%E3%83%9E%E3%83%BC%E3%82%B8%E3%81%AE%E5%9F%BA%E6%9C%AC

どちらも以下のコマンドで可能。
```
$ git merge (ブランチ名)
```
これにより、masterブランチにmergeされる。

(2)のマージのみ、コンフリクトが起こる可能性がある。


# 6. CacheとCookie
## 6.1. Cache
Cacheとは、主にWebブラウザなどで一度見たページの動画や画像をローカルPC上のキャッシュメモリの上に保存しておくことで
二回目以降に閲覧したときに読み込みを早くする機能。

Macでは、以下のパスにGoogle Chromeのキャッシュが保存されている。
~/Library/Caches/com.google.chrome

また、chrome://view-http-cache/とGoogle Chromeに打ち込むとCacheが確認できる。

Q: Cacheを用いてほしい画像や動画を持ってこれないか？
=> A: 

## 6.2. Cookie
Cookieとは、主にWebブラウザなどでページを見たとき、ユーザーの情報をローカルPC上に保存しておく機能。
サイトの方からアクセスすることができ、ユーザーの情報をもらい、それに適したサービスを展開することができる。
