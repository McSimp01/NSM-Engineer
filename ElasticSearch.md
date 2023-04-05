# ElasticSearch
---

1. sudo yum install elasticsearch-7.8.1
2. sudo chown elasticsearch: /data/elasticsearch/
3. cd /data/
4. ll
5. sudo -s
6. sudo cd /etc/elasticsearch
7. sudo vi /etc/elasticsearch/elasticsearch.yml
8. set nu
9. line 17 cluster name : and uncomment
10. line 23 node name, uncomment
11. line 33, /data/elasticsearch
12. line 43, uncomment
13. line 55, change network address to _local:ipv4_
14. line 59, uncomment, leave default port
15. line 68, uncomment and change to your sensor ip .100
16. line 72, uncomment, sensor ip .100 where you make master nodes
17. :wq
18. create a service directory on on our systemd to prevent esearch from being swapped out in memory
19. memory swapping will create performance issues
20. sudo mkdir -p /usr/lib/systemd/elasticsearch.service.d
21. sudo vi /usr/lib/systemd/system/elasticsearch.service.d/override.conf
22. Enter this:
[Service]
LimitMEMLOCK=infinity
23. sudo chmod 755 /usr/lib/systemd/system/elasticsearch.service.d
24. sudo chmod 644 /usr/lib/systemd/system/elasticsearch.service.d/override.conf
25. sudo systemctl daemon-reload
26. sudo systemctl start elasticsearch
27. ^start^status
28. ^status^enabled
Specifying how much RAM
29. sudo vi /etc/elasticsearch/jvm.options
30. set nu
31. line 11,10 change it to 4 in the xms thingy
32. :wq
33. sudo systemctl restart elasticsearch
34. sudo systemctl status elasticsearch
35. ss lnt
36. firewall --add-port={9200,9300}/tcp --permanent
37. firewall-cmd --reload
38. curl localhost:9200
39. `curl localhost:9200/_cat/nodes     `
40. `curl localhost:9200/_cat/nodes/?v `
VI INTO /ETC/ELASTICSEARCH/ELASTICSEARCH.YML
41. comment out discovery.seed.
42. comment out initial master nodes
43. add a line after line 68 `discovery.type : single node`
44. retry the curl commands above
45. restart elasticsearch
46. doneski
The initial master node wants a string and not an IP so make it match the name you picked for the node. That was the issue we were having. 
