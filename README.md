# timber_php ====> https://github.com/timber

<h3> Initialize timber</h3>

```
// index.php

$context = Timber::get_context();
$context['posts'] = Timber::get_posts();
$templates = array( 'index.twig' );

// index.twig

{% for post in posts %}
  <h2 class="h2"><a href="{{post.link}}">{{post.title}}</a></h2>
  <p>{{post.get_preview(25)}}</p>
  <p>id auteur post: {{ post.get_author.ID }}</p
  <p>Date de publication: {{ post.date }}</p>
  <p>Auteur: {{ post.author}}</p>
 {% endfor %}
 
 // multiple categories 
 
   <p>{{ post.terms('category') | join(', ') }}</p>
   
 // single categories 
 
 <p>{{ post.category }}</p>
 <p>{{ post.category.id }}</p>
 
```

<h3> Get the connected user </h3>

```
  // index.php

  $context['user'] = new TimberUser();
  
  // index.twig

  {{ user.id }} // display the user id
  {{ user.name }} //display the user name
```

<h3> Static variable </h3>

```
  // index.php
  
  $context['hello'] =  hello world;

  // index.twig
  
  {{hello}}
```

<h3> Add to twig</h3>

```
// function.php

add_filter('get_twig', 'add_to_twig');

function add_to_twig($twig) {
    $twig->addExtension(new Twig_Extension_StringLoader());
    $twig->addFilter(new Twig_SimpleFilter('whatever', 'my_whatever'));
    return $twig;
}

function my_whatever($text) {
    $text .= ' or whatever';
    return $text;
}

// index.twig

{{posts.title|whatever}} ==> dracula or whatever
```

<h3>Find post by the current author</h3>

```
// index.php

$args = array(
    'author'        =>  $current_user->ID,
    'orderby'       =>  'post_date',
    'order'         =>  'DESC',
    'posts_per_page' => -1
);

// 'posts_per_page' => -1 for all posts

// 'posts_per_page' => 7 for7 posts

$context['posts'] = Timber::get_posts( $args );
```

<h3>Find post by categ</h3>

```
// index.php

$args = array(
    'post_type' => 'post',
    'posts_per_page' => -1,
    'category_name'    => 'categorie_name',
    'orderby' => array(
        'date' => 'ASC'
    )
);
```
<h3>All parameters custom post type</h3>

```
$args = array(
	'posts_per_page'   => 5,
	'offset'           => 0,
	'category'         => '',
	'category_name'    => '',
	'orderby'          => 'date',
	'order'            => 'DESC',
	'include'          => '',
	'exclude'          => '',
	'meta_key'         => '',
	'meta_value'       => '',
	'post_type'        => 'post',
	'post_mime_type'   => '',
	'post_parent'      => '',
	'author'	   => '',
	'author_name'	   => '',
	'post_status'      => 'publish',
	'suppress_filters' => true 
);
```

<h3>Timber menu's</h3>

```
// function.php

add_theme_support('menus');

register_nav_menu('headerMenus','Main Menu');
register_nav_menu('footerMenus','Main Footer');

add_filter('timber_context', function ($context) {
    $context['headerMenus'] = new TimberMenu('headerMenus');
    $context['footerMenus'] = new TimberMenu('footerMenus');
    return $context;
});

// index .twig

{%  set_name_of_the_menu = footerMenus %}

<nav>
    <ul class="main-nav">
        {% for item in set_name_of_the_menu.get_items %}
            <li class="nav-main-item {{item.classes | join(' ')}}"><a class="nav-main-link" href="{{item.get_link}}">{{item.title}}</a>
                {% if item.get_children %}
                    <ul class="nav-drop">
                        {% for child in item.get_children %}
                            <li class="nav-drop-item"><a href="{{child.get_link}}">{{child.title}}</a></li>
                        {% endfor %}
                    </ul>
                {% endif %}
            </li>
        {% endfor %}
    </ul>
</nav>
```

<h3> Change dirname of the templates </h3>

```
// function.php

By default the name of the templates dir is 'templates'you can to change the name with this.

Timber::$dirname = 'views'; // in the current folder

or 

Timber::$dirname = '/folder/folder2/views'; // in the other folder

```

<h3> Load all settings </h3>

```
// function.php

Create a folder settings and put all files in this.

for example assets.php - menu.php - ...

// Load the settings.
foreach (glob(__DIR__ .'/settings{/,/*/}*.php', GLOB_BRACE) as $filename) {
    require_once $filename;
}
```

<h3> Load assets </h3>

```
// function.php

call_user_func(function() {
    $cssDir = get_stylesheet_directory_uri() . '/stylesheets';
    $jsDir = get_stylesheet_directory_uri() . '/scripts';

    // front
    add_action('wp_enqueue_scripts', function() use ($cssDir, $jsDir) {
        // Override the jQuery library
        wp_deregister_script('jquery');
        wp_register_script('jquery', $jsDir .'/vendors/jquery.js');

        // Stylesheets
        wp_enqueue_style('ilets-style', $cssDir . '/dist/main.css');

        // Scripts
        wp_enqueue_script('ilets-script', $jsDir . '/script.js',  array('jquery'));
    });
});
```
