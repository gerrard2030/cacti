# cacti
# cacti
Ken/cacti
image/cacti from docker smcline06/cacti   //https://hub.docker.com/r/smcline06/cacti

image/mysql from docker percona:5.7.14

├── cacti
│   ├── backups
│   ├── cacti_env
│   ├── logs
│   ├── plugins
│   │   └── README.md
│   └── rra
├── mysql
│   ├── log
│   └── my.cnf
└── README.md

# 确保先Run mysql, 再Run cacti，因为cacti会自动进入数据库建库
# cacti的环境变量中确保mysql的IP正确
# for mysql > = 8.0 
  --default-authentication-plugin=mysql_native_password
  
  
docker run -itd -p 3306:3306  \
--restart always  \
--privileged=true   \
--name cacti_mysql   \
-e MYSQL_ROOT_PASSWORD=3nvisi0n!   \
-v /root/cacti/mysql/my.cnf:/etc/mysql/my.cnf  \
-v /root/cacti/mysql/data:/var/lib/mysql \
-v /root/cacti/mysql/log:/logs \
percona:5.7.14   \
&& docker logs cacti_mysql -f



docker run -it -p 8080:80 -p 443:443 -d \
--restart always  \
--privileged=true \
--name cacti \
--env-file=/root/cacti/cacti/cacti_env \
-v /root/cacti/cacti/rra:/cacti/rra/  \
-v /root/cacti/cacti/plugins:/cacti/plugins/  \
-v /root/cacti/cacti/logs:/cacti/log/  \
-v /root/cacti/cacti//backups:/backups/  \
smcline06/cacti  \
&& docker logs cacti -f

