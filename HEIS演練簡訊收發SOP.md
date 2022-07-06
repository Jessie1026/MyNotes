# HEIS演練簡訊收發SOP

## HEIS 
### 匯入受測人員名單
至HEIS網站(Group Setting)，下載Group_Update_Example.xlsx，並填寫受測人員資料。填好必填欄位(有＊字號的欄位)後儲存並上傳匯入至HEIS網站。
![image](https://user-images.githubusercontent.com/99180553/177476380-52150da1-0be2-4223-92e5-a5c1d022f42e.png)

### 新增一個演練任務
至TASK MANAGEMENT啟動一個新的演練任務，並選取剛剛匯入的人員群組。

## MySQL
### 匯出BAS_USER Table
1. 開啟MySQL，直接複製貼上以下SQL，並按下「⚡️」執行，在下方Result Grid可以看到Query結果。
```
-- Query BAS_USER 演練人員名單資料表
SELECT * FROM HEIS.BAS_USER;
```
2. 點擊Export 匯出 CSV(;separated) 檔案。注意‼️ 是選擇**CSV(;separated)**

![image](https://user-images.githubusercontent.com/99180553/177485603-429ab92a-77c6-400a-a5f6-2e7007b8d51f.png)

<img width="744" alt="截圖 2022-07-06 下午2 47 49" src="https://user-images.githubusercontent.com/99180553/177487125-2b233ac3-b609-4190-9d3d-328c9e5e3bb4.png">


## Excel
### 在Office Excel匯入產出的CSV檔
[使用 Office Excel 載入 CSV 檔案的陷阱與技巧](https://www.youtube.com/watch?v=QJxYOkOs8Ow)

### 在Google試算表匯入產出的CSV檔
左上選項檔案>匯入
<img width="1440" alt="截圖 2022-07-06 下午4 35 02" src="https://user-images.githubusercontent.com/99180553/177507469-a561facd-3330-4511-871a-af4964c0d18e.png">
彈跳視窗相關設定參考以下截圖，主要設定就是「自訂分隔符類型」、「;」(半形)，點選匯入即可編輯查看資料。  
<img width="530" alt="截圖 2022-07-06 下午4 37 53" src="https://user-images.githubusercontent.com/99180553/177508151-c3edb037-ff5f-4a54-8df5-148e45491891.png">


