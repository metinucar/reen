# Reen Starter Theme

- [Extras](lib/extras.php) - A collection functions and filters that are essential
- [Snippets](snippets.md) - A collection of code snippets that might be useful
- [Plugins](plugins.md) - A collection of plugins that are recommended
- [.htaccess example](htaccess-example.md) - A `.htaccess`example

## Requirements

| Prerequisite    | How to check | How to install
| --------------- | ------------ | ------------- |
| PHP >= 5.4.x    | `php -v`     | [php.net](http://php.net/manual/en/install.php) |
| Node.js 0.12.x  | `node -v`    | [nodejs.org](http://nodejs.org/) |
| gulp >= 3.8.10  | `gulp -v`    | `npm install -g gulp` |
| Bower >= 1.3.12 | `bower -v`   | `npm install -g bower` |

## Install Wordpress
```bash
mkdir new-wordpress-project
cd new-wordpress-project
wget http://wordpress.org/latest.tar.gz
tar xfz latest.tar.gz
mv wordpress/* ./
rmdir ./wordpress/
rm -f latest.tar.gz
mv wp-config-sample.php wp-config.php
```

### Symlink project folder to MAMP/htdocs
```bash
cd ~/Applications/MAMP/htdocs
ln -s /path/to/new-wordpress-project symlink-name
```

## Install Theme

### Specify environment
```bash
subl wp-config.php // Open wp-config.php with fav editor
```

```php
define('WP_ENV', 'development'); // Add this for development environment
define('WP_ENV', 'production'); // Add this for production environment
```

### Download and Install Reen
```bash
cd wp-content/themes/
git clone git@github.com:ucarmetin/reen.git
```

```bash
npm install -g npm@latest // node.js, latest version
npm install -g gulp bower // gulp and bower, latest versions
```

```bash
cd wp-content/themes/reen/ // Navigate to the theme directory
npm install // Install dependencies
bower install // Install bower dependencies
```

### Configuration
Edit `lib/config.php` to enable or disable theme features and to define a Google Analytics ID.
Edit `lib/init.php` to setup navigation menus, post thumbnail sizes, post formats, and sidebars.

## gulp commands
* `gulp` — Compile and optimize the files in your assets directory
* `gulp watch` — Compile assets when file changes are made
* `gulp --production` — Compile assets for production (no source maps).

## Using BrowserSync

To use BrowserSync during `gulp watch` you need update `devUrl` at the bottom of `assets/manifest.json` to reflect your local development hostname.

For example, if your local develoment URL looks like `http://localhost:8888/project-name/` you would update the file to read:

```json
...
  "config": {
    "devUrl": "http://localhost:8888/project-name/"
  }
...
```

## Sycn with production
[Sublime FTP](http://wbond.net/sublime_packages/sftp) package with the config file, `sftp-config.json`, can be used for file synchronization between development and remote server.

## [Sage](https://github.com/roots/sage)
Sage documentation is available at [https://roots.io/sage/docs/](https://roots.io/sage/docs/).

## [Soil](https://github.com/roots/soil)
Install the Soil plugin to enable additional features:
* Cleaner output of `wp_head` and enqueued assets
* Root relative URLs
* Nice search (`/search/query/`)
