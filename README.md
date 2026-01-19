<VirtualHost *:80>
    ServerAdmin arefsajjadi80@gmail.com
    ServerName localhost
    ServerAlias oauthweb
    DocumentRoot /var/www/public

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    <Directory /var/www>
        Options FollowSymLinks
        AllowOverride All
    </Directory>

    <Directory /var/www/public>
         Options Indexes FollowSymLinks MultiViews
         AllowOverride All
         Order allow,deny
         allow from all
         Header set Access-Control-Allow-Origin: *
         Header set Access-Control-Allow-Methods: 'GET,POST,PATCH,DELETE,PUT,OP>
         Header set Access-Control-Allow-Headers: 'Origin, Content-Type, Author>
    </Directory>
</VirtualHost>

