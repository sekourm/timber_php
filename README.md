# timber_php ====> https://github.com/timber

This is an example for use initialize timber in a file

<h3> /** index.php */ </h3>

<pre>
$context = Timber::get_context();
$context['posts'] = Timber::get_posts();
$templates = array( 'index.twig' );
</pre>

<h3> /** index.twig */ </h3>

```
{% for post in posts %}
  <h2 class="h2"><a href="{{post.link}}">{{post.title}}</a></h2>
  <p>{{post.get_preview(25)}}</p>
  <p>id auteur post: {{ post.get_author.ID }}</p
  <p>Date de publication: {{ post.date }}</p>
  <p>Auteur: {{ post.author}}</p>
 {% endfor %}
```

<h3> /** get the connected user in index.php */ </h3>

<pre>
  $context['users'] = new TimberUser();
</pre>

<h3> /** get the connected user in index.twig */ </h3>

```
  {{ users.id }}
```

<h3> /** how to declare variable  with a static value in index.php */ </h3>

<pre>
  $context['toto'] = je suis toto ;
</pre>

<h3>/** index.twig */</h3>

```
  {{toto}}
```

<h3>/** add to twig , in file function.php */</h3>

```
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
```

<h3>/** index.twig */</h3>

```
{{posts.title|whatever}} ==> je suis le titre or whatever
```

<h3>/** find post by the current author and display in index.twig */</h3>

```
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

