### Nginx on dynamic ports alongside Laravel Valet (for developing microservices behind a proxy)

Using the config below (replacing USERNAME with our username, the workflow is as follows:
> Assumes you are using laravel and laravel valet or valet linux, and have `valet park`ed your `~/Sites` directory
- create a new laravel app
  ```bash
  cd ~/Sites && laravel new laravel-svc-2002
  ```
- create `$HOME/NginxPorts` if it doesn't already exist
  ```bash
  mkdir -p $HOME/NginxPorts
  ```
- symlink project to folder numbered after desired port under $HOME/NginxPorts
  ```bash
  ln -s $HOME/Sites/laravel-svc-2002 $HOME/NginxPorts/2002
  ```
- (optionally) proxy the service with laravel valet
  ```bash
  valet proxy laravel-svc-2002 http://127.0.0.1:2002
  ```
  
```nginx
server {
	listen 2001;
	listen 2002;
	# listen [::]:80;
	server_name default_server;

    # set $my_root /home/USERNAME/NginxPorts/$server_port; # uncomment to use public directory only when 'laravel' is in the hostname

    # check if $host contains 'laravel'
    # if ($host ~* laravel) { # uncomment to use public directory only when 'laravel' is in the hostname
     
     set $my_root /home/USERNAME/NginxPorts/$server_port/public;
    
    # } # uncomment to use public directory only when 'laravel' is in the hostname

    root $my_root;

	index  index.php index.html index.htm;

	location / {
		try_files $uri $uri/ /index.php?$args;
	}

	location ~ \.php$ {
		fastcgi_split_path_info ^(.+\.php)(/.+)$;
		### fastcgi_pass must use the valet.sock
		### unix:/Users/YOUR_USER_NAME/.config/valet/valet.sock
		fastcgi_pass unix:/home/USERNAME/.valet/valet.sock;
		fastcgi_index index.php;
		fastcgi_intercept_errors on;
		include fastcgi.conf;
		include fastcgi_params;
		fastcgi_param PATH_INFO $fastcgi_path_info;
		fastcgi_buffers 16 16k;
		fastcgi_buffer_size 32k;
	}

	location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
		expires max;
		log_not_found off;
	}

	# error_page  500 502 503 504  /50x.html;
	# location = /50x.html {
	#	root   html;
	# }
}
```

### Trusting proxies

>It is necessary to configure trusted proxies to avoid mixed content errors 
>when using ssl termination behind a reverse proxy

The following `app/Http/Middleware/TrustProxies.php` will trust all proxies:

```php
<?php

namespace App\Http\Middleware;

use Illuminate\Http\Middleware\TrustProxies as Middleware;
use Illuminate\Http\Request;

class TrustProxies extends Middleware
{
    /**
     * The trusted proxies for this application.
     *
     * @var array<int, string>|string|null
     */
    protected $proxies = '*';

    /**
     * The headers that should be used to detect proxies.
     *
     * @var int
     */
    protected $headers =
        Request::HEADER_X_FORWARDED_FOR |
        Request::HEADER_X_FORWARDED_HOST |
        Request::HEADER_X_FORWARDED_PORT |
        Request::HEADER_X_FORWARDED_PROTO |
        Request::HEADER_X_FORWARDED_AWS_ELB;
}
```


