```
chown -R www-data: html/

find html -type f -exec chmod 664 {} + -o -type d -exec chmod 775 {} +

usermod -a -G www-data myid
```
