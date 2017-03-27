# yii nginx config
```php

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location /backend {
        index /web/index.php;
        alias $project_root/backend/web;
        try_files $uri $uri/ /web/index.php?$args;

        location ~ ^/backend/.*\.php$ {
            rewrite ^/backend/(.*)$ /web/$1;
            fastcgi_pass  phpbackend;
            fastcgi_index index.php;
            include fastcgi.conf;
        }
    }

    location /api {
        index /web/index.php;
        alias $project_root/api/web;
        try_files $uri $uri/ /web/index.php?$args;

        location ~ ^/api/.*\.php$ {
            rewrite ^/api/(.*)$ /web/$1;
            fastcgi_pass  phpbackend;
            fastcgi_index index.php;
            include fastcgi.conf;
        }
    }

    location ~ ^/web/.*\.php$ {
        internal;
        root $project_root/backend;
        fastcgi_pass  phpbackend;
        fastcgi_index index.php;
        include fastcgi.conf;
    }

    location ~* \.php$ {
        try_files $uri =404;
        fastcgi_pass  phpbackend;
        fastcgi_index index.php;
        include fastcgi.conf;
    }

```
