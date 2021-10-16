# What is XAMPP?  XAMPP (Windows, Mac OS X, Linux, Solaris)
XAMPP is the most popular PHP development environment
https://www.apachefriends.org/index.html

Change the permissions to the installer
```
chmod 755 xampp-linux-x64-8.0.11-2-installer.run
```
Run the installer

```
sudo ./xampp-linux-x64-8.0.11-2-installer.run
```


![Screenshot from 2021-10-15 12-28-21](https://user-images.githubusercontent.com/21187699/137543285-4eea41fa-512e-4c91-9c06-8036d82a4621.png)
)

config apache web server port to 168
mysql port to 3306




https://www.apachefriends.org/faq_linux.html

To start XAMPP simply call this command:
```sh
sudo /opt/lampp/lampp start
```
open xampp control panel
```
sudo /opt/lampp/manager-linux-x64.run
```

phpmyadmin
```browser
http://localhost:168/phpmyadmin/
```


```install mysql
sudo apt install mysql-client-core-8.0
```
mysql

change root password
Now type the following query in the textarea and click Go
```
UPDATE mysql.user SET Password=PASSWORD('password') WHERE User='root'; FLUSH PRIVILEGES;
```


apache root path
```
cd /opt/lampp/htdocs
```



