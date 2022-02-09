# 開機自動讀取tomcat script
#### File path：  `/etc/systemd/system/tomcat.service`
#### 舊版內容：
tomcat version `apache-tomcat-9.0.46` 請替換對應版本號

```
[Unit]
Description=Apache Tomcat Project
After=syslog.target network.target

[Service]
Type=forking
Environment=CATALINA_PID=/root/Desktop/apache-tomcat-9.0.46/tomcat.pid
Environment=CATALINA_HOME=/root/Desktop/apache-tomcat-9.0.46/
Environment=CATALINA_BASE=/root/Desktop/apache-tomcat-9.0.46/
ExecStart=/root/Desktop/apache-tomcat-9.0.46/bin/startup.sh
ExecStop=/root/Desktop/apache-tomcat-9.0.46/bin/shutdown.sh
Restart=on-failure
RestartSec=20s

[Install]
WantedBy=multi-user.target
```
# 更改tomcat資料夾權限
`cd /root/Desktop`

`chmod -R 755 apache-tomcat-9.0.46`

# 重新啟動script
- `systemctl stop config.service`
- `systemctl start config.service`
