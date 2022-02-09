# 客戶端維護相關筆記
### 客戶端(VM)主程式更版
#### Tools
- Cmd(main)
- FileZilla(sup)
#### 初次更版流程
由於舊有的環境與local有差異，所以第一次更版需注意:
1. tomcat7 → tomcat9(目前7會自啟要注意!!不然會占用port號)
將tomcat9移入遠端環境，並修改server.xml中port號
2. jdk7 → jdk8
- `yum -y install java-1.8.0-openjdk java-1.8.0-openjdk-devel`

- `alternatives --config java` → 選擇jdk8

3. /report(舊版report)中有多處path指到tomcat7 Reportfile 要記得更新替換
#### 正常更版流程
1. 連線客戶端: `ssh root@<ip>`
2. 備份原程式(壓縮或是拖曳至其他資料夾皆可)
Path: /root/Desktop/apache-tomcat-9.0.46/webapps
Zip command: `zip -r FileName.zip DirName`
3. 刪除舊的ROOT資料夾: `rm -rf ROOT`
4. 於local產生新版war檔
5. 將新的heis_standalone.war檔丟到webapps下面，它會自動解壓
6. 到/bin下停止tomcat: `sh catalina.sh stop` (建議下兩次指令)
7. 將產生出來的資料夾rename成"ROOT"，並將war檔移至Desktop
8. 到/bin下重啟tomcat: `sh catalina.sh start`
看log輸出: `tail -f -n 300 catalina.out`
9. 若客戶有自行製作的tempalte記得先備份(html/image)或ReportFile下的報告檔案再複製至新的/mailTemplate或/ReportFile下!!!

