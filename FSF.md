# FSF
---
1. **sudo yum install fsf**
2. **sudo vi /opt/fsf/fsf-server/conf/config.py**
3. **LINE 9 LOG_PATH changed to : '/data/fsf/logs'**
4. **LINE 18 SERER_CONFIG changed to : "local host"**
5. **LINE 10 YARA_PATH changed to '/var/lib/yara-rules/rules.yara'**
6. **LINE 12 EXPORT_PATH changed to '/data/fsf/archive'**
7. **LINE 11 PID_PATH changed to '/run/fsf/fsf.pid'**
---
### Now We Are Going to Make Those Directories
---
8. **cd /data/fsf/**
9. **ll**
10. **sudo mkdir -p /data/fsf/{logs,archive}**
11. **sudo chown -R fsf: /data/fsf**
12. **sudo chmod -R 0755 /data/fsf**
---
### Going to the FSF Client Config Now
---
13. **sudo vi /opt/fsf/fsf-client/conf/config.py**
14. **No changes that are needed to be made**
---
### Going to be Making the FSF Daemon
---
15. **sudo vi /etc/systemd/system/fsf.service**
16. **Enter what is below:**
```
[Unit]  
Description=File Scanning Framework (FSF-Server) Service  
After=network.target  

  [Service]  
  Type=forking  
  User=fsf  
  Group=fsf  
  WorkingDirectory=/  
  PIDFile=/run/fsf/fsf.pid  
 PermissionsStartOnly=true  
 ExecStartPre=/bin/mkdir -p /run/fsf  
 ExecStartPre=/bin/chown -R fsf:fsf /run/fsf  
 ExecStart=/opt/fsf/fsf-server/main.py start  
 ExecStop=/opt/fsf/fsf-server/main.py stop  
 ExecReload=/opt/fsf/fsf-server/main.py restart  

 [Install]  
 WantedBy=multi-user.target  
 ```
17. **sudo systemctl daemon-reload**
18. **sudo systemctl start fsf**
19. **^start^status**
---
### Make a Directory for Our Extracted Files
---
20. **touch test**
21. **vi test**
22. **/opt/fsf/fsf-client/fsf_client.py --full test**
23. **cd /data/fsf/logs**
24. **vi rockout.log**
25. **vi scan_log.log**
26. **sudo mkdir /data/zeek/extracted_files**
27. **sudo chown -R zeek: /data/zeek/extracted_files**
28. **sudo chmod 0755 /data/zeek/extracted_files**
---
### Now We Make A Script For Zeek to Pull Said Files From FSF
---
29. **sudo vi /usr/share/zeek/site/scripts/extract_files.zeek**
30. **Enter below:**
```
@load /usr/share/zeek/policy/frameworks/files/extract-all-files.zeek  
redef FileExtract::prefix = "/data/zeek/extracted_files/";  
redef FileExtract::default_limit = 1048576000;  
```
31. **sudo vi /usr/share/zeek/site/local.zeek**
32. **add after the @load kafka this: @load ./scripts/extract_files.zeek**
33. **sudo systemctl restart zeek**
34. **sudo curl -L -O 192.168.2.20:8080/putty.exe**
35. **ls -l /data/zeek/extracted_files/**
36. **cd /data/zeek/extracted_files/**
37. **file extract-391824y812837918273812blahblahblahblah**
38. **cd /usr/share/zeek/site/scripts**
39. **sudo curl -L -O 192.168.2.20:8080/zeek_scripts/fsf.zeek**
40. **vi fsf.zeek**
41. Key in on a specific event that occurs in zeek during anlysis.
42. If that file is extracted, then I want to set the variables and run that scan command.
43. **CHANGE local file_path to : "/data/zeek/extracted_files/";**
44. **sudo vi /usr/share/zeek/site/local.zeek**
45. **After @load extract_files, enter this :**
46. **@load ./scripts/fsf.zeek**   
47. **sudo systemctl restart zeek**
48. **sudo systemctl status zeek**
49. **sudo curl -L -O 192.168.2.20:8080/putty.exe**
50. **cat /data/fsf/logs/rockout.log | wc -l**
51. 
