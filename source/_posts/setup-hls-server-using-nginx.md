---

title: Setup HLS Server Using Nginx

date: 2018-03-02 14:00:57

tags: [HLS, nginx]

categories: Multimedia

---

为了学习HLS，首先在Ubuntu 14.04 x86_64平台上用Nginx搭一个HLS Streaming Server玩一玩，具体步骤如下：

### Install Nginx with RTMP support

* 下载Nginx及nginx-rtmp-module

``` bash
$ mkdir ~/hls
$ cd ~/hls
$ git clone https://github.com/arut/nginx-rtmp-module
# Download related version of nginx
$ wget http://nginx.org/download/nginx-1.12.2.tar.gz
```

* 编译安装Nginx

``` bash
# Install nginx dependencies
$ sudo apt-get install build-essential libpcre3 libpcre3-dev libssl-dev
$ tar xf nginx-1.12.2.tar.gz
$ cd nginx-1.12.2
$ ./configure --with-http_ssl_module --with-http_stub_status_module \
--add-module=../nginx-rtmp-module
$ make -j4
$ sudo make install
```

* 安装Nginx init脚本

``` bash
$ git clone https://github.com/JasonGiedymin/nginx-init-ubuntu.git
$ sudo cp nginx-init-ubuntu/nginx /etc/init.d/nginx
$ sudo chmod +x /etc/init.d/nginx
$ sudo update-rc.d nginx defaults
$ sudo service nginx start
```

### Config Nginx with RTMP

根据[nginx-rtmp-module的Wiki](https://github.com/arut/nginx-rtmp-module/wiki/Getting-started-with-nginx-rtmp)配置RTMP，修改nginx.conf如下：

```
--- /usr/local/nginx/conf/nginx.conf.default	2018-03-01 10:38:34.726129303 +0800
+++ /usr/local/nginx/conf/nginx.conf	2018-03-05 12:20:56.532758031 +0800
@@ -1,6 +1,6 @@

 #user  nobody;
-worker_processes  1;
+worker_processes  auto;

 #error_log  logs/error.log;
 #error_log  logs/error.log  notice;
@@ -92,6 +92,39 @@
     #    }
     #}

+    server {
+        listen 8000;
+        server_name localhost;
+
+        # rtmp stat
+        location /stat {
+            rtmp_stat all;
+            rtmp_stat_stylesheet stat.xsl;
+        }
+
+        location /stat.xsl {
+            # you can move stat.xsl to a different location
+            root /tmp/hls;
+        }
+
+        # rtmp control
+        location /control {
+            rtmp_control all;
+        }
+
+        location /hls {
+            types {
+                text/html html htm;
+                application/dash+xml mpd;
+                application/x-mpegurl m3u8;
+                video/mp2t ts;
+            }
+
+            root /tmp;
+            index index.html;
+            expires -1;
+        }
+    }

     # HTTPS server
     #
@@ -115,3 +148,21 @@
     #}

 }
+
+rtmp {
+    server {
+            listen 1935;
+            chunk_size 4096;
+
+            application hls {
+                live on;
+                # Turn on HLS
+                hls on;
+                hls_path /tmp/hls;
+                hls_fragment 5;
+                # hls_playlist_length 60;
+                # Disable consuming the stream from nginx as rtmp
+                # deny play all;
+            }
+    }
+}
```

写到这里，插一个题外话，vim打开nginx.conf，需要拷贝nginx.conf.diff的内容到Markdown编辑器atom，这里就涉及到vim如何与系统剪贴板交互，搜索一番后结果如下：

``` bash
# Check if vim installed supports system clipboard
$ vim --version | grep clipboard
# If displayed as "-clipboard" then install vim-gtk
$ sudo apt-get install vim-gtk
# "+y for yank to the clipboard register
# "+p for paste from the clipboard register
# :help clipboard for more information
```

修改完成后，执行命令`sudo service nginx reload`重新加载nginx.conf文件。

### Push live stream to Nginx

Nginx将RTMP stream作为输入，对于HLS来说，video stream格式一般是x264，audio stream格式一般是AAC。RTMP stream通过ffmpeg来提供。

* 安装ffmpeg

```
# Reinstall ffmpeg from another PPA
$ sudo apt-get --purge remove ffmpeg
$ sudo apt-get --purge autoremove
$ sudo apt-get install ppa-purge
$ sudo ppa-purge ppa:kirillshkrogalev/ffmpeg-next

# -E enable root using common user's env setting
$ sudo -E add-apt-repository ppa:mc3man/trusty-media
$ sudo apt-get update
$ sudo apt-get dist-upgrade
$ sudo apt-get install ffmpeg
```

* ffmpeg提供RTMP stream

`$ ffmpeg -re -i big_buck_bunny_720p_h264.mov -c copy -f flv rtmp://localhost/hls/bbb`

RTMP的URI格式为：`rtmp://nginx_host:[:nginx_port]/app_name/stream_name`，按照nginx.conf的配置，这里可以通过vlc(vlc --no-overlay --no-video-on-top)或ffplay来播放测试的HLS stream，URI可以是RTMP或者HTTP格式：

`rtmp://localhost/hls/bbb`

`http://localhost:8000/hls/bbb.m3u8`

另外，通过`http://localhost:8000/stat`可以查看RTMP stream的相关信息，需要拷贝nginx-rtmp-module目录下的stat.xsl到/tmp/hls目录下。

### Reference links

* [Peer5 docs](https://docs.peer5.com/guides/setting-up-hls-live-streaming-server-using-nginx/)
* [Vultr docs](https://www.vultr.com/docs/setup-nginx-on-ubuntu-to-stream-live-hls-video)
