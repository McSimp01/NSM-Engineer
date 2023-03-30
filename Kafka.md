# Kafka
---
-Zookeeper is its dependencies so we are going to be installing that as well
1. **sudo yum install kafka zookeeper**
---
### Configure Zookeper
---
2. **sudo vi /etc/zookeeper/zoo.cfg**
3. No changes to be made, just browse
4. **sudo systemctl start zookeeper**
5. **^start^status**
6. **^status^enable**
---
### Take a Look at Kafka
---
7. **sudo vi /etc/kafka/server.properties**
8. **Line 31, uncomment "listeners = PLAINTEXT:/your.host.name:9092"**
9. **Replace the your.host.name with your sensor IP so 172.16.30.100:9092**
10. **Line 36, uncomment "advertised.listeners=PLAINTEXT... and have the IP as 172.16.30.100:9092"**
11. **Line 60, log.dirs=/data/kafka**
12. **Line 65 change the num.partitions= to 3**
13. **Line 107, uncomment and change log.retention.byte= to 90000000000**
14. **Line 123, change to "zookeeper.connect=127.0.0.1:2181"**
15. **wq!**
16. **sudo chown -R kafka: /data/kafka**
17. **ll**
---
### Implementations to our Firewall
---
18. **sudo firewall-cmd --add-port=9092/tcp --permanent**
19. **sudo firewall-cmd --add-port=2181/tcp --permanent**
20. **sudo firewall-cmd --reload**
---
### Now We Try To Start Kafka
---
21. **sudo systemctl start kafka**
22. **^start^status**
23. **cd /data/kafka**
24. **ll**
25. **ss -lnt** Another troubleshooting tidbit
---
### Add A Script to Zeek so it can talk to Kafka
---
26. **cd /usr/share/zeek/site/scripts**
27. **sudo curl -L -O 192.168.2.20:8080/zeek_scripts/kafka.zeek**
28. **ll**
29. **sudo vi kafka.zeek**
30. **Change LINE 7 sensor-ip:9092 CHANGE IT TO : "172.16.30.100:9092"** Take out the < > home boy
31. **cd ..**
32. **vi local.zeek (in site directory)**
33. **add after @load "@load ./scripts/kafka.zeek"**
34. **wq!**
35. **sudo systemctl restart zeek**
36. **^restart^start**
37. **^start^status**
#### Validating Zeek Raw
39. **sudo /usr/share/kafka/bin/kafka-topics.sh --bootstrap-server 172.16.30.100:9092 --list**
40. **sudo /usr/share/kafka/bin/kafka-topics.sh --bootstrap-server 172.16.1.100:9092 --describe --topic zeek-raw**
---
### Make sure our Logs are getting Streamed Over to Topic
---
41. **sudo /usr/share/kafka/bin/kafka-console-consumer.sh --bootstrap-server 172.16.1.100:9092 --topic zeek-raw**
42.   
