# Website for API Entreprise

## Local serve
`bundle exec jekyll serve`

## PDF version
The PDF is generated by mina with Pandoc (+texlive, +xelatex) with a command like this:

`pandoc cgu.md -o cgu.pdf --latex-engine=xelatex`

A rake task is available :

`bundle exec rake cgu_to_pdf`

So it need to have installed:

- pandoc
- texlive
- texlive-xetex

## Deploy
`bundle exec mina deploy to=[sandbox|production]

-----
-----
-----
----
### Informations tirées du SIADE, à remettre à jour => Concerne la rédaction de contenu pour un public non tech

# API Entreprise
Le site vitrine d'API Entreprise est disponible à l'adresse https://entreprise.api.gouv.fr.

Il s'agit d'un site statique en Jekyll dont les sources sont hostées sur [Github](https://github.com/etalab/entreprise.api.gouv.fr).

## Aspects purement techniques 
En production, le site est servi à la racine du domaine entreprise.api.gouv.fr.

En *sandbox*, sur Github Pages (plus d'information sur l'utilisation de Github Pages [ci-dessous](#environnement-de-sandbox)), le site est servi à l'adresse https://etalab.github.io/entreprise.api.gouv.fr/. La branche source pour Github Pages est *gh-pages*.

Pour que le site soit fonctionnel dans les deux environnements il est nécessaire que la résolution des URLs soit indépendante de la présence ou non d'un préfixe d'URL. Pour cela, l'option `baseurl` dans le fichier `_config.yml`, qui permet de spécifier un préfixe d'URL, est évaluée à "/entreprise.api.gouv.fr" pour que  le site fonctionne sur Github Pages. Pour que le site soit servi directement à la racine du domaine, le site est *build* avec l'option `--baseurl ''` fournie à la ligne de commande et qui efface le préfixe d'URL.

## Édition du contenu

### Environnement de *sandbox*
Il est indispensable que des profils non techniques puissent éditer du contenu sur le site statique d'API Entreprise, cela est même rédhibitoire quant au choix de la solution. Le site vitrine historiquement fait en Jekyll répond au besoin à deux pré-requis près : 
* utilisation de Markdown (pas vraiment technique, et c'est aujourd'hui un standard pour éditer du contenu *Web based*) ; 
* utilisation de Github (possible depuis l'interface et donc sans utiliser directement *Git* en ligne de commande).

Le gros avantage de Github ici c'est Github Pages car nous n'avons pas d'intégration continue en place à ce jour. Github Pages affichera automatiquement le résultat des changements appliqués poussés sur la branche `gh-pages`. Une version à jour de cette branche sera donc disponible à la visualisation à l'adresse https://etalab.github.io/entreprise.api.gouv.fr/.

**Qu'est-ce que cela veut dire concrètement pour les équipes fonctionnelles ?**

*  Ce n'est pas la peine d'ouvrir des PR aux équipes techniques pour demander une validation pour merge dans `gh-pages`. Cela évite une sursoliciation des équipes techniques, favorise l'indépendance des équipes métier et surtout il n'y a aucun risque : Si tout est cassé, ce n'est pas grave car ce n'est pas en production, et grâce à Github Pages, vous pouvez être presque entièrement autonomes pour "réparer".
*  les seuls moments ou vous avez besoin de l'aval de l'équipe technique, c'est pour demander à déployer en production
*  les seuls Pull Request (PR) de merge dans `master` viennent de `gh-pages`. Pourquoi ? Déjà parce que vous avez vu ce que cela donne en sandbox, vérifiez que ça ne cassait rien, et validez entre vous que c'était prêt pour la prod. Ensuite, pour l'équipe technique, cela permet de simplement aller voir en sandbox (et c'est bien plus agréable que via le diff dans github) avant de déployer. 

##### En résumé 
1. La branche de *sandbox* est **gh-pages**. Les modifications doivent être appliquées sur cette branche pour être automatiquement visualisées grâce à Github Pages.
2. Les changements sont automatiquement visibles à l'URL **https://etalab.github.io/entreprise.api.gouv.fr/**. Il peut nécessiter quelques minutes avant que les modifications soient visibles après raffraichissement. De manière générale le site est petit et "compilé" rapidement, moins d'une minute est constatée avant que les changements soient pris en compte.

### Ajouter des éléments
Le *templating* du site Jekyll est en place pour que l'ajout d'élément de contenu au site web soit simple : uniquement en Markdown et avec le minimum de configuration. La procédure d'ajout de contenu est détaillée ci-après. Aujourd'hui, il est possible d'éditer trois types d'éléments : 
* documenter des [cas d'usage](#editer-les-cas-dusage) d'API Entreprise ; 
* ajouter des [fournisseurs de données](#editer-les-fournisseurs-de-données) ; 
* documenter les [données disponibles](#documenter-les-données-disponibles).

Exemple de rendu pour le contenu suivant : 

```markdown
---
layout: article
title: Mon super titre !
---

## Je veux parler de ça

### Premier sous titre
Wow sympa, tu sais que tu peux aussi écrire en **gras**.

### Deuxième sous titre
En *italique* ça marche aussi, il y a aussi pleins de trucs et astuces :

1. truc 1
2. astuce 2

* en fait une liste sans ordre c'est bien aussi
* non ?

On peut même ajouter des [liens](https://www.markdownguide.org/cheat-sheet/) vers la documentation de la syntaxe de Markdown !
```

![Capture_d_écran_2019-10-18_à_14.39.48](uploads/92bb28fda2c65fd387569a25eb2e25fe/Capture_d_écran_2019-10-18_à_14.39.48.png)

Plus de détails au cas par cas ci-dessous !

#### Editer les cas d'usage
Une liste des cas d'usage est disponible à la page "Cas d'usage" accessible depuis l'en-tête du site ou à l'url `/cas_usage/`. Cette page ne fait que lister les cas d'usage documentés, chaque élément de la liste n'étant qu'un lien hypertexte vers la page de description du cas d'usage en question. 

![Capture_d_écran_2019-10-18_à_14.46.07](uploads/6144f13ab9be17ba70791b779ab37ab5/Capture_d_écran_2019-10-18_à_14.46.07.png)

##### Fichiers sources 
Pour ajouter un cas d'usage il suffit de créer un nouveau fichier avec dans le dossier `_use_cases`, par exemple `_use_cases/marches_publics.md`. Le nom du fichier n'a aucune incidence sur le rendu mais il est préférable de rester cohérent pour identifier facilement les différents éléments, par contre il est important que l'extension soit bien en Markdown `.md`.

##### Configuration 
Pour un cas d'usage, les éléments de configuration suivant sont nécessaires. Il doivent être placée tout en haut du fichier, en en-tête : 

```yaml
--- 
layout: article
title: Mon cas d'usage
--- 
```

* **layout** : il est nécessaire d'indiquer le layout, "article", pour que Jekyll sache quel style appliquer à la page où sera détaillé le cas d'usage. Il faut indiquer le même pour tous les cas d'usage pour assurer une cohérence de présentation entre tous.
* **title** : indiquer le titre du cas d'usage. C'est cette valeur qui sera utilisé dans la liste de la page "Cas d'usage" (voire la capture d'écran ci-dessus) ; elle sera aussi disponible dans l'en-tête de la page du cas d'usage.

#### Editer les fournisseurs de données
Les fournisseurs de données partenaires d'API Entreprise sont listés à la page "Données disponibles", à l'adresse `/donnees_disponibles/`. Cette page référence la liste de tous les fournisseurs de données qui ont une page d'information dans les sources. Cette page est aussi le point d'entré pour les données : pour chaque fournisseur sont listées les liens vers les pages de documentation des données mises à disposition. Plus sur l'édition du contenu des données [ci-dessous](#documenter-les-données-disponibles)

![Capture_d_écran_2019-10-18_à_15.02.14](uploads/5d5038de86951eada812ccdbfbaa1441/Capture_d_écran_2019-10-18_à_15.02.14.png)

##### Fichiers sources
Pour ajouter un fournisseurs de données il suffit de créer un nouveau fichier avec dans le dossier `_providers`, par exemple `_providers/ademe.md`. Le nom du fichier n'a aucune incidence sur le rendu mais il est préférable de rester cohérent pour identifier facilement les différents éléments, par contre il est important que l'extension soit bien en Markdown `.md`.

Pour ajouter un logo à côté du texte à côté du texte descriptif du fournisseur, il faut télécharger l'image et la placer dans le dossier `assets/images/providers/` à côté de celles déjà présentes.

##### Configuration
Pour un fournisseur de données, les éléments de configuration suivant sont nécessaires. Il doivent être placée tout en haut du fichier, en en-tête : 

```yaml
---
title: Mon fournisseur
label: fournisseur
logo: "/assets/images/providers/logo.png"
sources_url: https://www.fournisseur.fr/lien/vers/une/éventuelle/documentation/fournisseur
---
```

* **title** : généralement le nom du fournisseur, le "title" est utilisé comme sous-titre de section du fournisseur sur la page "Données disponibles" ; 
* **label** : la valeur est au choix ! Cette information n'a aucune incidence sur le rendu final, elle ne sert qu'à associer la page de description d'une donnée à son fournisseur (voire la [configuration des fichiers de description des données](#configuration-2));
* **logo** : chemin vers le logo du fournisseur ;
* **sources_url** : optionel, lien vers une page d'information en ligne, généralement sur le site du fournisseur lui même.

#### Documenter les données disponibles
Les données disponibles sont listées, par fournisseurs, à la page "Données disponibles" (adresse `/donnees_disponibles/`. Cette liste n'est constituée que de liens hypertextes vers les pages de description des données.

##### Fichiers sources 
Pour ajouter une description d'une donnée il suffit de créer un nouveau fichier avec dans le dossier `_available_data`, par exemple `_available_data/ma_donnee.md`. Le nom du fichier n'a aucune incidence sur le rendu mais il est préférable de rester cohérent pour identifier facilement les différents éléments, par contre il est important que l'extension soit bien en Markdown `.md`.

##### Configuration
Pour une description de données, les éléments de configuration suivant sont nécessaires. Il doivent être placée tout en haut du fichier, en en-tête : 

```yaml
---
layout: article
title: Certificat RGE
provider_label: ademe
---
```

* **layout** : il est nécessaire d'indiquer le layout, "article", pour que Jekyll sache quel style appliquer à la page où sera détaillé le cas d'usage. Il faut indiquer le même pour tous les cas d'usage pour assurer une cohérence de présentation entre tous ; 
* **title** : le titre de la donnée. C'est cette valeur qui est utilisée dans les listes des données disponibles par fournisseurs à la page "Données disponibles" ; elle sera aussi disponible dans l'en-tête de la page de description de la dite donnée.
* **provider_label** : il faut renseigner ici le *label* du fournisseur qui met à disposition la donnée. Cette information est renseignée pour chaque fournisseur (voire la [configuration des fournisseurs](#configuration-1)). Cette information permet de lister les données disponibles sous la description de leur fournisseur sur la page "Données Disponibles".



