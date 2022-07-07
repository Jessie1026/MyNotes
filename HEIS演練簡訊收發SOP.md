# HEIS演練簡訊收發SOP

## HEIS 
### 匯入受測人員名單
至HEIS網站(Group Setting)，下載Group_Update_Example.xlsx，並填寫受測人員資料。填好必填欄位(有＊字號的欄位)後儲存並上傳匯入至HEIS網站。
![image](https://user-images.githubusercontent.com/99180553/177476380-52150da1-0be2-4223-92e5-a5c1d022f42e.png)

### 新增一個演練任務
至TASK MANAGEMENT啟動一個新的演練任務，並選取剛剛匯入的人員群組。

### 啟動任務
將剛剛新增的任務啟動，紅框處留意「任務名稱」。
![image](https://user-images.githubusercontent.com/99180553/177673050-973f714b-7a83-4932-9709-c1ecf6b2d905.png)


## MySQL
直接複製貼上以下SQL，並按下「⚡️」執行，在下方Result Grid可以看到Query結果。

### Query BAS_USER 全部演練人員名單資料表:
```
SELECT * FROM HEIS.BAS_USER;
```


### 用任務名稱取得此次演練任務PROJECT_UUID:
>本次範例`PROJECT_UUID＝b87d2274c53e41388daf5ef06b8b5440`
```
SELECT PROJECT_UUID FROM HEIS.BAS_PROJECT WHERE PROJECT_NAME = 'txt'; 
```
![image](https://user-images.githubusercontent.com/99180553/177677160-8c063ff7-2ca4-424b-a66d-9a91ba9f27ea.png)


### 用演練任務PROJECT_UUID Query Mail_UUID: 
>本次範例`PROJECT_UUID＝b87d2274c53e41388daf5ef06b8b5440`
```
SELECT * FROM HEIS.MAIL WHERE PROJECT_UUID = 'b87d2274c53e41388daf5ef06b8b5440';
```
![image](https://user-images.githubusercontent.com/99180553/177678038-d20feba2-75d9-4a2f-a136-14bb1e1891d8.png)


### 待寄出信件資訊:
> 將`PROJECT_UUID = 'b87d2274c53e41388daf5ef06b8b5440'`替換成您的`PROJECT_UUID` !!  
> 這邊語法搜出來的資料有直接幫您把收件者名字列在最右邊且同USER_UUID皆擺在一起，更方便查對
```
SELECT HEIS.MAIL_DETAIL.*,HEIS.BAS_USER.USER_NAME
FROM HEIS.MAIL_DETAIL
RIGHT JOIN HEIS.BAS_USER
ON MAIL_DETAIL.USER_UUID = BAS_USER.USER_UUID
WHERE PROJECT_UUID = 'b87d2274c53e41388daf5ef06b8b5440'
ORDER BY MAIL_DETAIL.USER_UUID;
```
![image](https://user-images.githubusercontent.com/99180553/177679789-ffa2268f-9c4a-400a-bd1d-ccab5687f533.png)

### ACTION_LIST:
>[基本]Query ACTION_LIST，排除`ACTION_TYPE = '01'`且ACTION_TIME行為時間最新的會在最上面:
```
SELECT * FROM HEIS.ACTION_LIST WHERE ACTION_TYPE = '02' ORDER BY ACTION_TIME DESC ;
```
>[進階]Query ACTION_LIST，排除`ACTION_TYPE = '01'`且ACTION_TIME行為時間最新的會在最上面，額外增加`USER_UUID`方便查對！
```
SELECT HEIS.ACTION_LIST.*,HEIS.MAIL_DETAIL.USER_UUID
FROM HEIS.ACTION_LIST
RIGHT JOIN HEIS.MAIL_DETAIL
ON ACTION_LIST.MAIL_DETAIL_UUID = MAIL_DETAIL.MAIL_DETAIL_UUID
WHERE ACTION_TYPE = '02'
ORDER BY ACTION_TIME DESC;
```


### 匯出Table
點擊Export 匯出 CSV(;separated) 檔案。注意‼️ 是選擇**CSV(;separated)**

![image](https://user-images.githubusercontent.com/99180553/177485603-429ab92a-77c6-400a-a5f6-2e7007b8d51f.png)

<img width="744" alt="截圖 2022-07-06 下午2 47 49" src="https://user-images.githubusercontent.com/99180553/177487125-2b233ac3-b609-4190-9d3d-328c9e5e3bb4.png">

## 製作簡訊
### 手工重製連結
>要替換的變數有兩個：`<HEIS_SERVER_IP>`及`<MAIL_DETAIL_UUID>`。  

基本公式：
```
http://<HEIS_SERVER_IP>/entrance.jsp?id=<MAIL_DETAIL_UUID>&word=02
```
範例：
```
http://61.219.126.3:8080/entrance.jsp?id=d4753990d12749cfb1241041079dff4e&word=02
```

### 將重製連結轉成短連結
Tools: [短連結產生器](https://tinyurl.com/app/) 需註冊！！     

＊Enter a long URL to make a TinyURL：貼上前面重置好的連結。  
＊Customize your link：填上自定義的別名       
＊結果範例：`tinyurl.com/xecure`      
![image](https://user-images.githubusercontent.com/99180553/177684960-ce030f1b-d05d-42f1-9a95-83d9c61ea74c.png)



## Excel
### 在Office Excel匯入產出的CSV檔
[使用 Office Excel 載入 CSV 檔案的陷阱與技巧](https://www.youtube.com/watch?v=QJxYOkOs8Ow)

### 在Google試算表匯入產出的CSV檔
左上選項檔案>匯入指定CSV檔案
<img width="1440" alt="截圖 2022-07-06 下午4 35 02" src="https://user-images.githubusercontent.com/99180553/177507469-a561facd-3330-4511-871a-af4964c0d18e.png">
彈跳視窗相關設定參考以下截圖，主要設定就是「自訂分隔符類型」、「;」(半形)，點選匯入即可編輯查看資料。  
<img width="530" alt="截圖 2022-07-06 下午4 37 53" src="https://user-images.githubusercontent.com/99180553/177508151-c3edb037-ff5f-4a54-8df5-148e45491891.png">


