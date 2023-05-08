# DUCUMENTATION OF PROJECT2

## STEP 1 – INSTALLING THE NGINX WEB SERVER
`sudo apt update`

`sudo apt install nginx`

`sudo systemctl status nginx`

![NGINX Status](./images/NGINX STATUS1.MPG)

`curl http://localhost:80`

![NGINX Status](./images/NGINX STATUS2.MPG)

[Nginx Site Page](https://13.50.99.254:80)

![NGINX Status](./images/NGINX SITE WELLCOME PAGE.MPG)


## STEP 2 — INSTALLING MYSQL
`sudo apt install mysql-server`

`sudo mysql`

![MYSQL Status](./images/MYSQL_MONITOR.MPG)

`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`

`mysql> Exit`

`sudo mysql_secure_installation`

`sudo mysql -p`

`mysql> exit`

## STEP 3 – INSTALLING PHP-FPM AND PHP-MYSQL
`sudo apt install php-fpm php-mysql`


## STEP 4 — CONFIGURING NGINX TO USE PHP PROCESSOR
`sudo mkdir /var/www/projectLEMP`

`sudo chown -R $USER:$USER /var/www/projectLEMP`

`sudo nano /etc/nginx/sites-available/projectLEMP`

```
{
server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

      index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
```

`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

`sudo nginx –t`

![NGINX_CONFIG Status](./images/NGINX_CONFIGURATION__TEST.MPG)

`sudo unlink /etc/nginx/sites-enabled/default`

`sudo systemctl reload nginx`

`sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html`

[Nginx](https://13.50.99.254:80)

![NGINX Status](./images/NGINX_SITE_VIEW.MPG)

## STEP 5 — TESTING PHP WITH NGINX

`sudo nano /var/www/projectLEMP/info.php`


```
{

<?php phpinfo(); ?>

}

```

[PHP](http://13.50.99.254:80/info.php

![PHP Status](./images/ PHP_WITH_NGINX_OUTPUT.MPG)

`sudo rm /var/www/your_domain/info.php`

## STEP 6 – RETRIEVING DATA FROM MYSQL DATABASE WITH PHP (CONTINUED)
`sudo mysql –p`

`mysql> CREATE DATABASE `example_database`;`

`mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`

`mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';`

`mysql -u example_user –p`

`mysql> exit`

`mysql> SHOW DATABASES;`

![MYSQL](./images/USER_DATABASE_SCREEN.MPG)

‘ mysql> CREATE TABLE example_database.todo_list (
 mysql>     item_id INT AUTO_INCREMENT,
 mysql>     content VARCHAR(255),
 mysql>     PRIMARY KEY(item_id)
 mysql> );’

‘mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first  important item");’

‘mysql>  SELECT * FROM example_database.todo_list;’

![MYSQL](./images/TODO_LIST_TABLE.MPG)
‘mysql> exit’
‘nano /var/www/projectLEMP/todo_list.php’

```
{
<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}

}
``

[Todo_List Table](http://13.50.99.254:80/todo_list.php

![Apache Status](./images/website_output.MPG)
