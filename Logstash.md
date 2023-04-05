# Logstash
---
1. sudo yum install logstash-7.8.1
2. cd /etc/logstassh/
3. ll
4. sudo curl -LO http://192.168.2.20:8080/logstash.tar
5. sudo tar -xf logstash.tar
6. cd conf.d
7. sudo vi logstash-100-input-kafka-zeek.conf
8. change bootstrap_servers to 172.16.30.100
9. :wq
10. sudo vi logstash-100-input-kafka-suricata.conf
11. change line 8 to sensor ip
12. sudo vi logstash-100-input-kafka-fsf.conf
13. change line 8 to sensor ip
14. sudo vi logstash-9999-output-elasticsearch.conf
15. change all 127.0.0.1 entries to sensor ip
16. can do the  `:%s/127.0.p0.1/172.16.1.100`
17. comment out line 2
18. :wq
19. sudo systemctl start logstash
20. enable
21. status
---
22. had to change the port is binding too sooo
go to /etc/elasticsearch/elasticsearch.yml and edit
###### **network.host: _eno1_**
---
#### BECAUSE WE CHANGED THIS WE GOTTA CHANGE THAT    
##### **like 28 in /etc/kibana/kibana.yml to ["https:172.16.30.100:9200"]**
---
FSF CORRECTION
---
cd /etc/kibana   
ll   
sudo vi /opt/fsf/fsf-client/conf/config.py  
:set nu  
line 9, put the sensor IP     
sudo vi /opt/fsf/fsf-server/conf/config.py      
change line 18 to sensor IP
---
# MAKING INDICES ON KIBAUHNUR
---
Stakc MGMT
Type what u want
Timestamp babay booey
Create
Mapping error? punch your computer aint got time for that 
