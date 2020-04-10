## Aliyun重启后，配置重启Nginx 

1. **停掉系统Nginx (OpenResty)** 

   ```
   sudo service nginx stop 
   ```

2. **清除nginx.pid** 

   ```
   lin@iZ28qwdrakwZ:/$ sudo find . -name 'nginx.pid' 
   
   ./home/lin/work/logs/nginx.pid 
   
   lin@iZ28qwdrakwZ:/$ sudo rm /home/lin/work/logs/nginx.pid 
   ```

3. **启动系统 /etc/init.d/nginx** 

   ```
   lin@iZ28qwdrakwZ:/$ sudo /etc/init.d/nginx start 
   
   Starting nginx: 
   
   lin@iZ28qwdrakwZ:/$ 
   ```