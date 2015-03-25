# Snippets
A collection of code snippets that might be useful

## Table of contents
- [Display Custom Post Types in Search Results](#display-custom-post-types-in-search-results)
- [Display Child Pages of a Parent Page](#display-child-pages-of-a-parent-page)
- [Show a Message on Admin View](#show-a-message-on-admin-view)
- [Controlling Menu Items on Backend](#controlling-menu-items-on-backend)
- [Turn on WordPress Error Reporting](#turn-on-wordpress-error-reporting)
- [“Edit This” Button on Posts and Pages](#edit-this-button-on-posts-and-pages)
- [Add Category Name to body_class](#add-category-name-to-body_class)
- [Admin Panel Link Only For Admins](#admin-panel-link-only-for-admins)
- [Custom Database Error Page](#custom-database-error-page)
- [Disable Automatic Formatting](#disable-automatic-formatting)
- [Get Featured Image URL](#get-featured-image-url)
- [Insert Images with Figure/Figcaption](#insert-images-with-figurefigcaption)
- [Make Archives.php Include Custom Post Types](#make-archivesphp-include-custom-post-types)
- [Remove Private/Protected from Post Titles](#remove-privateprotected-from-post-titles)
- [Simple maintenance mode](#simple-maintenance-mode)
- [Year Shortcode](#year-shortcode)


## Display Custom Post Types in Search Results
Example: http://codex.wordpress.org/Plugin_API/Action_Reference/pre_get_posts

```php
add_action( 'pre_get_posts', 'pre_get_posts_example' );
function pre_get_posts_example( $query ) {
  if ( ! is_admin() && $query->is_main_query() ) {
    if ( $query->is_search ) {
      $query->set( 'post_type', array( 'post', 'custom-post-type' ) );
    }
  }
}
```


## Display Child Pages of a Parent Page
Source: http://stackoverflow.com/a/23117529

```php
  <?php
  if ($post->post_parent) {
    $ancestors = get_post_ancestors($post->ID);
    $parent = $ancestors[count($ancestors) - 1];
  } else {
    $parent = $post->ID;
  }

  $children = wp_list_pages("title_li=&child_of=" . $parent . "&echo=0");
  if ($children) { ?>
    <ul class="subnav">
      <?php echo $children; ?>
    </ul>
  <?php } ?>
```



## Show a Message on Admin View
Example: http://wpsnippy.com/show-notification-message-wordpress-admin-pages/

```php
add_action( 'admin_notices', 'admin_notices_example' );
function admin_notices_example() {
  echo '<div class="error">
    <p>We are performing website maintenance. Please don\'t make any changes in your posts until further notice!</p>
  </div>';
}
```


## Controlling Menu Items on Backend
Example: http://wpsnippy.com/remove-top-level-wordpress-dashboard-menu/

```php
if ( ! current_user_can( 'manage_options' ) ) {
    add_action( 'admin_menu', 'admin_menu_example' );
}

function admin_menu_example() {
  remove_menu_page( 'index.php' );                  //Dashboard
  remove_menu_page( 'edit.php' );                   //Posts
  remove_menu_page( 'upload.php' );                 //Media
  remove_menu_page( 'edit.php?post_type=page' );    //Pages
  remove_menu_page( 'edit-comments.php' );          //Comments
  remove_menu_page( 'themes.php' );                 //Appearance
  remove_menu_page( 'plugins.php' );                //Plugins
  remove_menu_page( 'users.php' );                  //Users
  remove_menu_page( 'tools.php' );                  //Tools
  remove_menu_page( 'options-general.php' );        //Settings

}
```


## Turn on WordPress Error Reporting
Comment out the top line there, and add the rest to your `wp-config.php file to get more detailed error reporting from your WordPress site. Definitely don't do this live, do it for local development and testing.

```php
// define('WP_DEBUG', false);

define('WP_DEBUG', true);
define('WP_DEBUG_LOG', true);
define('WP_DEBUG_DISPLAY', false);
@ini_set('display_errors', 0);
```


## “Edit This” Button on Posts and Pages
```php
<?php edit_post_link(__('Edit This')); ?>
```


## Add Category Name to body_class
```php
add_filter('body_class','add_category_to_single');
function add_category_to_single($classes, $class) {
  if (is_single() ) {
    global $post;
    foreach((get_the_category($post->ID)) as $category) {
      // add category slug to the $classes array
      $classes[] = $category->category_nicename;
    }
  }
  // return the $classes array
  return $classes;
}
```


## Admin Panel Link Only For Admins
```php
<?php if (current_user_can("manage_options")) : ?>
  <a href="<?php echo bloginfo("siteurl") ?>/wp-admin/">Admin</a>
<?php endif; ?>
```


## Custom Database Error Page
Put a file called "db-error.php" directly inside your /wp-content/ folder and WordPress will automatically use that when there is a database connection problem.

```php
<?php // custom WordPress database error page

  header('HTTP/1.1 503 Service Temporarily Unavailable');
  header('Status: 503 Service Temporarily Unavailable');
  header('Retry-After: 600'); // 1 hour = 3600 seconds

  // If you wish to email yourself upon an error
  // mail("your@email.com", "Database Error", "There is a problem with the database!", "From: Db Error Watching");

?>

<!DOCTYPE HTML>
<html>
<head>
<title>Database Error</title>
<style>
body { padding: 20px; background: red; color: white; font-size: 60px; }
</style>
</head>
<body>
  You got problems.
</body>
</html>
```


## Disable Automatic Formatting
The wptexturize function is responsible for lots of automatic alterations to text stored in WordPress like automatic elipses (...), em and en dashes, typographers quotes, etc. Add the followings to `functions.php`

```php
remove_filter('the_content', 'wptexturize');
remove_filter('the_excerpt', 'wptexturize');
remove_filter('comment_text', 'wptexturize');
remove_filter('the_title', 'wptexturize');
```


## Get Featured Image URL
```php
// Add the following to functions.php if the theme has no featured image support
add_theme_support('post-thumbnails');

// Then fetch featured image url via the following
$thumb_id = get_post_thumbnail_id();
$thumb_url_array = wp_get_attachment_image_src($thumb_id, 'thumbnail-size', true);
$thumb_url = $thumb_url_array[0];
```


## Insert Images with Figure/Figcaption
```php
// Add the following to functions.php
function html5_insert_image($html, $id, $caption, $title, $align, $url) {
  $html5 = "<figure id='post-$id media-$id' class='align-$align'>";
  $html5 .= "<img src='$url' alt='$title' />";
  if ($caption) {
    $html5 .= "<figcaption>$caption</figcaption>";
  }
  $html5 .= "</figure>";
  return $html5;
}
add_filter( 'image_send_to_editor', 'html5_insert_image', 10, 9 );
```


## Make Archives.php Include Custom Post Types
```php
// Add the following to functions.php
function namespace_add_custom_types( $query ) {
  if( is_category() || is_tag() && empty( $query->query_vars['suppress_filters'] ) ) {
    $query->set( 'post_type', array(
     'post', 'nav_menu_item', 'your-custom-post-type-here'
    ));
    return $query;
  }
}
add_filter( 'pre_get_posts', 'namespace_add_custom_types' );
```


## Remove Private/Protected from Post Titles
```php
// Add the following to functions.php
function the_title_trim($title) {

  $title = attribute_escape($title);

  $findthese = array(
    '#Protected:#',
    '#Private:#'
  );

  $replacewith = array(
    '', // What to replace "Protected:" with
    '' // What to replace "Private:" with
  );

  $title = preg_replace($findthese, $replacewith, $title);
  return $title;
}
add_filter('the_title', 'the_title_trim');
```

## Simple maintenance mode
Example: http://wp-snippets.com/articles/7-code-snippets-you-should-use-on-every-site/

```php
add_action( 'get_header', 'get_header_example' );
function get_header_example() {
  if ( ! current_user_can( 'activate_plugins' ) ) {
    wp_die( 'The website will be back soon.' );
  }
}
```

## Year Shortcode
Use [year] in your posts

```php
// Add the following to functions.php
function year_shortcode() {
  $year = date('Y');
  return $year;
}
add_shortcode('year', 'year_shortcode');
```
