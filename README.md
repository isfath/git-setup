# Git sous Windows

> **Remarque** : ce guide est spécifique à Windows 10.

## Installation

### Manuelle
1. Téléchargez Git depuis le [site officiel](https://git-scm.com/download/win)
1. Déplacez l'exécutable dans ce dossier
1. Adaptez le nom de l'exécutable dans `setup.bat`
1. Lancez le batch

### Automatisée
Dans AD, les GPO permettent de configurer les postes de manière centralisée :
1. Un MSI est automatiquement installé, mis à jour et désinstallé.
   Malheureusement, Git n'est pas distribué sous cette forme.
   Le wrapper le plus connu (MSI Wrapper) est payant.
2. Une _tâche immédiate_ exécutée une seule fois peut lancer une installation
   silencieuse (reprendre le contenu de [setup.bat](setup.bat)).
   Les fichiers sont stockés dans le même dossier que la _Policy_
   (qu'on retrouve en triant les dossiers par date de dernière modification) et
   référencés via un UNC.
   L'inconvénient, c'est que la désinstallation est manuelle.

## Configuration utilisateur

1. Ouvrez un terminal : Win+R, `cmd`, Enter

2. Renseignez votre nom et votre e-mail
```
git config -e --global
```

3. Générez une clé SSH
```
ssh-keygen
```

4. Copiez votre clé publique
```
notepad .ssh\id_rsa.pub
```

5. Collez-la dans vos paramètres GitHub

## Passages à la ligne

Windows et les dérivés d'UNIX ne sont pas d'accord sur les fins de lignes.
Quand un dépôt doit être manipulé depuis différents OS, il faut normaliser
ses fichiers sous peine de pourrir nos diff et de perturber leurs outils natifs.

Le paramètre `core.autocrlf` permet de configurer la normalisation dans git :
- `false` : rien changer (par défaut sous GNU/Linux)
- `true` : normalise et checkout avec CRLF (Windows)
- `input` : normalise seulement (GNU/Linux)

Certains fichiers ne s'utilisent que sur une seule plateforme (`.vbs`...),
on peut spécifier leur type de fin de lignes dans `.gitattributes`
qui sera inclus au dépôt :
```
*.vbs eol=crlf
*.hta eol=crlf
```

Après un changement, on convertit les fichiers avec `git add --renormalize`.
Malgré leur réécriture, on y voit clair grâce à `git blame --ignore-rev`.

Si vous voulez en savoir plus, [cet article](
https://www.aleksandrhovhannisyan.com/blog/crlf-vs-lf-normalizing-line-endings-in-git/
) explique bien les choses.
