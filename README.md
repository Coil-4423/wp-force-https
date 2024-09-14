# WP Force HTTPS

## Introduction
This repository provides a step-by-step guide on how to force HTTPS for a WordPress website. It includes configurations for `.htaccess`, `wp-config.php`, and other essential details for setting up SSL and enforcing secure connections.

## Prerequisites
- An SSL certificate installed (either via Let's Encrypt or a purchased one).
- Access to your WordPress Admin dashboard.
- Ability to edit server configuration files like `.htaccess` and `wp-config.php`.

To **force your WordPress live site** to be accessible only over **HTTPS**, you need to follow a few steps to ensure that all traffic is redirected to the secure HTTPS version. Here's a step-by-step guide to implement this:

### Step 1: Install an SSL Certificate
Before forcing HTTPS, make sure you have an **SSL certificate** installed on your website. Most hosting providers offer free SSL certificates through services like **Let's Encrypt**, or you may purchase one.

If you already have an SSL certificate installed, proceed to the next steps.

---

### Step 2: Update WordPress URLs to HTTPS

1. **Go to WordPress Admin**:
   - Navigate to **Settings > General** in the WordPress dashboard.

2. **Update the URLs**:
   - In the **WordPress Address (URL)** and **Site Address (URL)** fields, ensure both URLs start with `https://` instead of `http://`.
   - Example:
     - **WordPress Address (URL)**: `https://yourdomain.com`
     - **Site Address (URL)**: `https://yourdomain.com`

3. **Save Changes**:
   - After updating the URLs, click **Save Changes**. This change ensures that WordPress itself is aware that it should use HTTPS.

---

### Step 3: Force HTTPS via `.htaccess` (For Apache Servers)

If you're using an **Apache** server, you can use your `.htaccess` file to force all HTTP traffic to redirect to HTTPS.

1. **Edit Your `.htaccess` File**:
   - Using an FTP client (e.g., FileZilla) or your hosting provider's file manager, access your website's root directory where the `.htaccess` file is located.
   - Open the `.htaccess` file for editing.

2. **Add the Following Code to Redirect HTTP to HTTPS**:
   Add this code to the **top** of your `.htaccess` file (above the `# BEGIN WordPress` line):

   ```apache
   <IfModule mod_rewrite.c>
   RewriteEngine On
   RewriteCond %{HTTPS} !=on
   RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
   </IfModule>
   ```

   ### Explanation:
   - **`RewriteCond %{HTTPS} !=on`**: This condition checks if HTTPS is not enabled.
   - **`RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]`**: This rule redirects all HTTP requests to the HTTPS version of the same URL. The `301` status code is used for a permanent redirect.

3. **Save the File**:
   - Save and close the `.htaccess` file.

---

### Step 4: Force HTTPS via wp-config.php (Optional)

You can also add the following to your `wp-config.php` file to ensure WordPress enforces HTTPS:

1. **Edit the `wp-config.php` file**:
   - Access your website's root directory using FTP or your hosting file manager.

2. **Add the Following Code to `wp-config.php`**:
   Place this code before the line that says `/* That's all, stop editing! Happy publishing. */`.

   ```php
   define('FORCE_SSL_ADMIN', true);
   if (strpos($_SERVER['HTTP_X_FORWARDED_PROTO'], 'https') !== false)
       $_SERVER['HTTPS'] = 'on';
   ```

3. **Save the Changes**:
   - After adding the code, save and close the `wp-config.php` file.

---

### Step 5: Clear Caches

If you are using any caching plugins or your server has caching enabled, clear the cache to ensure the new redirect rules take effect.

### Step 6: Test Your Site

After applying the changes, visit your website using `http://` in the URL, and it should automatically redirect to the `https://` version.

---

### Step 7: Fixing Mixed Content Issues (If Any)

If you still see security warnings or mixed content issues (where some resources are still being loaded over HTTP), follow these steps:

- Install a plugin like **Really Simple SSL** which helps detect and fix mixed content issues.
- Or manually update internal links and resources (like images or stylesheets) to use HTTPS instead of HTTP.

---

### Summary of Actions:
1. **Ensure SSL is installed**.
2. **Update WordPress URLs** in **Settings > General**.
3. **Redirect HTTP to HTTPS** using `.htaccess` (or server config).
4. **Optional**: Add SSL enforcement to `wp-config.php`.
5. **Clear caches** and **test the redirection**.
