# Zeek Setup
---

1. ssh in
2. **sudo yum install zeek zeek-plugin-kafka zeek-plugin-af_packet**
3. This is just to ensure that all these plugins come down too ontop of zeek
4. **sudo vi /etc/zeek.networks.cfg**
5. Just verifying
6. **sudo vi /etc/zeek/zeekctl.cfg**
7. **:set nu**
8. Line 67, change to **/data/zeek**
9. line 76 [after 75] add **lb.custom.InterfacePrefix=af_packet::**
10. **:wq!**
11. **sudo vi /etc/zeek/node.cfg**
12. **line 8-11, comment them out**
13. **comment lines 16-18 back in**
14. **on line 23 we are adding "pin_cpus=1"** after host=localhost
15. **on line 33 add lb_method=custom** after interface enp5s0 **which we also changed**
16. **on line 34 add "lb_procs=2"** after method
17. **next line add "pin_cpus=2,3"**
18. **next line add env_vArs=fanout_id=77**
19. ```**UNCOMMENT EVERYTHING EXCEPT WORKER 2**```
20. LB METHOD is our load balance method, and we are making it custom by adding these lines
21. Now lets make a directory for our scripts
22. **sudo mkdir /usr/share/zeek/site/scripts**
23. **cd /usr/share/zeek/site/scripts**
24. **sudo curl -L -O 192.168.2.20:8080/zeek_scripts/afpacket.zeek**
25. **ll**
26. If you curl the wrong thing, you have to remove it
27. **sudo rm afpacket.zeek**
28. **vi afpacket.zeek**
29. Now we need to get into our zeek extension -- extension is what adds the meta tags
30. We have to curl it first though
31. **sudo curl -L -O 192.168.2.20:8080/zeek_Scripts/extension.zeek**
32. **vi extension.zeek**
33. **sudo vi /usr/share/zeek/site/local.zeek**
34. **go to the bottom, SHIFT+G i.e. line 103/104**
35. We are basically going to tell zeek about the scripts we uploaded
36. **add "@load ./scripts/afpacket.zeek"**
37. **add "@load ./scripts/extension.zeek"**
38. **:wq**
39. **cd /data/**
40. **ll**
41. **sudo chown -R zeek: /etc/zeek**
42. **sudo chown -R zeek: /data/zeek**
43. **sudo chown -R zeek: /usr/share/zeek**
44. **sudo chown -R zeek: /usr/bin/zeek**
45. **sudo chown -R zeek: /usr/bin/capstats**
46. **sudo chown -R zeek: /var/spool/zeek**
47. Tuning the capabilities and accesses of zeek so we can have an added layer of defense in depth
48. **sudo /sbin/setcap cap_net_raw,cap_net_admin=eip /usr/bin/zeek**
49. **sudo /sbin/setcap cap_net_raw,cap_net_admin=eip /usr/bin/capstats**
50. **sudo getcap /usr/bin/zeek**
51. **sudo getcap /usr/bin/capstats**
52. Creating the daemon for the zeek service  
53. **sudo vi /etc/systemd/system/zeek.service**
54. **Type below in this location**

```
[Unit]    
Description=Zeek Network Analysis Engine  
After=network.target  

[Service]  
Type=forking  
User=zeek  
ExecStart=/usr/bin/zeekctl deploy  
ExecStop=/usr/bin/zeekctl stop  

[Install]  
WantedBy=multi-user.target  
```   
55. **wq!**
56. reload the daemon
57. **sudo systemctl daemon-reload**
58. **sudo systemctl start zeek**
59. Validate and look at some logs
60. **sudo ls -l /data/zeek/current/**
61. **sudo systemctl enable zeek**
62. 
