# timber_php

this is an example for use initialize timber in a file

/** index.php **/

<pre>
$context = Timber::get_context();
$context['posts'] = Timber::get_posts();
$templates = array( 'index.twig' );
</pre>

/** index.twig **/

'
{% for post in posts %}
  <h2 class="h2"><a href="{{post.link}}">{{post.title}}</a></h2>
  <p>{{post.get_preview(25)}}</p>
  <p>Date de publication: {{ post.date }}</p>
  <p>Auteur: {{ post.author}}</p>
 {% endfor %}
'
