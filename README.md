# cruiser-docker
cruiser-docker is php container.

https://ja.wikipedia.org/wiki/巡航戦車_Mk.I

# Overall view
```
.
├── cruiser   <- https://github.com/kurohige113/cruiser.git
│   └── ...
└── cruiser-docker   <- here
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

<Directory /var/www/cruiser>
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
	DocumentRoot /var/www/cruiser
</VirtualHost>
```

4. make html
```
docker exec -it web bash
touch /var/www/cruiser/index.html
```
or clone (https://github.com/kurohige113/cruiser.git)

5. browse localhost

you can see the website.

6. close container

```
docker-compose down
```
