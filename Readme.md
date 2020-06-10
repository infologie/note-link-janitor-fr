# note-link-janitor-fr

`note-link-janitor` est un script créé par Andy Matuschak qui lit un dossier de fichiers Markdown, relève tous les [[liens de type wiki]] entre ces fichiers, puis ajoute une section spéciale intitulée "Backlinks" qui liste les passages faisant référence à un fichier donné.

Ce dépôt offre une traduction de la documentation du script, ainsi qu'une localisation française sous la forme d'une variante intitulée `note-link-janitor-fr`.

Le principe du script est le suivant. Dans un fichier `Note A.md`, le texte suivant sera ajouté :

```
## Backlinks
- [[Note B]]
    - Un passage de la note B qui contient un lien vers [[Note A]].
    - Un autre passage de la même note B qui renvoie également vers [[Note A]].
- [[Note C]]
    - Un passage d'une autre note qui renvoie vers [[Note A]].
```

Le script est idempotent ; lorsqu'il est exécuté à nouveau, _il met à jour les rétroliens sur place_.

La section contenant les rétroliens est insérée à la fin du fichier. Si votre note se termine par un bloc de commentaire HTML `<!-- -->`, les rétroliens sont insérés avant.

## Postulats/avertissements

1. Les liens sont formatés `[[comme ceci]]`.
2. Les titres des notes sont déduits de la première ligne de chaque note, qui est supposée être formatée comme un titre, c'est-à-dire `# Titre de la note`.
3. Tous les fichiers `.md` sont considérés comme étant au même niveau ; dans sa version actuelle, le script ne parcourt pas récursivement une arborescence de fichiers (bien que ce soit une modification assez simple à réaliser si vous en avez besoin ; voir `lib/readAllNotes.ts`)
4. La section des rétroliens est définie comme l'intervalle AST entre `## Backlinks` et la balise d'en-tête suivante (ou bien `<!-- -->`). Tout texte que vous pourriez ajouter à cette section sera supprimé. N'ajoutez pas de texte après la liste des rétroliens sans intervertir un titre entre les deux !

### *FYI-style open source*

Ce script est partagé sans aucun engagement de maintenance. Les signalements de bugs et les *pull requests* seront ignorées ou fermées sans commentaire. C'est valable pour le dépôt d'origine, ainsi que pour cette traduction. Toutefois, si vous faites quelque chose d'intéressant avec ce script, [faites-le savoir à son auteur](mailto:andy@andymatuschak.org).

## Utiliser la variante francophone

Sur ce dépôt, j'ai modifié le fichier `updateBacklinks.ts` pour traduire le titre de la section `## Backlinks` en `## Rétroliens`. J'ai également changé le marqueur de liste, à l'origine l'astérisque, en tiret.

Pour installer cette variante :

```
yarn global add @arthurperret/note-link-janitor-fr
```

Ensuite, pour l'exécuter :

```
note-link-janitor-fr chemin/vers/le/dossier
```

## Utiliser du script d'origine

Pour installer le script d'Andy Matuschak :

```
yarn global add @andymatuschak/note-link-janitor
```

Ensuite, pour l'exécuter (attention, cela modifiera vos fichiers `.md` *sur place* ; pensez à faire des sauvegardes au cas où !):

```
note-link-janitor chemin/vers/le/dossier
```

Ceci exécute le script une fois. Pour l'exécuter de manière régulière, vous devrez créer une tâche planifiée avec `cron` ou autre service de type daemon.

Le script est conçu pour fonctionner avec Node >=12, vous devrez donc peut-être mettre à jour votre version d'exécution ou la remplacer.

## Fabriquer une copie locale

Clonez [le dépôt d'Andy Matuschak](https://github.com/andymatuschak/note-link-janitor). Installez [Yarn](https://classic.yarnpkg.com/fr/docs/install/). Lancez les commandes suivantes :

```
yarn install
yarn run build
```

## Projets

Surveillez le dépôt d'origine, Andy Matuschak ayant exprimé l'intention d'étendre ce projet au suivi des liens morts ou orphelins, ainsi qu'à d'autres phénomènes hypertextuels.
