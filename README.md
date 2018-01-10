# django-uwsgi-nginx

## 部署用uwsgi+nginx部署django  django+uwsgi+nginx搭建服务器

### 1、安装django python 等环境   能够用Python/python3 manage.py runserver 运行该项目
### 2、安装uwsgi   pip/pip3 install uwsgi
#### 测试uwsgi是否安装成功   test.py
```python
def application(env, start_response):
          start_response('200 OK', [('Content-Type','text/html')])
          return [b"Hello World"] # python3
          #return ["Hello World"] # python2
```
```
uwsgi --http :8000 --wsgi-file test.py
```
输入该命令，如果端口被占用  就杀死该进程 或者使用 --http-socket。 打开浏览器输入127.0.0.1：8000 显示helloworld则安装成功

### 3、在django项目下新建 uwsgi.ini文件 
#### http 为指定运行该项目端口
#### socket 为指定socket端口
#### chdir为该项目根目录
#### module为该项目的wsgi文件
#### processes为 maximum number of worker processes   (可修改)
#### threads 为 thread numbers startched in each worker process
#### vaccum=true 为 退出的时候清空环境
#### buffer-size 默认为4096    如果单独使用uwsgi的时候不需要这项  如果使用nginx作为反向代理的时候 可能因为文件过大报：1 rev（）...错时加上这句
```
uwsgi --ini uwsgi.ini
```
输入该命令 打开浏览器输入127.0.0.1:9000,能够打开该项目，不过此时并没有静态文件 只有单纯的html  使用nginx做反向代理

### 4.安装配置nginx
#### 安装nginx命令   
```
sudo apt-get install nginx
```
#### 配置nginx 
常用的路径为  /etc/nginx/nginx.conf 或者 /etc/nginx/sites-enabled/default等  也可以通过自己建立 XXX.conf文件 用命令
``` 
sudo ln -s XXX.conf （ngin配置文件路径）
```
将配置文件挂载到配置文件上;  此次修改的时sites-enabled下面的 default文件：
#### 配置的 ip 地址访问 不是域名
#### listen 8000 端口 本身是80 害怕被占用改成8000;
#### server name  为服务器 ip地址  如果是公司内网网段  可以在路由器的 虚拟服务器中  将web服务器的ip端口等映射出去 
#### uwsgi_buffers和uwsgi_buffer_size 比较重要 如果配置完 运行nginx报502网关错误 查看log时有  上游头部文件过大时错误  就加上这个
#### access_log和error_log为错误日志路径  出错了查看
#### location /中 include 为 uwsgi_params路径  一般nginx会自带uwsgi_params 没有到github下载   uwsgi_pass为在uwsgi_ini中的socket端口地址，不能出错
#### location /media 为项目 media路径    location /static 为项目 static路径
#### 常用nginx命令
```
sudo service nginx start/stop/restart/reload
```
先用 uwsgi.ini启动项目 再开启nginx  通过server name的ip地址就可以访问项目了




      

