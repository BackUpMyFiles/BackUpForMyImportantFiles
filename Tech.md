# 目次


- Database  
1.1. Mysql

- Programming Language  
2.1. Go

- Protocol
3.1. SSH


# 1. Database
## 1.1. Mysqlの使い方
### 1.1.1. コマンド集
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

