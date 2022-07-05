# MySQL常見問題集

## [Solved] Error Code: 2013. Lost connection to MySQL server during query
> Caused by: If you spend time running lots of MySQL queries, you might come across the Error Code: 2013. Lost connection to MySQL server during query. This article offers some suggestions on how to avoid or fix the problem.
1. 編輯`my.cnf`檔案。（需注意有些環境是叫`my.ini`）
```
vi /etc/my.cnf
```
2. 在[mysqld]之下增加以下設定。（網上有人設16M就解決了，但我開到32M才成功）
```
max_allowed_packet=32M
```
3. 重啟MySQL
```
service mysqld restart 
service mysql restart (5.5.7版本命令)
```
