## Installing Xampp

install the non VM virtual machine version
Wordpress needs at least version 7 of PHP

## Moving `htdocs` folder outside the Xampp folder

#### **Problem: I can't access localhost after moving my htdocs folder to the Documents directory on my Mac. It says "Access forbidden".**

The problem is that the new MacOSX Catalina (and higher versions of MacOS) is implementing some restrictions to the special user folders (Documents, Desktop, Downloads for example). So one of the solutions is use another location to your htdocs directory. For example, I've moved my htdocs to my user folder: /User/marcelovieira/htdocs.

Of course, after moving the folder, you'll need to repeat all of the instructions given in the course. You'll need to open the httpd.conf file once again and change the two lines under DocumentRoot and Directory. Mine is like this now:

```
DocumentRoot "/Users/marcelovieira/htdocs"<Directory "/Users/marcelovieira/htdocs">
```

And, of course, you'll need to give read and write permissions to the new folder.



## How to configure 

````
//httpd.conf (opened thorugh XAMPP UI)

# DocumentRoot: The directory out of which you will serve your
# documents. By default, all requests are taken from this directory, but
# symbolic links and aliases may be used to point to other locations.
#
DocumentRoot "/Applications/XAMPP/xamppfiles/htdocs/www"
<Directory "/Applications/XAMPP/xamppfiles/htdocs/www">
````

This parts needs to be changed:
````
//httpd.conf 

DocumentRoot "/Users/esteban/htdocs"
<Directory "/Users/esteban/htdocs">
````

And this part as well:
```
User daemon
Group daemon
```

then, on the terminal:
````
esteban@Esteban-mbp001 ~ % id
uid=503(esteban) gid=20(staff)
````

````
//httpd.conf 

User esteban
Group staff
````

Then change the permission of the htdocss file by going to `get Info` then click the lock, then change the permissions to `Read and Write` (for user, staf and everyone) and then click the lock again



## Install wordpress

1. download the zip file from Wordpress.org
2. create a db on localhost/phpmyadmin
3. got to `localhost/your-wordpress-folder`
4. Database Name: your-chosen-name-on-php-my-admin
5. Username: root
6. Password: (empty)
7. Database host: localhost (changes on prod, ask server provider)
8. Table prefix (PLEASE, change it for sec reasons): wp_esteban
9. Finish installin

The database  is now populated with tables and some default items

e.g `wp_estebancomments`, `wp_estebaposts`

## Basics files for a theme

1. Index.php // entry point of the theme
2. Style.css // contains some theme metadata (name, author)

In old themes, index.php file has some content

in the twenty-twentytwo theme, other approach is used, with empty index.php file and HTML docs, with some commented code referencing some PHP files, wow!



## Theme hierarchy

index.php is the fallback template

if there's no 404.php file, going to /random, renders the content of index.php, is the fallback

https://code.tutsplus.com/articles/wordpress-cheat-sheets-theme-anatomy-model--wp-22579

There are certain files that have access to the LOOP (access to database data, like posts)

In the current WPC repo, themes are enabled like this https://wpcstaging4.dctdigital.com/wp-admin/network/themes.php



## Screenshot.png

that is the best extensions, and the image size should be 1200x900px



## Tags

if we plan to submit the theme to the WordPress theme repository, we need to follow some rules:
https://make.wordpress.org/themes/handbook/review/required/theme-tags/

Tags mark some features of the theme and helps users searching for it



## Shortcut for <html> templating

Just type this `!`, and you'll get:

````
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
````

that's an `Emmet` shortcut baked into VSCode, https://emmet.io/



## Body_class() hook, helping theming

````
<body <?php body_class(); ?> >

// in the browser
<body class="home blog logged-in admin-bar no-customize-support"
````

it says `home` so we could write some CSS that targets that home class, neat.



## Header.php

it must be named like that, or it can have another name, like `custom-header.php` and then be called like this: `get_header( 'custom' );`



## Loading styles

Functions.php only affect one theme (the one it's contained within), not like plugins that affect all

There many moments in the WP loading, and we can step into them with Hooks.

We can see these hooks in `wp-settings.php`.

````
// wp-settings.php line 220
require ABSPATH . WPINC . '/script-loader.php';
````

````
//script-loader.php line 2142

/**
 * Wrapper for do_action( 'wp_enqueue_scripts' ).
 *
 * Allows plugins to queue scripts for the front end using wp_enqueue_script().
 * Runs first in wp_head() where all is_home(), is_page(), etc. functions are available.
 *
 * @since 2.8.0
 */
function wp_enqueue_scripts() {
	/**
	 * Fires when scripts and styles are enqueued.
	 *
	 * @since 2.8.0
	 */
	👉 do_action( 'wp_enqueue_scripts' );
}

````

I don't fully understand this: the `do_action( 'wp_enqueue_scripts' )` code will run when `wp_enqueue_scripts() ` is called.

So there should be in the code some call to `wp_enqueue_scripts()` in the WP core files, so we can hook into that action.

I've found this file `wp-includes/class-wp-customize-manager.php` that has a call to that function 🤔



These `moments` are when `do_action('something')` is called in the WP core files

do_action( 'wp_enqueue_scripts' ); s an event caused when the devs call `wp_enqueue_scripts()` function, so it's a moment in the WP loading process that can happen or not.

other moments, like `init` always happen, no matter what the devs code on top of WP core files.

Step 1: 
````
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	...
    👉 <?php wp_head(); ?> // right before the end of the <head> content
 </head>   
````

Without that wp_head() there will be no <link> tag added to the <head> tag.

Step 2: create /css/template.css  + functions.php file and add this code

`````
// hook into the 'wp_enqueue_styles' hook
// functions.php

function load_scripts(){
	👉 wp_enqueue_style( 'templat-handle', get_template_directory_uri() . '/css/template.css', array(), '1.0', 'all' );
}
add_action( 👉 'wp_enqueue_scripts', 'load_scripts' );
`````

Then, inside the <head> element, this will appear in the browser:
````
<link rel="stylesheet" id="template-handle-css" href="http://localhost/wordpress-theme-development/wp-content/themes/learnwp/css/template.css?ver=1.0" type="text/css" media="all">
````

## Get_header behaviour

if we have:
````
// index.php

?php get_header(); ?>
````



````
// header-custom.php

some header code
````

Then, a default header will appear on the HTML of the front end, and the CSS files loaded via functions.php will load, as the default header.php that takes over, has a wp_head() call maybe.

No code from header-custom.php will be shown in the FE

## Loading boostrap

Jquery is part of the wp instalation already, so it's a matter of adding it to the dependancy array when enqueing Bootstrap JS file from the CDN

From bootstrap v5, Jquery is no longer a dependancy! 🎉

````
<?php 

// Including stylesheet and script files
function load_scripts(){
	wp_enqueue_script( 'bootstrap-js', 'https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta/js/bootstrap.min.js', array( 'jquery' ), '4.0.0', 👉 true ); // load in footer
	wp_enqueue_style( 'bootstrap-css', 'https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta/css/bootstrap.min.css', array(), '4.0.0', 'all' );	
	wp_enqueue_style( 'template', get_template_directory_uri() . '/css/template.css', array(), '1.0', 'all' );
}
add_action( 'wp_enqueue_scripts', 'load_scripts' );
````

Make sure you call wp_footer() to load the script in the footer
````
<footer>Footer here</footer>
<?php wp_footer(); ?>
</body>
</html>
````

then, in the browser:

````
</footer>
<script type="text/javascript" src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta/js/bootstrap.min.js?ver=4.0.0" id="bootstrap-js-js"></script>
````



## Testing bootstrap

add this to the metadata to make the site responsive:
````
<meta name="viewport" content="width=device-width, initial-scale=1.0" >
````



## Creating Menus

let's give editors the ability to change the nav menus on the site

with the default instalation, there's no Menu UI on the admin dashboar under `Appearance` item on the left.

We need to register a menu on functions.php

````
register_nav_menus(
	array(
		'my_main_menu' => 'Main Menu',
		
		👉 `another_menu` (it can be called from PHP with this string)
		👉'Another Menu'(it appears under that name on the Location list in the WP dashboar)
	)
);
````

Then, going to `Appearance`->`Menus`, create a new Menu called `Top Menu` or whatever, and hook it to the registered menu when it says `Menu settings`-> `Display location` and check the box it says `Main Menu`

by default, published pages appear on the menu when it's created. To add new page we'll create in the future, enable this option `Automatically add new top-level pages to this menu`



To render the menu in the front end, we need a wp function:
````
//header.php

<nav class="main-menu"><?php 👉 wp_nav_menu( array( 'theme_location' => 'my_main_menu' ) );?></nav>
````



This is the menu in the front end:
````html
<div class="menu-top-menu-container">
   <ul id="menu-top-menu" class="menu">
      <li id="menu-item-17" class="menu-item menu-item-type-custom menu-item-object-custom menu-item-home menu-item-17"><a href="http://localhost/wordpress-theme-development/">Home</a></li>
      <li id="menu-item-18" class="menu-item menu-item-type-post_type menu-item-object-page current-menu-item page_item page-item-10 current_page_item menu-item-18"><a href="http://localhost/wordpress-theme-development/contact/" aria-current="page">Contact</a></li>
      <li id="menu-item-20" class="menu-item menu-item-type-post_type menu-item-object-page menu-item-20"><a href="http://localhost/wordpress-theme-development/services/">Services</a></li>
   </ul>
</div>
````



if we click on pages, then nothing different than the content of index.php is shown.



## The loop

we can loop through the posts (regular posts, pages, custom post type like events, and so on) and call functions to get certain data from the post, like the title, link, etc and display it in the FE.

by default, we'll get all the posts found in the db, maybe that's too much, we'll find ways to get fewer from the db later in the course.

To loop through the regulars posts:

````php+HTML
// index.php

 <?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
   <h2><?php the_permalink(); ?></h2>
   <?php endwhile;?>
 <?php endif; ?>
````

````php

// src code

function have_posts() {
    global $wp_query;
    return $wp_query->have_posts();
}
````

````php

// src code

function the_post() {
    global $wp_query;
    $wp_query->the_post();
}
````

````php
function the_permalink( $post = 0 ) {
    /**
     * Filters the display of the permalink for the current post.
     *
     * @since 1.5.0
     * @since 4.4.0 Added the `$post` parameter.
     *
     * @param string      $permalink The permalink for the current post.
     * @param int|WP_Post $post      Post ID, WP_Post object, or 0. Default 0.
     */
    echo esc_url( apply_filters( 'the_permalink', get_permalink( $post ), $post ) );
}
````



## Template tags

are just the functions that brings some pieces of info from the post:

- the_permalink();
- The_title()
- etc

When there is `get_` , e.g `get_the_data` we need to echo out the result to be displayed in the HTML, because usually those functions use 👉`return` instead of `echo`.

````
function 👉get_the_date( $format = '', $post = null ) {
    $post = get_post( $post );
 
    if ( ! $post ) {
        return false;
    }
 
    $_format = ! empty( $format ) ? $format : get_option( 'date_format' );
 
    $the_date = get_post_time( $_format, false, $post, true );
 
    /**
     * Filters the date a post was published.
     *
     * @since 3.0.0
     *
     * @param string|int  $the_date Formatted date string or Unix timestamp if `$format` is 'U' or 'G'.
     * @param string      $format   PHP date format.
     * @param WP_Post     $post     The post object.
     */
   👉 return apply_filters( 'get_the_date', $the_date, $format, $post );
}
````



Let's see an example of the_title()

```
function the_title( $before = '', $after = '', 👉 $echo = true ) {
    $title = get_the_title();
 
    if ( strlen( $title ) == 0 ) {
        return;
    }
 
    $title = $before . $title . $after;
 
    if ( $echo ) {
       👉 echo $title;
    } else {
        return $title;
    }
}
```



So far, the list of post is shown on the Home page, that's not great, I wanna show them on the Blog page

The loop still works but it iterates once, getting me the Contact page post data in the HTML:
```
//current situation

http://localhost/wordpress-theme-development/home/
outputs the array of posts in the HTML 🤦‍♂️


http://localhost/wordpress-theme-development/blog/
🤦‍♂️
Blog
July 19, 2022
This is the blog page


```

The roles are mixed ‼️, I want the opposite, the home page to show some Home page content, and the Blog page to  show the list of posts.

🔧 To do that, we need to go to the admin dashboard:
Settings->Reading->Your homepage displays->
Swith from `Your latest posts` to `A [static page](http://localhost/wordpress-theme-development/wp-admin/edit.php?post_type=page) (select below)`

And set 
Homepage: Home

Postpage: Blog



Notes: Blog and Home is a page I created already, because that's the only way to use the `Static page` setup above.



By default, in the navbar there's a Home page, but that one is a default that can't be used in the static setup unfortunately.



Make sure you don't have duplicate Home links in the navbar, by checking the Menu in the admin dashboard and deleting the default Home if it's present there.



## Template pages

how to change the content and look of a page?

### Way #1: page-something.php:

````
// page-services.php will power `/services` page

<?php get_header(); ?>
	<div class="content-area">
		<main>
			<h1>Services page here!, powered by services.php</h1>
		</main>
	</div>
<?php get_footer(); ?>
````

If we go to the home page, we see that the permalink is [http://localhost/wordpress-theme-development/(opens in a new tab)](http://localhost/wordpress-theme-development/), but to edit overrride it, we need to create a `page-home.php` file, so in this case the permalink segment and the file name don't match, but it does for the rest of the pages.

### Way #2: something-template.php:

````
//general-template.php

<?php
/** 
 Template Name: General Template
*/

?>

<?php get_header(); ?>
	<div class="content-area">
		<main>
            <h1>Page powered by general-template.php</h1>
		</main>
	</div>
<?php get_footer(); ?>
````

Then, on the dashboard, when going to any page, we'll see a dropdown under `Templates` and I'll be able to choose the new template called `General Template`. The `Default` template refers to the `index.php` file, as that file is the fallback file for all pages.

the advantage is that this template can power many pages at the same time, it's just a matter of selecting it in the dashboar on a specific page.





## Theme support

Editing things of the FE, like logos, header image, etc. from the backend.

The idea is to add some placeholders to the theme, that show up in the dashboard in the Theme Customizer, under `Appearance-> Customize`, among other places (e.g Feature Image input (input fields are called metaboxes) will appear on an individual page of a Post)

What if we want to add a background image for the header?

````
//functions.php

function learnWPConfig() {
	$args = [
		'height' => 225,
		'width' => 1920
	];
    
	👉 add_theme_support( 'custom-header', $args);
}

👉 add_action('after_setup_theme', 'learnWPConfig', 👉 0);
````



```
//can be used anywhere!

<img class="img-fluid" src="<?php 👉 header_image(); ?>" height="<?php echo 👉get_custom_header()->height; ?>" width="<?php echo 👉 get_custom_header()->width; ?>" alt="" />

```

so we could use it just inside `page-home.php` or for all pages, on `header.php`, so it's really flexible.

They do the job of setting up ACF for common things, in this case is built in with functions to grab it in a PHP template.



## Adding thumbnail to posts

If we go to a post edit page, like http://localhost/wordpress-theme-development/wp-admin/post.php?post=24&action=edit

we have no fields for picking up a thumbnail image. 

So let's make that happen!

```
add_theme_support( 'post-thumbnails' ); 
```

Then, we can consume it:
````
if ( has_post_thumbnail() ) {
	👉 the_post_thumbnail();
} 
````

But, what size is the displayed featured image?
```
<img width="1915" height="2560" src="http://localhost/wordpress-theme-development/wp-content/uploads/2022/08/1655549120709-scaled.jpeg" class="attachment-post-thumbnail size-post-thumbnail wp-post-image" alt="" srcset="http://localhost/wordpress-theme-development/wp-content/uploads/2022/08/1655549120709-scaled.jpeg 1915w, http://localhost/wordpress-theme-development/wp-content/uploads/2022/08/1655549120709-224x300.jpeg 224w, http://localhost/wordpress-theme-development/wp-content/uploads/2022/08/1655549120709-766x1024.jpeg 766w, http://localhost/wordpress-theme-development/wp-content/uploads/2022/08/1655549120709-768x1027.jpeg 768w, http://localhost/wordpress-theme-development/wp-content/uploads/2022/08/1655549120709-1149x1536.jpeg 1149w, http://localhost/wordpress-theme-development/wp-content/uploads/2022/08/1655549120709-1532x2048.jpeg 1532w" sizes="(max-width: 1915px) 100vw, 1915px">
```

The above code snippet renders the full size image as it was uploaded, because that's the default thumbnail size.

We can pass arguments:
````
the_post_thumbnail( 👉 'thumbnail'); 

// displays a 150x150 image
````

Sizes for each thumbnail size ('thumbail' , 'medium', etc, can be changed.

To check the current sizes, go to `Settings -> Media Settings`

To see all the versions of the uploaded images, just go to the code editor, and go to the `uploads` folder.

Images are too big to be kept on databases.

````
<img width="150" height="150" src="http://localhost/wordpress-theme-development/wp-content/uploads/2022/08/1655549120709-150x150.jpeg" class="attachment-thumbnail size-thumbnail wp-post-image" alt="">
````

If we wanna render another size:

````
the_post_thumbnail( 👉[500, 500]);
````

Then, an image from the pre-defined sizes is rendered and the with and height attributes are set to 500

We can also add a new pre-defined size, like `the-courier-thumbnail` , by following these instructions: 

```
 add_image_size( 'the-courier-thumbnail', 275, 275 ); 
```



## Template parts

Instead of using `require`, we use a built in function:

````
get_template_part( 'path-to-file');

get_template_part( '/template-parts/content');
````



## Post Formats

what if we want the posts to look different on each category when looping over them (e.g show only the image, a video, etc)

There are 9 predefined formats that can't be changed, to ensure compatibility with themes.

let's add that `Format` metafield box on the single post edit page:
````
add_theme_support( 'post-formats', [ 'video', 'image'] );
````

Then, we can chose the appropiate format for each post, and display a different UI when looping through them:
```
/template-parts
  /content.php
  /content-video.php
  /content-image.php
  
  each template part can render the post data differentyly
```



````
<?php get_header(); ?>
	...
				 <?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
						<?php get_template_part('/template-parts/content', 👉 get_post_format()); ?>
					<?php endwhile;?>
	...			
<?php get_footer(); ?>
````

That way, we can load a different template part depending on the Format, on each loop iteration, pretty neat!

For styling purposes, we can target the classes added by WP to the different post formats:
````
/content.php

<h1><?php 👉 post_class(); ?>
````



## Adding Sidebars

sidebars contain `wisgets` like: latest posts, recommended reads, etc

Having Sidebars allows users to customize parts of the FE, like it's done with Menus

A `Widgets` section will appear under `Appearance` item in the dashboard.

```
// functions.php

// Registering our sidebars
add_action( 'widgets_init', 'learnwp_sidebars' );
function learnwp_sidebars(){
	register_sidebar(
		array(
			'name' => 'Home Page Sidebar', (for the Dashboard)
			'id' => 'sidebar-1', (for css classes)
			'description' => 'This is the Home Page Sidebar. You can add your widgets here. ', (for the dashboard)
			'before_widget' => '<div class="widget-wrapper">', (change the default widget wrapper which is <section>)
			'after_widget' => '</div>',
			'before_title' => '<h2 class="widget-title">', (change the default widget title tag)
			'after_title' => '</h2>'
		)
	);
	register_sidebar(
		array(
			'name' => 'Blog Sidebar',
			'id' => 'sidebar-2',
			'description' => 'This is the Blog Sidebar. You can add your widgets here. ',
			'before_widget' => '<div class="widget-wrapper">',
			'after_widget' => '</div>',
			'before_title' => '<h2 class="widget-title">',
			'after_title' => '</h2>'
		)
	);	
}
```

And on each Sidebar, we can add whatever widgets we want, any guttenbergh block (image, text, )

After adding blocks to the sidebars, we can then consume them in the FE!
```
// sidebar.php

<?php
if (is_active_sidebar('sidebar-1')){ // checks if the sidebar-1 has widgets
    dynamic_sidebar('sidebar-1');
}
```

```
// any page
<section class="slide"><?php 👉 get_sidebar(); ?></section>
```

We can have multiple sidebar php files,

````
// sidebar-home.php, sidebar-blog.php, etc
````

````
<section class="slide"><?php 👉 get_sidebar('home'); ?></section>
````



A section of Services with 3 cards, can be done with 3 sidebars that have a card each (made up of text and image widgets)

So we can make parts of the FE content editable with sidebars.



## WPQUERY

it's a way of changing the default behaviour of looping thorugh posts on the index.php page, or through the current service page (which is a post) when going to /services page.

What if I want to loop through a list of products and events in the same page? I need 2 loops.

There are 2 methods:

- Query string: `post_type=post&posts_per_page=1&cat=`, etc
- Array [arg1 => value1, arg2 => value2 ✅ ( prefered option, cleaner)

Pass the Category, there are 2 ways:

- cat=4,8, etc (pass the id)
- Category_name=politics (pass the slug)

Just go to the dashboard and check the url for each category, and grab the id, or check the slug

The best option is the cat arg, because the id will never change.

````php+HTML
<h1>looping coding posts</h1>
<?php
// The Query
$query1 = new WP_Query( 'post_type=post&cat=10' );

// The Loop
while ( $query1->have_posts()): ?>
<?php $query1->the_post(); ?>
<!-- I can use the normal template tags here -->
<li><?php the_title(); ?></li>
<?php endwhile ?>
<?php wp_reset_postdata();
?>
````

```
// better syntax

$query1 = new WP_Query( ['post_type' => 'post', 'cat' => 10 ] );
```

