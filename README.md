### CMS Engine

The choosen engine for all the projects at the moment is native Wordpress (hosted); eventually i'm keen on switching to ClassicPress (https://www.classicpress.net), but while there's still some plugins of choose using React and modules from native WP, switch is not fully possible at the moment.

### Plugins used

1. Advanced Custom Fields PRO (https://www.advancedcustomfields.com/pro/) - custom fields for editing any content available from the CMS itself;
2. CompressX (https://wordpress.org/plugins/compressx/) - for automatic WEBP/AVIF images compression; there's other plugins that handle WEBP auto-compression, but AVIF compression is not always free in those;
3. Contact Form 7 (https://wordpress.org/plugins/contact-form-7/) - forms handling; most clean, lightweight and non-ui-bloated way;
4. Disable Media Pages (https://wordpress.org/plugins/disable-media-pages/) - while Wordpress up from version 6.4 has option to automatically disable attachment pages, redirects and slug handling is still tone incorrectly, this plugin handle those;
5. Easy SVG Support (https://wordpress.org/plugins/easy-svg/) - SVG uploads handling; while there's other plugins like Safe SVG or SVG support, the problem is that the first one has problem with uploads in any other place than media library and the latter is outdated;
6. Clean Image Filenames (https://wordpress.org/plugins/clean-image-filenames/) - for filenames sanitization; alternatively Filenames to lating can be used too, but it's outdated;
7. Classic Editor (https://wordpress.org/plugins/classic-editor/) - since i'm not using Gutenberg in any of my projects, the plugin is bringing back the old WYSIWYG/Text editor;
8. Mobile Detect (https://wordpress.org/plugins/tinywp-mobile-detect/) - optionally, if wp_is_mobile() is used in project and the functions needs to exclude tablets;
9. Regenerate Thumbnails (https://wordpress.org/plugins/regenerate-thumbnails/) - optionally, if project needs regenerating thumbnails at some point of development;
10. Pressidium Cookie Consent (https://wordpress.org/plugins/pressidium-cookie-consent/) - cookies handling, with saving user consents and Google Consent mode v2 compability;
11. The SEO Framework (https://wordpress.org/plugins/autodescription/) - clean, fast and lightweight SEO handling on website; additionally if needed some schema3 json scripts needs to be added, since those are not "free" per se; i'm handling the polish translation for the plugin, so I have extra understanding of how the plugin or the functions works;
12. WP-PageNavi (https://wordpress.org/plugins/wp-pagenavi/) - optionally, if projects needs to use pagination with numbers;
13. WP-Sweep (https://wordpress.org/plugins/wp-sweep/) - for cleaning revisions, orphaned meta and such; plugin can even handle large websites;
14. Migration, Backup, Staging – WPvivid Backup & Migration (https://wordpress.org/plugins/wpvivid-backuprestore/) - for backups handled by CMS.

### Translations handling

Pretty much only viable option is WPML; Polylang have too many paywalled options and can cause some "weird" problems, while WPML can even provide extended support for not always related issues. It's kind of heavy for the website and I would prefer using things as BOGO (https://wordpress.org/plugins/bogo/), but realistically speaking it's mostly not possible in projects.

### Security measures

1. NO heavyweight security engines like Wordfence.
2. For .htaccess in root folder:
```
<FilesMatch "wp-config.*\.php|\.htaccess|readme\.html">
Order allow,deny
Deny from all
</FilesMatch>
```
\- we don't want anyone to access those.
```
RewriteCond %{QUERY_STRING} author=\d
RewriteRule ^ /? [L,R=301]
```
\- disable user enumeration

3. For wp-config.php:

* Move DB credentials to another file, for example:
```
// DB credentials
require_once "db-001x.php";
```
* Disable Debug completely
```
// Debug
define( 'WP_DEBUG', false );
define( 'WP_DEBUG_LOG', false );
define( 'WP_DEBUG_DISPLAY', false );
@ini_set( 'display_errors', 0 );
```

* Additional defines
```
define(	'DISALLOW_FILE_EDIT', true); // Disable file edits from CMS
define( 'WP_POST_REVISIONS', 10 ); // We don't want to have too much of revisions for each post/page
define( 'WP_DISABLE_FATAL_ERROR_HANDLER', true); // We are disabling Wordpress internal site-health.php, since it's too confusing for clients and too time consuming to explain each time why some of those "errors" are not actually viable, also we are handling website maintenance by ourselfs, so there's no need for such third-party tool
```

* Fresh set of SALT keys after deploying to production.

4. For /uploads/ folder
.htaccess with:
```
<FilesMatch "\.(?i:php)$">
	Order allow,deny
	Deny from all
</FilesMatch>
```
\- any kind of .php files should not be present in that folder, or even run from.

5. For /wp-includes/ folder
.htaccess with:
```
<FilesMatch "\.(?i:php)$">
	Order allow,deny
	Deny from all
</FilesMatch>
<Files wp-tinymce.php>
	Allow from all
</Files>
<Files ms-files.php>
	Allow from all
</Files>
```
\- same as above + handle wp-tinymce.php and ms-files.php properly.

6. Database credentials
* db_name != db_user;
* strong password;
* table prefix CANNOT start with wp_
* the database CAN'T be shared between websites.

7. Accounts
* NO "admin" "wp_admin" or any kind of "admin" suffix/prefix related account names.

8. If possible, set BasicAuth for login page (wp-login.php/wp-admin).

9. If possible, set 2FA.

10. SSL is a must for any project.
