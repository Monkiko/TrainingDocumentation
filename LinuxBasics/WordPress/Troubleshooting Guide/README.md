# WordPress Troubleshooting: Common Errors

## Table of Contents
1. Redirect to Old Site
2. Database Connection Error
3. Unable to upload files
4. Displays a blank page
5. Faulty plugin/theme
<p><br>
<br>
</p>

### Resources

* https://www.wpbeginner.com/wp-tutorials/how-to-properly-move-wordpress-to-a-new-domain-without-losing-seo/
* https://www.wpbeginner.com/wp-tutorials/how-to-change-your-wordpress-site-urls-step-by-step/
* https://kinsta.com/knowledgebase/wordpress-change-url/
<p><br>
<br>
</p>

## Redirect to Old Site

### Scenario

A WordPress site was copied over to a new server to serve as a staging/development server. DNS records were created for staging.domain.com pointing to the new server. Whenever the Administrator tries to login to wp-admin.php, they are being redirected back to the original domain.
<p><br>
<br>
</p>

### How Does this Happen?

* There are database records that indicate the site_url and base_url/home that the application is responding to
  * This creates an Application level redirect
* site_url and base_url/home can also be hardcoded in wp-config.php
* Less commonly, URLs or IP addresses can be hardcoded into the website code itself, separate from the WordPress code
<p><br>
<br>
</p>

### How to Correct This Issue

* A proactive method would be to update the URLs in the Admin page and then backup your files and database
  * Revert your changes if keeping existing installation as is
* If you cannot access your Admin page, you will need to access the host server
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
    * Use wp-cli from the terminal
    ```
    wp option update home 'https://domain.com'
    wp option update siteurl 'https://domain.com'
    ```
  * Method requiring direct database access
    * Using PHPMyAdmin/MySQL Workbench
      * Find wp_options (by default) table, look for option_name of siteurl and home, update option_value with https://domain.com
    * Using SQL Query
      * UPDATE
<p><br>
<br>
</p>

## Database Connection Error
