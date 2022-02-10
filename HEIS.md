# HEIS Notes

## Run on local
### Eclipse run heis_standalone
1. **Switch to local db**  
Path: heis\WebContent\META-INF\context.xml  
更改以下值:  
    - `username`  
    - `password`(需轉base64)  
    -  `url="jdbc:mysql://localhost:3306/HEIS"` →"HEIS"為db name(若無異動可略過)  
 
    ![](https://i.imgur.com/BQ2wyOF.png)  
  

2. **Tomcat 9 server setting**  
Right click and select the "add and remove...",Add heis_standalone,and Finish

    ![](https://i.imgur.com/SkakkYH.png)

3. **Start the server**


### Sender
1. **Modify config .py**  
    `MAIL_TEMPLATE_PATH`:填入mailtemplate擺放路徑
    `APP_CONTENT_PATH`:要填入"/專案名稱"  
    ```
    MAIL_TEMPLATE_PATH = "D:/jessie/Work/testing/heis_standalone/WebContent"
    APP_CONTENT_PATH = '/heis_standalone'
    ```
2. **Install requirements** : `pip install -r requirements.txt`
3. **Run main .py** : `python main.py`


### Report  
1. **Modify i/config .py**  
    `REPORT_PATH`:產出report的擺放位置
    `#Database Configs`:設定local DB資訊
    ```
    # Report DIS
    REPORT_PATH = 'D:/jessie/Work/testing/heis_standalone/WebContent/ReportFile'
    # Database Configs
    DB_CONFIGS = {
      'host' : 'localhost',
      'port' : '3306',
      'user' : 'root',
      'password' : 'jessie1234',
      'database' : 'heis'
    }
    ```
3. **Modify d/config .py**  
    `REPORT_PATH `:產出report的擺放位置report位置
    ```
    # Report DIS
    REPORT_PATH = 'D:/jessie/Work/testing/heis_standalone/WebContent/ReportFile'
    #REPORT_PATH = 'output/'
    ```
5. **Modify Chart .py**  
    `font`:修改字型—linux用`self.label['FONT']`，local環境用`{'family': 'Microsoft JhengHei', 'size': '14'}`
    ```
    def chart_setting(self):
    #font = self.label['FONT']
    font  = {'family': 'Microsoft JhengHei', 'size': '14'}
    mpl.rc('figure', max_open_warning = 0)
    plt.rcParams['axes.unicode_minus']=False
    plt.rc('font', **font)
    self.path = REPORT_PATH + self.task.uuid + '/chart/'
    #self.path = REPORT_PATH + '/zh_TW/' + self.task.uuid + '/chart/'
    ```
7. **Change Server IP Address or Domain**  
    HEIS 左側選單SYSTEM CONFIG → Server IP Address or Domain → 127.0.0.1
9. **Modify data on Mysql**  
       - bas_project:PROJECT_END_TIME (小於當下時間、大於最後行為觸發時間)  
       - bas_project:PROJECT_FLOW (確保狀態為03或01，04是失敗)  
5. **Run main .py** : `python main.py`
