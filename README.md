# How set up nginx REMP Server
##### Step 1
```aidl
First of all please don't install nginx 
if already have
Fully remove nginx
```

##### Step 2
Clone nginx-rtmp-module
```text
$ git clone https://github.com/sergey-dryabzhinsky/nginx-rtmp-module.git
``` 
##### Step 3
Install nginx dependencies
```text
$ sudo apt-get install build-essential libpcre3 libpcre3-dev libssl-dev
``` 
##### Step 4
Download nginx-1.17.0 [it is appropriate version because it have not any bug] <br>
Download site: http://nginx.org/download/
```text
$ wget http://nginx.org/download/nginx-1.17.0.tar.gz
$ tar -xf nginx-1.17.0.tar.gz
$ cd nginx-1.17.0
``` 
##### Step 5
Compile nginx
```text
$ ./configure --with-http_ssl_module --add-module=../nginx-rtmp-module
$ make
$ sudo make install
```

##### Step 6
nginx configuration <br>
Nginx install location: /usr/local/nginx <br>
Edit /usr/local/nginx/conf/nginx.conf
```text
worker_processes  1;

events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream; 
    server {
        listen       3007;
        server_name  localhost;
        location / {
            root   html;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }
}

rtmp {
    server {
        listen 127.0.0.1:1935; 
        chunk_size 4000;
        application show {
            live on;
            hls on;
            hls_path /mnt/d/video/;
            hls_fragment 3;
            hls_playlist_length 60;
            deny play all;
        }
    }
}
```
##### Step 7
Test the configuration file
```
$ /usr/local/nginx/sbin/nginx -t
```
Start nginx in the background
```
$ /usr/local/nginx/sbin/nginx
```
Start nginx in the foreground
```text
$ /usr/local/nginx/sbin/nginx -g 'daemon off;'
```
Reload the config on the go
```
$ /usr/local/nginx/sbin/nginx -t && nginx -s reload
```
Kill nginx
```
$ /usr/local/nginx/sbin/nginx -s stop  
```
##### Step 8
Your rtmp url
```
 rtmp://127.0.0.1:1935/show/
```
### OBS Client Configuration 
[![Watch the video](https://raw.githubusercontent.com/vikramhalder/nginx-rtmp/master/Screenshot%20(9).png)](https://www.youtube.com/watch?v=40YmA1kEPWY)
