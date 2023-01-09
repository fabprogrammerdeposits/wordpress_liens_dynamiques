# wordpress_liens_dynamiques

Le lien HTML en dur, source d’erreurs 404
Voici un lien en langage HTML :

<a href="https://www.oboqo.com/contact/">Contact</a>
 

L’attribut href correspond à l’URL du lien, tandis que le mot situé entre les balises <a></a>, appelé « ancre de lien », sera le texte cliquable. Si je veux par exemple créer un lien vers la page contact de mon site dans le footer de mon thème WordPress, je peux simplement y insérer cette ligne de code qui fonctionnera parfaitement.

Toutefois, mon lien en dur a ses limites : il suffit que je modifie l’URL de ma page ou que mon site change de nom de domaine pour que mes visiteurs rencontrent une erreur 404 en cliquant dessus (les robots des moteurs de recherche sont aussi concernés !).

Plutôt que de devoir corriger systématiquement tous les liens au moindre changement sur le site, mieux vaut anticiper et utiliser des liens dynamiques qui s’adapteront automatiquement à ces changements. C’est d’autant plus important lorsque l’on travaille sur un espace de développement et que le nom de domaine doit changer lors de la mise en ligne du site.

Qu’est-ce qu’un lien dynamique ?
Sur WordPress, un lien dynamique est un lien HTML classique généré par une fonction PHP qui va chercher automatiquement l’URL à afficher dans la base de données. Selon la ou les fonction(s) utilisée(s), on pourra aussi récupérer l’ancre de lien ainsi que d’autres informations à afficher dynamiquement dans notre balise <a>.

Le résultat dans le code source sera identique à notre premier exemple en HTML pour l’internaute et les moteurs de recherche, le PHP étant un langage serveur. A la différence du JavaScript qui est exécuté par le navigateur, le PHP est traité avant d’être envoyé au client (c’est-à-dire votre ordinateur ou smartphone, ou le robot d’indexation d’un moteur de recherche).

Votre navigateur reçoit donc une page HTML générée par le serveur, dans laquelle nos liens dynamiques apparaîtront en dur, comme si nous les avions écrits en HTML. C’est une précision importante pour le référencement, car les moteurs de recherche ne prennent pas en compte les liens dynamiques générés par JavaScript. Avec PHP, aucun problème à ce niveau, alors n’hésitez pas à les utiliser !

Un lien dynamique vers la page d’accueil avec bloginfo()
La fonction bloginfo() est l’une des plus utiles de la bibliothèque de fonctions PHP de WordPress. Il s’agit d’un marqueur de modèle, c’est-à-dire une instruction qui demande à WordPress de faire quelque chose ou de récupérer une information. On l’utilise avec un paramètre qui définit l’information à afficher (voir le Codex WordPress pour plus d’informations).

Dans notre cas, la fonction bloginfo() va être très utile pour afficher dynamiquement l’URL du site telle qu’elle est définie dans le back-office de WordPress (Réglages > Général > Adresse web du site).

Elle s’utilise de cette façon :

<a href="<?php bloginfo('url') ; ?>">Accueil</a>
Résultat dans le code source :

<a href="https://www.oboqo.com/">Accueil</a>
Pour faire un lien dynamique vers ma page contact, je pourrais donc utiliser le code suivant :

<a href="<?php bloginfo('url') ; ?>/contact/">Contact</a>
Si je change de nom de domaine, mon lien continuera de fonctionner grâce à cette méthode. En revanche, cela ne règle pas mon problème de modification d’URL, puisque la partie /contact/ est toujours écrite en dur. Il faut donc éviter d’utiliser la fonction bloginfo() pour des liens dynamiques autres que ceux renvoyant vers la page d’accueil du site.

La fonction get_permalink() pour les pages et articles
Pour faire un lien dynamique vers une page, un article ou un custom post, on utilise de préférence le marqueur de modèle get_permalink(). Il permet de chercher automatiquement l’URL d’un post (page ou article) dans la base de données à partir de son identifiant numérique. Cette fonction se contente de récupérer la valeur sans l’afficher, il faut donc utiliser echo avant la fonction pour pouvoir afficher l’URL dans le code source.

Exemple pour la page contact :

<a href="<?php echo get_permalink(27) ; ?>">Contact</a>
Résultat dans le code source :

<a href="https://www.oboqo.com/contact/">Contact</a>
L’utilisation de cette fonction permettra de changer de nom de domaine et/ou de modifier l’URL du post sans avoir à toucher au lien.

Astuce : pour récupérer l’identifiant numérique d’un post, allez sur sa page de modification dans le back-office. L’identifiant est visible dans l’URL affichée par votre navigateur (variable post).

Exemple : https://www.oboqo.com/wp-admin/post.php?post=27&action=edit

La fonction get_category_link() pour les catégories
Il existe un équivalent de get_permalink() pour les catégories : get_category_link(). Son fonctionnement est identique, il suffit d’utiliser l’identifiant numérique de la catégorie comme paramètre de la fonction.

Exemple pour une catégorie « Actualités » :

<a href="<?php echo get_category_link(5) ; ?>">Actualités</a>
Résultat dans le code source :

<a href="https://www.oboqo.com/actualites/">Actualités</a>
Afficher le titre d’une page ou d’un article avec get_the_title()
Maintenant que notre URL est entièrement dynamique, il nous reste encore à faire de même avec l’ancre de lien pour que celle-ci change automatiquement si le post est renommé.

Pour cela, il suffit d’utiliser la fonction get_the_title() avec l’identifiant du post en paramètre. WordPress va alors récupérer le titre du post dans la base de données. Comme pour get_permalink(), cette fonction doit être précédée d’un echo afin que le résultat soit affiché.

Exemple pour la page contact :

<a href="<?php echo get_permalink(27) ; ?>"><?php echo get_the_title(27) ; ?></a>
Résultat dans le code source :

<a href="https://www.oboqo.com/contact/">Contact</a>
Afficher le titre d’une catégorie avec get_cat_name()
Pour afficher le titre d’une catégorie, il existe une fonction similaire à get_the_title() : il s’agit de get_cat_name(). Elle s’utilise de la même façon, en lui donnant l’identifiant numérique de la catégorie comme paramètre.

Exemple pour une catégorie « Actualités » :

<a href="<?php echo get_category_link(5) ; ?>"><?php echo get_cat_name(5) ; ?></a>
Résultat dans le code source :

<a href="https://www.oboqo.com/actualites/">Actualités</a>
Une alternative pour l’ancre de lien : l’intitulé du fil d’Ariane de Yoast SEO
Sur un site bien optimisé pour le référencement, on a souvent deux versions de titre pour une même page : le titre court, que l’on retrouve dans les menus, l’URL de la page et le fil d’Ariane ; et le titre optimisé, souvent plus long, qui s’affiche dans la balise <h1> et que l’on renseigne dans le champ de titre principal de la page d’édition du back-office.

C’est ce deuxième titre qui est récupéré par la fonction get_the_title(). Or, dans la plupart des cas, on préférera utiliser la version courte pour une ancre de lien. Pour ce faire, il existe une astuce très pratique que vous pouvez utiliser si votre site possède un fil d’Ariane géré par l’extension Yoast SEO.

Concrètement, il s’agit de récupérer l’intitulé du fil d’Ariane, que l’on peut définir dans l’onglet « Avancé » de Yoast SEO. On utilise pour cela la fonction WPSEO_Meta ::get_value('bctitle', $id), où $id est à remplacer par l’identifiant de la page (echo doit être utilisé pour afficher le résultat).

Reprenons l’exemple de la page contact. Admettons que son titre soit « Nous contacter » et que l’intitulé du fil d’Ariane soit « Contact ».

Avec get_the_title :

<a href="<?php echo get_permalink(27) ; ?>"><?php echo get_the_title(27) ; ?></a>
Résultat dans le code source :

<a href="https://www.oboqo.com/contact/">Nous contacter</a>
Avec l’intitulé du fil d’Ariane de Yoast SEO (j’ai commenté chaque ligne pour une meilleure compréhension) :

<a href="<?php echo get_permalink(27) ; ?>">
  <?php 
  // On récupère le titre de la page dans une variable 
  $title $title = get_the_title(27) ; 
  // On vérifie si le fil d'Ariane de Yoast est activé 
  if (function_exists('yoast_breadcrumb')) { 
    // On récupère le contenu du champ "Intitulé du fil d'Ariane" 
    $title_breadcrumb = WPSEO_Meta ::get_value('bctitle', 27) ; 
    // On vérifie que le champ est rempli 
    if (!empty($title_breadcrumb)) { 
      // On remplace le contenu de la variable $title par l'intitulé du fil d'Ariane 
      $title = $title_breadcrumb ; 
    } 
  } 
  // On affiche le contenu de la variable $title echo $title ; ?>
</a>
Résultat dans le code source :

<a href="https://www.oboqo.com/contact/">Contact</a>
