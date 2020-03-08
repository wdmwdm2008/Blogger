#### 重启Nginx

service nginx restart  
/etc/init.d/nginx stop  
/etc/init.d/nginx start  

#### Ubuntu Nginx

$sudo service nginx start  
$sudo service nginx stop  


ps -ef | grep nginx

#### 从容停止Nginx：
kill -QUIT 主进程号

#### 快速停止Nginx：
kill -TERM 主进程号

#### 强制停止Nginx：
pkill -9 nginx

#### nginx 重启
nginx -s reload

#### gunicorn 开启
gunicorn --workers 3 --bind unix:myproject.sock --daemon -m 007 --user www-data --worker-class gevent wsgi:app

#### gunicorn 关闭
kill -9 1234

#### gunicorn 重启
kill -HUP 1234

#### 得到外网域名
./ngrok  http 80

## Reference
https://www.jianshu.com/p/f5c271d95e39        // 使用docker搭建nginx，flask，gunicorn运行环境
https://segmentfault.com/a/1190000011827338    //将本地web服务映射到公网访问
