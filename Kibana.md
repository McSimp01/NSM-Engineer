# Kibana
---
1. **sudo yum install kibana-7.8.1**
2. **sudo vi /etc/kibana/kibana.yml**
3. **:set nu**
4. **line 7 update server host to sensor ip**
5. **uncomment elastic search host on line 28**
6. **:wq**
7. **sudo firewall-cmd --add-port=5601/tcp --permanent**
8. **sudo firewall-cmd --reload**
9. **ss -lnt**
10. **go to 172.16.30.100:5601 t0 get t0 get to kibana**
11. **go to home directory**
12. **curl -LO http://192.168.2.20:8080/ecskibana.tar.gz**
13. **tar -zxvf ecskibana.tar.gz**
14. **cd ecskibana**
15. **./import-index-templates.sh**
16. **exit till youre admin**
17. **cd /data**
18. **Go back to kibana and to DEV tools**
19. remove and just type **`GET _cat/templates?v`**
20.  we are just making sure our script carried over
21. Try `GET _Cat/templates/ecs*`
22. Saying that if the indexes being made match these names, then use these mappings
23. Honestly sounds like indices creation
24. `GET _template/default`
25. `GET _template/templates?v&s=name`
26. `GET _template/ecs_mapping`
27. `Get _template/ecs_zeek`
28. Stack MGMT
29. Index PAtterns
30.
