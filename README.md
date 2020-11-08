# cruiser-docker
cruiser-docker is php and node container.
node container introduces typescript.

https://ja.wikipedia.org/wiki/巡航戦車_Mk.I

# Overall view
```
.
├── cruiser   <- https://github.com/kurohige113/cruiser.git
│   └── ...
└── cruiser-docker   <- here
    ├── README.md
    ├── docker-compose.yml
    ├── php
    │   ├── Dockerfile
    │   └── php.ini
    └── node
        └── Dockerfile
	
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

5. make js,ts dir
```
mkdir -p cruiser/js/ts    
mkdir -p cruiser/js/dist  
```

6. init npm
```
docker-compose run --rm node npm init
```

7. edit package.json
```
#cruiser/package.json
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "clean": "rimraf js/dist",                // add               
  "tsc": "tsc",                             // add               
  "build": "npm-run-all clean tsc"          // add
},
```

8. install typescript
```
docker-compose run --rm node npm install -D typescript @types/node
docker-compose run --rm node npm install -D ts-node ts-node-dev rimraf npm-run-all
```

9. init typescript
```
docker-compose run --rm node npx tsc --init
```

10. edit typescript config
```
#cruiser/tsconfig.json
{
  "compilerOptions": {
    "target": "ES2019",                       /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */
    "module": "commonjs",                     /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */
    "sourceMap": true,                        /* Generates corresponding '.map' file. */
    "outDir": "./js/dist",                    /* Redirect output structure to the directory. */
    "strict": true,                           /* Enable all strict type-checking options. */
    "esModuleInterop": true,                  /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */
    "skipLibCheck": true,                     /* Skip type checking of declaration files. */
    "forceConsistentCasingInFileNames": true  /* Disallow inconsistently-cased references to the same file. */
  },
  "include": [
    "js/ts/**/*"                              /* All files under js/ts/(all recursively even if the directory nesting is deep) are to be compiled. */
  ]
}
```

11. make sample code
```
#cruiser/js/ts/greeter.ts

interface Person {
    firstName: string;
    lastName: string;
}

class Student {
    fullName: string;
    constructor(public firstName: string, public middleInitial: string, public lastName: string) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = new Student("Jane", "M.", "User");
document.body.textContent += greeter(user);

```

```
#cruiser/index.html

<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="utf-8">
    <title>Hello World.</title>
</head>
<body>
    <script src="./js/dist/greeter.js"></script>
</body>
</html>
```

12. compiled
```
docker-compose run --rm node npm run build
```

13. browse localhost
you can see the website.

14. close container
```
docker-compose down
```
