# 目次

- 1. Mysqlの使い方

# 1. Mysqlの使い方
### 1.1. コマンド集
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


