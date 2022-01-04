# WordPress Troubleshooting: Common Errors

## Table of Contents
1. Redirect to Old Site
2. Database Connection Error
3. Unable to upload files
4. Blank Page Displayed
5. Faulty plugin/theme
<p><br>
</p>

### Resources

* https://www.wpbeginner.com/wp-tutorials/how-to-properly-move-wordpress-to-a-new-domain-without-losing-seo/
* https://www.wpbeginner.com/wp-tutorials/how-to-change-your-wordpress-site-urls-step-by-step/
* https://kinsta.com/knowledgebase/wordpress-change-url/
* https://www.wpbeginner.com/wp-tutorials/how-to-fix-the-error-establishing-a-database-connection-in-wordpress/
<p><br>
<br>
</p>

## 1) Redirect to Old Site

### Scenario

A WordPress site was copied over to a new server to serve as a staging/development server. DNS records were created for staging.domain.com pointing to the new server. Whenever the Administrator tries to login to wp-admin.php, they are being redirected back to the original domain.
<p><br>
</p>

### How Does this Happen?

* There are database records that indicate the site_url and base_url/home that the application is responding to
  * This creates an Application level redirect
* site_url and base_url/home can also be hardcoded in wp-config.php
* Less commonly, URLs or IP addresses can be hardcoded into the website code itself, separate from the WordPress code
<p><br>
</p>

### How to Correct This Issue

* A proactive method would be to update the URLs in the Admin page and then backup the files and database
  * Revert changes if keeping existing installation as is
* If Admin page is inaccessible, access to the host server is required
  * Methods using SFTP/FTP/SSH
    * Update wp-content/themes/your-theme-folder/functions.php with the following at the bottom:
    ```
    update_option( 'siteurl', 'https://domain.com' );
    update_option( 'home', 'https://domain.com' );
    ```
    * Update wp-config.php by adding the following:
    ```
    define( 'WP_HOME', 'https://domain.com' );
    define( 'WP_SITEURL', 'https://domain.com' );
    ```
  * Method requiring SSH access
    * Use wp-cli from the terminal:
    ```
    wp option update home 'https://domain.com'
    wp option update siteurl 'https://domain.com'
    ```
  * Method requiring direct database access
    * Using PHPMyAdmin/MySQL Workbench
      * Find wp_options (by default) table, look for option_name of siteurl and home, update option_value with https://domain.com
    * Using SQL Query
      ```
      UPDATE wp_options SET option_value="http://new_host/wordpress" WHERE option_name = "home";
      UPDATE wp_options SET option_value="http://new_host/wordpress" WHERE option_name = "siteurl";
      ```
<p><br>
<br>
</p>

## 2) Database Connection Error

* This error is when the WordPress application is unable to communicate with the database backend
* Is prominently displayed when browsing to the website
* Is shown as a 500 status error code when running curl against the site
```

```
<p><br>
</p>

### Causes of this Error

* The host server has ran out of memory and oom-killer killed the mysqld process
* Incorrect database information in wp-config.php
* Database host server is down
<p><br>
<br>
</p>

### Typical Investigation Steps
* Check the status of the MySQL/MariaDB service

```
Ubuntu 14.04/RedHat 6 and earlier
---------------------------------

# Ubuntu
service mysql status

# RedHat
service mysqld status


Ubuntu 16.04/RedHat 7 and later
-------------------------------

# Ubuntu
systemctl status mysql

# RedHat
systemctl status mysqld
```

  * If the service is dead/inactive, parse the system logs for Out of Memory errors:

  ```

  # Ubuntu
  egrep -i "Out of Memory| oom" /var/log/syslog

  # RedHat
  egrep -i "Out of Memory| oom" /var/log/messages

  ```

* Check if wp-config.php contains the correct credentials
  * Configuration Example:

  ```
  root@Monkiko-WordPress:~# grep -i 'DB_' /var/www/html/wp-config.php
  define( 'DB_NAME', 'wordpress' );
  define( 'DB_USER', 'wordpress' );
  define( 'DB_PASSWORD', '07e03897714e8a123e634ebf08c6d2a5cfa4fa5f474ade6d' );
  define( 'DB_HOST', 'localhost' );
  define( 'DB_CHARSET', 'utf8' );
  define( 'DB_COLLATE', '' );
  ```

* In environments where the web server and the database server are located on different devices, need to check the database host server
<p><br>
<br>
</p>

### Remediation

* If the service is stopped/dead:

```
Ubuntu 14.04/RedHat 6 and earlier
---------------------------------

# Ubuntu
service mysql restart

# RedHat
service mysqld restart


Ubuntu 16.04/RedHat 7 and later
-------------------------------

# Ubuntu
systemctl restart mysql

# RedHat
systemctl restart mysqld
```

* Can test Database credentials using CLI:

```
root@localhost:~# mysql -h localhost -u wordpress -p
```

  * If credentials don't work, either update wp-config.php or update the credentials in MySQL/MariaDB
  <p><br>
  <br>
  </p>

## 3) Unable to Upload Files

* This error can appear in two circumstances:
  * While using SFTP, SSH, SCP, rsync to copy/upload files
  * Using the WordPress Admin control panel to upload files
* This is usually a matter of ownership and permissions
<p><br>
</p>
