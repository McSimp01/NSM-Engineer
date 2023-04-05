# Beats
---
1. **sudo yum install filebeat-7.8.1**
2. **sudo mv /etc/filebeat/filebeat.yml /etc/filebeat/filebeat.yml.bk**
3. **sudo curl -LO 192.168.2.20:8080/filebeat.yml**
3. **sudo vi /etc/filebeat/filebeat.yml**
4. Add changes after line 11
```  
- type: logs
enabled: true
paths:
 - /data/fsf/logs/rockout.logs
 json.keys_under_root: true
 fields:
  kafka_topic: fsf-raw
 fields_under_root: true
 ```
 5. **sudo systemctl start filebeat**
 6. **enable**
 7. **status**
 8. **sudo /usr/share/kafka/bin/kafka-topics.sh --bootstrap-server 172.16.30.100:9092 --list**
 9. **vi /etc/filebeat/filebeat.YML**
 10. **change the hosts under output.kafka to our sensor ip**
 11. **then restart filebeat**
 12. **sudu /usr/share/kafka/bin/kafka-console-consumer.sh --bootstrap-server 172.16.1.100:9092 --topic suricata-raw --from-beginning**
