# Web-NCAE
Web Master for NCAE

## Apache

### Installing Apache  
```
sudo apt update
sudo apt install apache2(y and enter)  
sudo systemctl start apache2  
sudo systemctl enable apache2
```

### Connecting Apache to MySQL using PHP  
```
sudo apt install php libapache2-mod-php php-mysql(y and enter)  
sudo apt install nano(optional but if not used, make sure to use vim or vi whenever I call nano)  
sudo nano /etc/apache2/mods-enabled/dir.conf  
move index.php to the second position  
sudo systemctl restart apache2
```

I will be using database as the name as my "your_domain"  
```
sudo mkdir /var/www/your_domain(sudo mkdir /var/www/database)  
sudo chown -R $USER:$USER /var/www/your_domain (sudo chown -R $USER:$USER /var/www/database)
```
"$USER" uses current user  
```
sudo nano /etc/apache2/sites-available/your_domain.conf(sudo nano /etc/apache2/sites-available/database.conf)
```
```
<VirtualHost *:80>
  ServerName your_domain
  ServerAlias www.your_domain
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/your_domain
  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
This command connects apache to your domain instead of the default one
```
sudo a2ensite your_domain
```
This removed the connection between apache and the default directory 
```
sudo a2dissite 000-default
```
This command is optional but tests to ensure the config files are set up properly
```
sudo apache2ctl configtest
```
```
sudo systemctl reload apache2
```
# Setting up the configuration from apache to MySQL
```
sudo nano /var/www/your_domain/index.php(sudo nano /var/www/database/index.php)
```
example_user is the user in MySQL connecting to this IP address  
password is the password connected to the username  
example_database is the name of the database you want to read  
todo_list is the name of the table you want to read from  
```
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
```
