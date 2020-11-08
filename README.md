# leichttraktor-docker
leichttraktor-docker is php container.

https://ja.wikipedia.org/wiki/ライヒトトラクトーア

# Overall view
```
.
├── leichttraktor   <- https://github.com/kurohige113/leichttraktor.git
│   └── ...
└── leichttraktor-docker   <- here
    ├── README.md
    ├── docker-compose.yml
    └── php
        ├── Dockerfile
        └── php.ini
```

# How to start

1. start container
```
docker-compose up -d
```

2. change root dir (apache2.conf)
```
docker exec -it web bash
vim /etc/apache2/apache2.conf

<Directory /var/www/leichttraktor>
	Options Indexes FollowSymLinks
	AllowOverride None
	Require all granted
</Directory>
```

3. change virtual host (000-default.conf)
```
docker exec -it web bash
vim /etc/apache2/sites-available/000-default.conf

<VirtualHost *:80>
	DocumentRoot /var/www/leichttraktor
</VirtualHost>
```

4. make html
```
docker exec -it web bash
touch /var/www/leichttraktor/index.html
```
or clone (https://github.com/kurohige113/leichttraktor.git)

5. browse localhost

you can see the website.

6. close container

```
docker-compose down
```
