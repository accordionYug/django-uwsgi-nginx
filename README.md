# django-uwsgi-nginx

## 部署用uwsgi+nginx部署django  django+uwsgi+nginx搭建服务器

### 1、安装django python 等环境   能够用Python/python3 manage.py runserver 运行该项目
### 2、安装uwsgi   pip/pip3 install uwsgi
####测试uwsgi是否安装成功   test.py
''' python
def application(env, start_response):
          start_response('200 OK', [('Content-Type','text/html')])
          return [b"Hello World"] # python3
          #return ["Hello World"] # python2
'''
      uwsgi --http :8000 --wsgi-file test.py   如果端口被占用  就杀死该进程 或者使用 --http-socket
      打开 127.0.0.1：8000 显示helloworld则安装成功
      
      
      
      

