#  Installation custom de Wordpress

## procédure d'installation:

1.
initialiser le projet avec `composer init`=> On valide avec entrée tout du long sauf pour les questions.

```
Would you like to define your dependencies (require) interactively [yes]? no
Would you like to define your dev dependencies (require-dev) interactively [yes]? no
```

ce `composer init` crée notre `composer.json` 

2.
on demande à composer d'installer le code de wordpress dans un répertoire `wp` (pas obligatoire)
On définit une option dans le fichier composer.json pour gérer (si on veut le faire )le nom du dossier dans lequel on installera wordpress
installer le package _johnpbloch/wordpress_ : `composer require johnpbloch/wordpress`

warning: il ne faut pas oublier d'ajouter au .gitignore (ou de l créer)

3. on récupère index.php et wp-config-sample.php depuis les fichiers de wordpress(installés dans le répertoire wp). Il faut adapter le chemin du require présent dans index.php : `'require _DIR_ ./wp/'`

```
vendor
wp
```
4. on ajoute nos identifiants de base de données dans une ***copie*** du fichier `wp-config.sample`  que l'on nomme `wp-config.php`!

On a maintenant accès à un formulaire nous permettant d'installer wp (ne pas utiliser)

5. Config custom :

 On ajoute des constantes à notre wp-config

 ```
// je définis l'url vers la page d'accueil de mon site 
define(
'WP_HOME',
rtrim('put_your_home_url_here','/')
);

//je définis l'url vers le dossier source de wordpress
define(
	'WP_SITEURL',
	WP_HOME .'/wp'
	);
//je définis l'url vers le dossier wp-content
define(
	'WP_CONTENT_URL',
	WP_HOME .'/wp-content'
	);

//je définis le path (chemein côté serveur ) vers le dossier wp-content
define(
	'WP_CONTENT_DIR',
	__DIR__ .'/wp-content'
	);
		
/* Add any custom values between this line and the "stop editing" line. */



/* That's all, stop editing! Happy publishing. */

/** Absolute path to the WordPress directory. */
if ( ! defined( 'ABSPATH' ) ) {
	define( 'ABSPATH', __DIR__ . '/' );
}

 ```

 6. Installation de wp 

 on utilise WP-CLI  pour installer wp en  LDC 

 `wp core install --prompt ` => démarrer l'installation en mode intéractif
 La commande nous demande des infos :

 ```
1/6 --url=<url>: http://localhost/template-wordpress
2/6 --title=<site-title>: e-blog
3/6 --admin_user=<username>: xxx
4/6 [--admin_password=<password>]: xxxx
5/6 --admin_email=<email>: salut@truc.com
6/6 [--skip-email] (Y/n): valider par entrée 
 ``` 
 si succès on aura la réponse  :

 `Success: WordPress installed successfully.`

 7. on utilise wppackagist avec Composer pour récupérer des thèmes et des plugins dans composer.json 

 ```
  "repositories":[
        {
            "type":"composer",
            "url":"https://wpackagist.org",
            "only": [
                "wpackagist-plugin/*",
                "wpackagist-theme/*"
            ]
        }
    ],
```
pour installer un thème  : `composer require wpackagist-theme/twentytwenty`

pour installer un plugin : `composer require wpackagist-plugin/akismet `

Attention , quand on installe un plugin, il faut l'activer (avec wp-cli) 

`wp plugin activate[nomduplugin]`# wp-cli-template
