# Initiation à Git

## Qu'est-ce que Git ?

Git est un outil permettant de garder l'historique de fichiers amenés à évoluer dans le temps. C'est ce qu'on appelle un gestionnaire de versions (VCS = _version control software_ = logiciel de contrôle de versions).

On l'initialise sur un dossier, on indique les fichiers / dossiers à ne pas garder (souvent générés, ou sensibles, contenant des secrets), et on "photographie" l'état initial, sauvant les fichiers dans un dépôt (_repository_).

Puis on travaille sur les fichiers de ce dossier, généralement des fichiers texte (souvent du code source), même si Git sait gérer des fichiers binaires (mais de façon moins efficace).

Quand on atteint un état qu'on veut garder (généralement une étape stable), on peut enregistrer cet état (_commit_). En fait, Git voit quels fichiers ont été modifiés et ne fait rien pour ceux qui n'ont pas changé. Il fait la différence (cherchant les lignes modifiées) entre les fichiers modifiés, et leur version précédente. C'est cette différence qu'il enregistre, minimisant la taille des enregistrements, comme en général peu de lignes sont changées.

Bien sûr, les nouveaux fichiers sont enregistrés intégralement, ceux supprimés sont retirés (avec leur dernier contenu conservé dans l'historique, bien entendu), et Git peut, dans une certaine mesure, traquer les renommages de fichiers s'ils sont peu modifiés en même temps.

En fait, Git n'est pas qu'un VCS, c'est un DVCS, où le D indique qu'il est distribué : il permet de partager les dépôts, les historiques, via un réseau (Internet en général), permettant un travail d'équipe efficace.

## Les concepts fondamentaux

### Zones

Pour faire son travail, Git distingue des "zones" de fichiers.

On a l'espace de travail (_working directory_), qui est le dossier (et ses sous-dossiers) où on modifie les fichiers.

On peut mettre tout ou partie des fichiers modifiés dans une zone de transit (_staging area_ aussi appelée _index_), qui dit à Git « ces fichiers-là m'intéressent particulièrement, à l'heure actuelle ».

On les passe de l'espace de travail à la zone de transit avec la commande `git add`, qui peut prendre une liste de fichiers et dossiers (le contenu des dossiers est ajouté récursivement).

Enfin, on fait passer les fichiers dans le dépôt (_repository_) via la commande `git commit` qui demande un message de commit, permettant de décrire ce qui a été fait dans cette session.

La plupart du temps, on fait un "commit" de tous les fichiers modifiés, mais parfois on peut être plus sélectif, par exemple quand on se dit qu'une certaine partie des modifications concernent plus un correctif que la fonctionnalité en cours, et qu'on veut l'isoler, ou si on a mélangé une fonctionnalité et un remaniement (_refactoring_) du code, ou isoler une correction d'une autre, etc. D'où l'intérêt de la zone de transit.

La dernière zone est sur un serveur spécialisé, c'est le dépôt distant (_remote repository_). On envoie le (ou les) commits effectués dernièrement sur le dépôt distant via la commande `git push`. On peut récupérer les travaux des autres avec `git pull` (ou `git fetch`).

### Branches

Un autre concept important de Git est le concept de branches, flexible et puissant.

Le projet vit sur une branche principale, appelée _master_ (maître). Quand on créé une nouvelle branche, les travaux (_commits_) y sont isolés, et peuvent évoluer en parallèle des autres branches. En fait, s'ils n'aboutissent pas, on peut simplement supprimer la branche et les autres branches (dont la branche maître) restent intactes.

Les branches sont généralement faites depuis _master_, mais on peut aussi en faire depuis une autre branche.

On peut aussi utiliser une branche pour marquer un point dans l'histoire du projet, une version stable, de référence. Cette branche peut évoluer légèrement, par exemple pour intégrer des correctifs : cela permet de faire vivre une version spécifique, par exemple livrée chez un client, sans bloquer le développement de nouvelles fonctionnalités.

Souvent, quand on veut travailler sur une fonctionnalité ou un correctif particulier, on créé une nouvelle branche. Elle part généralement depuis la dernière version de _master_, ou depuis une branche de maintenance. Sur cette branche, le développeur peut faire un ou plusieurs commits. Le codeur peut abandonner provisoirement un travail long sur une branche de fonctionnalité, et revenir sur _master_, pour faire une nouvelle branche pour corriger un bug urgent. Le travail en cours n'est pas affecté et l'attend.

De même, chaque développeur travaille sur sa propre branche, ce qui évite d'interférer avec le travail des autres.

Pour les branches de correctif ou d'évolution, quand le travail est fini, on intègre les changements dans _master_, faisant évoluer le logiciel. Cela s'appelle une fusion (_merge_) des branches. Si deux développeurs ont travaillé sur le même fichier, il peut y avoir des conflits (modifications sur les mêmes fichiers) qui peuvent être résolus de façon efficace.

## Réglages initiaux

Chaque commit porte l'identification de la personne qui l'a fait, via son e-mail, qu'on renseigne dans un fichier de configuration global. Une façon rapide de le renseigner est d'utiliser la commande : `git config --global user.email "your.name@example.com"`. On peut aussi renseigner `user.name` par exemple, et d'autres réglages comme spécifier un éditeur de texte, les outils de fusion, ou les options de fichier (`i18n.filesEncoding "utf-8"` et `core.autocrlf "input"` sont recommandés).

Cela créé un fichier `.gitconfig` à la racine du répertoire utilisateur (par exemple `C:\Users\<your.name>\.gitconfig` sous Windows), on peut le modifier avec un éditeur de texte pour ajouter ou modifier des options.

## Initialiser ou cloner

Quand on créé un nouveau projet, dans un répertoire dédié, on l'initialise avec la commande `git init`. Cela créé un répertoire `.git` dans le dossier courant, qui contiendra le dépôt.

Dans un projet, il y a parfois des fichiers qu'on ne veut pas mettre dans un dépôt. Généralement, ce sont des fichiers générés ou téléchargés, faciles à remettre dans le dossier de travail. Par exemple, le dossier `node_modules` contenant les bibliothèques utilisées dans un projet JavaScript, le dossier `bin` (ou `dist` ou autre nom), contenant le projet compilé, etc.

On peut dire à Git d'ignorer ces fichiers, en mettant leur nom ou chemin dans un fichier nommé `.gitignore`.

Si on veut travailler sur un projet existant, hébergé sur un serveur (GitHub, GitLab, etc.), on doit en faire un clone sur l'ordinateur local : `git clone git@git-manager.io:organization-name/project-name.git`. Cela va créer un dossier `project-name` dans le répertoire courant.

C'est la syntaxe SSH, on peut aussi utiliser la syntaxe HTTPS : `git clone https://git-manager.io/organization-name/project-name.git`.

Il faut avoir un compte sur le serveur et être connecté pour que ça fonctionne.
De plus, il faut générer et récupérer une clé SSH et la mettre dans sa configuration utilisateur.
Sous Windows, il faut utiliser l'outil Pageant (faisant partie de l'outil PuTTY) pour gérer la liaison SSH.

## Voir l'état du dépôt

À noter qu'en général, les commandes Git sont lancées depuis la racine du dossier de projet. Après avoir cloné un projet, on doit se déplacer dedans (`cd project-name`).

```sh
> git status
On branch The_branch_name
Your branch is up to date with 'origin/The_branch_name'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   src/app/shared/list/modified-file.ts

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    src/app/shared/list/deleted-file.ts
        modified:   src/app/shared/list/other-modified-file.ts

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        src/app/shared/list/new-file.ts
```
Comme vous voyez, Git rappelle les commandes essentielles pour manipuler les fichiers.
On peut avoir un listing moins verbeux, avec information sur la branche en cours :
```sh
> git status --short --branch
## The_branch_name
 D src/app/shared/list/deleted-file.ts
M  src/app/shared/list/modified-file.ts
 M src/app/shared/list/other-modified-file.ts
?? src/app/shared/list/new-file.ts
```

Note : c'est un tutoriel sur Git, l'outil en ligne de commande. Je me concentre donc sur les commandes, qu'il est bon de connaître. Mais pour un usage au quotidien, je recommande d'utiliser une interface utilisateur graphique, qui permet une meilleure visualisation de l'historique (commits, branches, différences) et facilite les opérations comme les clones, commits, pull et push, etc.

Il y a de nombreux logiciels de ce type, dont beaucoup de gratuits. À commencer par Visual Studio Code (et autres IDE) qui a une gestion basique de Git qui peut être améliorée par des extensions.

Pour ma part, j'utilise [Git Extensions](https://gitextensions.github.io/), outil Windows mais qui peut être utilisé sur Linux avec Mono. Il y a aussi [GitAhead](https://gitahead.github.io/gitahead.com/) qui semble intéressant (et multiplateforme).

## Faire un commit

Littéralement, _to commit_ = s'engager. L'opération peut être décrite comme "figer l'état des fichiers" à un instant donné. On va garder ici le terme anglais, plus concis, et plus courant.

### Préparation

Comme on a vu, pour faire un commit il faut commencer par mettre dans la zone de transit les fichiers dont on veut figer l'état avec `git add` suivi d'une liste de fichiers ou dossiers, les dossiers ayant leurs modifications ajoutées de façon récursive. Donc `git add .` (`.` signifiant "le dossier où je suis") peut suffire.

Si on a ajouté des fichiers qu'on ne veut pas commettre tout de suite, on peut les retirer de la zone de transit avec la commande `git restore --staged <chemin du fichier>` ou  `git reset <chemin du fichier>` . Sans nom de fichier, la dernière commande déplace tous les fichiers hors de l'index, dans la zone de travail.

Si on veut entièrement supprimer les modifications sur un fichier, on peut utiliser la commande `git restore --worktree <chemin du fichier>` ou alternativement, `git checkout -- <chemin du fichier>`.

On utilise `git status` pour s'assurer que l'on a bien tous les fichiers qu'on veut. Il est recommandé d'inspecter les différences introduites une dernière fois avant de faire le commit, avec `git diff`. Mais c'est plus facile avec une interface graphique.

### Message de commit

Enfin, on fait le commit à proprement parler avec la commande `git commit`. Git exige un message associé à ce commit. Cela permet de les distinguer, de savoir, dans l'historique, à quoi correspond chaque commit. C'est de la documentation sur l'évolution du projet.

Si on ne le précise pas sur la ligne de commande, Git va ouvrir un éditeur de texte pour permettre la saisie du message. Par défaut, c'est un éditeur primitif (_vi_) intégré, mais on peut spécifier l'éditeur qu'on veut, par exemple VSC, avec la commande `git config --global core.editor "code --wait"`. La partie entre guillemets change selon l'éditeur, on peut avoir besoin de spécifier le chemin vers l'éditeur, les options peuvent varier, etc.

Un message a au moins une ligne de texte, expliquant succinctement le but du commit. Cette ligne doit être gardée courte, autour de 80 à 100 caractères, parce qu'elle sera listée par les commandes d'historique.

On peut aussi ajouter, après une ligne vide, plusieurs lignes précisant des informations sur le commit : ticket corrigé, choix techniques, détail par fichier / fonctionnalité / bug fixé, etc.

Si le message ne comporte qu'une ligne (cas le plus fréquent), on peut le rédiger directement sur la ligne de commande sans passer par un éditeur : `git commit -m "Fix the disappearing line after modification"` par exemple.

### Conventions de message

Par convention, parce qu'un projet peut être ou devenir international, on rédige les messages en anglais, _lingua franca_ de l'informatique… Si c'est un projet purement personnel, ou si le projet a une politique différente, on peut écrire en français (ou une autre langue).

Une convention appliquée presque universellement est de toujours commencer un message de commit par un verbe à l'infinitif : "Fix this", "Update that", "Remove this", "Refactor these", "Move the files", etc.

Une convention qui se répand est d'utiliser un format appelé "[_conventional commit_](https://www.conventionalcommits.org/)", qui permet de classer les commits en catégories et portées. On garde le message commençant par un verbe, mais on le préfixe obligatoirement par une catégorie : `fix` ou `feat`, selon qu'on corrige un bug (_fix_) ou qu'on ajoute une fonctionnalité (_feature_).

On peut avoir d'autres catégories, les plus courantes sont : `chore` (tâche de maintenance sans impact fonctionnel, comme mettre à jour les dépendances), `refactor` (amélioration du code sans impact fonctionnel), `docs` (amélioration de la documentation, des commentaires), `test` (ajout / modification de tests), etc.

Les deux premières catégories (fix et feat) sont listées dans un fichier CHANGELOG (journal des modifications) parce qu'elles ont un impact sur les utilisateurs, et peuvent être mentionnées dans une livraison de version (_release_), etc. Les autres ne sont pas (normalement) visibles par l'utilisateur final, et ne sont pas listées, mais sont intéressantes pour les développeurs pour voir en un coup d'œil le genre de modification effectué.

La catégorie peut optionnellement être suivie par la portée de la modification, entre parenthèses, indiquant succinctement la fonctionnalité impactée, l'endroit du code qui a été touché.

Par exemple, on peut avoir : _fix(share): correct behavior of share option_, _feat(file-manager): allow to rename a file_, etc.
La portée est listée dans le _changelog_ qui regroupe, sous une version, les corrections et les nouvelles fonctionnalités, et pour chaque section, les groupe par portée.

Par exemple :
```
# [1.3.0](https://oogit.oodrive.net/postfiles/sharing-frontend/compare/v1.2.0...v1.3.0) (2021-04-30)

### Bug Fixes

* **access:** add MULTILINK right ([0439684](https://oogit.oodrive.net/postfiles/sharing-frontend/commits/043968408a2cf68308235f8701ad461b5b5674a7))
* **applications:** manage the Outlook plugin as sole app ([d97dfe0](https://oogit.oodrive.net/postfiles/sharing-frontend/commits/d97dfe0b2c6a4a27cc15f2f970d0c90534d7b9ad))
* **business-data-profile:** fix focus bug in modal ([088c2f9](https://oogit.oodrive.net/postfiles/sharing-frontend/commits/088c2f969287cafddb4d1d4fe43b910fba21ed2d))
* **business-data-profile:** improve management of empty values ([3c7d451](https://oogit.oodrive.net/postfiles/sharing-frontend/commits/3c7d451bb446c9d5571437c7f5d0e13f706b7d0d))
* **business-data-profile:** repair deletion of field ([8fab297](https://oogit.oodrive.net/postfiles/sharing-frontend/commits/8fab297aa014281f30170365073414c1d1306ad1))
[...]

### Features

* **actions:** show long action if needed ([39d477a](https://oogit.oodrive.net/postfiles/sharing-frontend/commits/39d477aa3f14909e6aa4476426f38b9bd0fb7769))
* **business-data:** manage BD profile data input ([c9755da](https://oogit.oodrive.net/postfiles/sharing-frontend/commits/c9755da2c5fcea0aa9e1c5e716d8c660f98b59eb))
* **business-data:** show business data information in folder details ([7f0b57e](https://oogit.oodrive.net/postfiles/sharing-frontend/commits/7f0b57ebd5c8f17977b8eaf725ba53202f13769d))
[...]
```
Note : ce genre de listing est généré par un outil Node.js spécifiquement, ce n'est pas Git qui le créé.

## Voir l'historique

Là encore, une bonne interface graphique est préférable, pour voir les branches parallèles, les fusions, etc.
Néanmoins, à défaut, la commande `git log` permet de voir les derniers commits, par lots. Taper sur la barre d'espace pour voir la suite, sur la touche q pour quitter ce listing.

La commande `git log --oneline --graph` montre l'historique de façon plus synthétique.
Une alternative est la commande `git gui`, intégrée à l'outil, qui offre une interface graphique primitive pour inspecter les commits.

## Créer une branche

Comme on l'a vu, on peut créer une branche pour isoler des modifications de la branche principale (master), et pour travailler en parallèle des autres développeurs du dépôt. À faire avant le premier commit !

On utilise pour cela la commande `git branch "nom_de_la_branche"`.

Après création, on peut basculer sur cette branche, les commits se feront dans la branche active : `git checkout "nom_de_la_branche"`. Il est possible de combiner création et basculement, avec `git checkout -b "nom_de_la_branche"`.

On peut supprimer une branche avec `git branch -d "nom_de_la_branche"`. S'il y a des commits sur la branche, il faudra forcer la suppression avec l'option `-f` (ou `--force`). On ne peut pas supprimer la branche active, il faut basculer sur une autre branche auparavant.

## Pousser des changements

### Créer une liaison avec un dépôt distant

Si on a cloné un dépôt, la liaison avec ce dépôt distant (_remote_) est créée automatiquement. Si on a créé le dépôt local avec `git init`, il faut déclarer le dépôt distant (_upstream_) créé au préalable sur GitLab, GitHub ou autre gestionnaire similaire.

Pour cela, on utilise la commande `git remote add origin git@git-manager.io:organization-name/project-name.git`.

Dans cette commande, `origin` est le nom du remote. Il est quasiment toujours unique et par convention, on l'appelle _origin_ quasiment tout le temps… `git-manager.net` est l'URL du serveur abritant le dépôt distant. Elle est suivie par le nom de l'utilisateur sur le serveur, et le nom du projet.

### Pousser les derniers changements

Avec la commande `git push -u origin master`, on indique sur quel _upstream_ (`-u` est la version courte de `--set-upstream`) on pousse la branche en cours vers la branche distante master (ou une autre).

On n'a besoin de préciser l'_upstream_ qu'une fois, il est mémorisé pour cette branche, par la suite on fait juste `git push`,

Dans certains cas (si on fait un _amend commit_, à voir plus tard), on peut avoir besoin de forcer le push : `git push --force-with-lease`.

## Récupérer les changements

En général, on ne travaille pas seul sur un projet. Il faut donc récupérer les modifications effectuées par les autres développeurs. On le fait par une opération de fusion du code (*merge*).

### Gérer les conflits

Tant que les développeurs travaillent sur des fichiers différents, tout va bien, en général. À des nuances près : on peut utiliser une fonctionnalité, un service qui est modifié, voire supprimé par un autre développeur. Il faudra alors adapter le code existant, qui ne compilera plus.

Mais si on modifie un fichier, une ligne, qui est aussi modifié par un autre développeur qui a poussé avant sur le serveur, quand on récupère ses changements, on aura un conflit : on a deux versions différentes pour cette (ou ces) ligne, et il va falloir trancher pour l'une ou l'autre, ou faire un compromis. Ça s'appelle résoudre un conflit de fusion.

Pour cela, on utilise un outil de fusion (*merge tool*) qui montre votre version, la version de l'autre développeur, et la version commune aux deux. On peut alors choisir soit une des deux versions, soit modifier le fichier pour résoudre le problème.

### Récupérer les autres branches distantes

Si des branches ont été créées, on peut les récupérer avec `git fetch --all`. Elles sont affichées sous la forme `origin/branch_name`. Si on bascule sur une de ces branches, il faudra la créer en local, généralement en gardant le même nom.

### Mettre à jour master

Quand vous êtes sur master, le meilleur moyen pour se mettre à jour des derniers changements sur cette branche est de faire `git pull --rebase`.

### Mettre à jour sa branche par rapport à master

Quand on développe sur une branche, master peut évoluer entre temps. On peut vouloir mettre à jour sa branche pour suivre les dernières évolutions : ça permet de les utiliser, ou au moins à éviter des conflits futurs, voire de résoudre tout de suite des conflits existants, avant qu'ils ne deviennent plus complexes.

Pour cela, il faut se mettre sur master, et le mettre à jour. Puis, revenir sur sa branche, et la rebaser sur master, avec la commande `git TODO`.

On peut faire un commit de merge à la place du rebasement, mais j'estime que ça créé du "bruit", des infos supplémentaires inutiles dans l'historique du projet.

## Taguer (labels)


## Corriger un commit


### Amend commit

### Rebase interactive
