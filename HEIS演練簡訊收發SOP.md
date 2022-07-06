# HEIS演練簡訊收發SOP

### 匯入受測人員名單
至HEIS網站(Group Setting)，下載Group_Update_Example.xlsx，並填寫受測人員資料。填好必填欄位(有＊字號的欄位)後儲存並上傳匯入至HEIS網站。
![image](https://user-images.githubusercontent.com/99180553/177476380-52150da1-0be2-4223-92e5-a5c1d022f42e.png)

### 新增一個演練任務
至TASK MANAGEMENT啟動一個新的演練任務，並選取剛剛匯入的人員群組。

### 匯出BAS_USER Table
1. 開啟MySQL，直接複製貼上以下SQL，並按下「⚡️」執行，在下方Result Grid可以看到Query結果。
```
-- Query BAS_USER 演練人員名單資料表
SELECT * FROM HEIS.BAS_USER;
```
2. 點擊Export 匯出 CSV(;separated) 檔案。注意‼️ 是選擇**CSV(;separated)**

![image](https://user-images.githubusercontent.com/99180553/177485603-429ab92a-77c6-400a-a5f6-2e7007b8d51f.png)

<img width="744" alt="截圖 2022-07-06 下午2 47 49" src="https://user-images.githubusercontent.com/99180553/177487125-2b233ac3-b609-4190-9d3d-328c9e5e3bb4.png">


