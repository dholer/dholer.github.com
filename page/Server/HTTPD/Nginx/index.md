---
layout: page 
title: Nginx
---

###Window nginx

[docs](http://nginx.org/en/download.html)

[nginx for Windows](http://www.nginx.org/en/docs/windows.html)



 Version of nginx for Windows uses the native Win32 API (not the Cygwin emulation layer). Only the select() request processing method is currently used, so high performance and scalability should not be expected. Due to this and some other known issues version of nginx for Windows is considered to be a beta version. At this time, it provides almost the same functionality as a UNIX version of nginx except for XSLT filter, image filter, GeoIP module, and embedded Perl language.

To install nginx/Windows, download the latest development version distribution (1.3.0), since the development branch of nginx contains all known fixes. Then unpack the distribution, go to the nginx-1.3.0 directory, and run nginx. Here is an example for the drive C: root directory:

	cd c:\
	unzip nginx-1.3.0.zip
	cd nginx-1.3.0
	start nginx
	
Run the tasklist command-line utility to see nginx processes:

	C:\nginx-1.3.0>tasklist /fi "imagename eq nginx.exe"
	
	Image Name           PID Session Name     Session#    Mem Usage
	=============== ======== ============== ========== ============
	nginx.exe            652 Console                 0      2 780 K
	nginx.exe           1332 Console                 0      3 112 K
	
One of the processes is the master process and another is the worker process. If nginx does not start, look for the reason in the error log file logs\error.log. If the log file has not been created, the reason for this should be reported in the Windows Event Log. If an error page is displayed instead of the expected page, also look for the reason in the logs\error.log file.

nginx/Windows uses the directory where it has been run as the prefix for relative paths in the configuration. In the example above, the prefix is C:\nginx-1.3.0\. Paths in a configuration file must be specified in UNIX-style using forward slashes:

	access_log   logs/site.log;
	root         C:/web/html;
	
nginx/Windows runs as a standard console application (not a service), and it can be managed using the following commands:

	nginx -s stop	fast shutdown
	nginx -s quit	graceful shutdown
	nginx -s reload	 changing configuration, starting new worker processes with a new configuration, graceful shutdown of old worker processes
	nginx -s reopen	re-opening log files
	
####Known issues

Although several workers can be started, only one of them actually does any work.
A worker can handle no more than 1024 simultaneous connections.
The cache and other modules which require shared memory support do not work on Windows Vista and later versions due to address space layout randomization being enabled in these Windows versions.
Possible future enhancements

####Running as a service.
Using the I/O completion ports as a request processing method.
Using multiple worker threads inside a single worker process. 

--
###资料
- [Nginx 简单的负载均衡配置示例[原创]](http://blog.s135.com/post/306/)




###configure file

	#user  nobody;
	worker_processes  1;
	
	#error_log  logs/error.log;
	#error_log  logs/error.log  notice;
	#error_log  logs/error.log  info;
	
	#pid        logs/nginx.pid;
	
	
	events {
	    worker_connections  1024;
	}
	
	
	http {
	    include       mime.types;
	    default_type  application/octet-stream;
	
	    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
	    #                  '$status $body_bytes_sent "$http_referer" '
	    #                  '"$http_user_agent" "$http_x_forwarded_for"';
	
	    #access_log  logs/access.log  main;
	
	    sendfile        on;
	    #tcp_nopush     on;
	
	    #keepalive_timeout  0;
	    keepalive_timeout  65;
	
	    gzip  on;
		server {
	        listen       80;
	        server_name  localhost;
			index index.html index.htm index.php;
			root  D:/Dholer/home/htdocs;
			
			location / {		
				try_files $uri @apache;
			}
			
			location @apache {
				internal;
				proxy_pass http://127.0.0.1:89;
				include proxy.conf;
			}
			
			
			
			location ~ .*\.(php|php5)?$
			{
				proxy_pass http://127.0.0.1:89;
				include proxy.conf;
			}
			
			location /status {
				stub_status on;
				access_log   off;
			}
			
			#location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
			#{
			#	expires      30d;
			#}
			#location ~ .*\.(js|css)?$
			#{
			#	expires      12h;
			#}		
			#access_log  /home/wwwlogs/access.log  access;
			
		}
		
		server {
	        listen       80;
	        server_name  www.iis.org;
			index index.html index.htm index.asp;
			root  D:/Dholer/home/htdocs;
			
			location / {
				try_files $uri @apache;
			}
			
			location @apache {
				internal;
				proxy_pass http://127.0.0.1:88;
				include proxy.conf;
			}
			
			location ~ .*\.(aspx|asp|php|php5)?$
			{
				proxy_pass http://127.0.0.1:88;
				include proxy.conf;
			}
			
			#location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
			#{
			#	expires      30d;
			#}
			#location ~ .*\.(js|css)?$
			#{
			#	expires      12h;
			#}		
			#access_log  /home/wwwlogs/access.log  access;		
		}	
		
	    server {
	        listen       80;
	        server_name  localhost1;
	
	        charset utf-8;
	
	        #access_log  logs/host.access.log  main;
	
	        location / {
	            root   html;
	            index  index.html index.htm;
	        }
	
	        #error_page  404              /404.html;
	
	        # redirect server error pages to the static page /50x.html
	        #
	        error_page   500 502 503 504  /50x.html;
	        location = /50x.html {
	            root   html;
	        }
	
	        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
	        #
	        #location ~ \.php$ {
	        #    proxy_pass   http://127.0.0.1;
	        #}
	
	        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
	        #
	        #location ~ \.php$ {
	        #    root           html;
	        #    fastcgi_pass   127.0.0.1:9000;
	        #    fastcgi_index  index.php;
	        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
	        #    include        fastcgi_params;
	        #}
	
	        # deny access to .htaccess files, if Apache's document root
	        # concurs with nginx's one
	        #
	        #location ~ /\.ht {
	        #    deny  all;
	        #}
	    }
	
	
	    # another virtual host using mix of IP-, name-, and port-based configuration
	    #
	    #server {
	    #    listen       8000;
	    #    listen       somename:8080;
	    #    server_name  somename  alias  another.alias;
	
	    #    location / {
	    #        root   html;
	    #        index  index.html index.htm;
	    #    }
	    #}
	
	
	    # HTTPS server
	    #
	    #server {
	    #    listen       443;
	    #    server_name  localhost;
	
	    #    ssl                  on;
	    #    ssl_certificate      cert.pem;
	    #    ssl_certificate_key  cert.key;
	
	    #    ssl_session_timeout  5m;
	
	    #    ssl_protocols  SSLv2 SSLv3 TLSv1;
	    #    ssl_ciphers  HIGH:!aNULL:!MD5;
	    #    ssl_prefer_server_ciphers   on;
	
	    #    location / {
	    #        root   html;
	    #        index  index.html index.htm;
	    #    }
	    #}
	
	}



