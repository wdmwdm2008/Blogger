### 1. 启动java jar包
sudo nohup java -Dhll.app.id=map-textsearch-svc -Xms6G -Xmx6G -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -Dapollo.configService=http://apollo-stg.myhll.cn:8080 -Dhll.env=stg -Dspring.devtools.restart.enabled=false -Dserver.port=8999 -jar /tmp/新文件夹/map-textsearch-web_20210813_dataAB.jar > /home/data/kickkk.sun/map-textsearch-web_20210813_dataAB.log 2>&1&

### 2. 统计代码行数

git log --since=2021-05-25 --until=2021-06-01 --pretty=tformat: --numstat | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }'


### 3. oss链接
oss.bucketname = ma
oss.acesskey.id = L
oss.acesskey.secret = m
oss.endpoint = oss.aliyuncs.com




