# Introduction au Shell

## General

### Que signifie RTFM ?
RTFM signifie "Read The F***ing Manual" (Lis le manuel). C'est une expression utilisee pour rappeler qu'avant de poser une question, il faut d'abord consulter la documentation disponible.

### Qu'est-ce qu'un Shebang ?
Un shebang est la sequence de caracteres `#!` placee en toute premiere ligne d'un script. Elle indique au systeme quel interpreteur utiliser pour executer le fichier. Exemple :
```
#!/bin/bash
```

---

## Le Shell

### Qu'est-ce que le shell ?
Le shell est un programme qui sert d'interface entre l'utilisateur et le systeme d'exploitation. Il interprete les commandes saisies par l'utilisateur et les transmet au noyau (kernel) pour execution.

### Quelle est la difference entre un terminal et un shell ?
- **Terminal** : programme graphique ou physique qui fournit une fenetre de saisie de texte (exemple : GNOME Terminal, iTerm2).
- **Shell** : programme qui tourne a l'interieur du terminal et interprete les commandes (exemple : bash, zsh, sh).

### Qu'est-ce que le prompt du shell ?
Le prompt est l'invite de commande affichee par le shell, signalant qu'il est pret a recevoir une commande. Il ressemble souvent a :
```
user@machine:~$
```

### Comment utiliser l'historique (les bases) ?
- `history` : affiche la liste des commandes precedemment executees.
- Fleche haut/bas : navigue dans l'historique.
- `!!` : reexecute la derniere commande.
- `!n` : reexecute la commande numero `n` de l'historique.

---

## Navigation

### A quoi servent `cd`, `pwd`, `ls` ?
- `cd` (change directory) : change le repertoire courant.
- `pwd` (print working directory) : affiche le chemin du repertoire courant.
- `ls` (list) : liste le contenu d'un repertoire.

### Comment naviguer dans le systeme de fichiers ?
On utilise `cd` suivi d'un chemin :
```
cd /home/user/Documents
cd ..       # remonter d'un niveau
cd ~        # aller dans le repertoire personnel
```

### Que sont les repertoires `.` et `..` ?
- `.` : represente le repertoire courant.
- `..` : represente le repertoire parent (un niveau au-dessus).

### Qu'est-ce que le repertoire de travail ?
C'est le repertoire dans lequel l'utilisateur se trouve actuellement. On l'affiche avec `pwd` et on le change avec `cd`.

### Qu'est-ce que le repertoire racine ?
C'est le sommet de l'arborescence du systeme de fichiers, note `/`. Tous les autres repertoires en decoulent.

### Qu'est-ce que le repertoire personnel (home) ?
C'est le repertoire prive de chaque utilisateur, generalement `/home/nom_utilisateur`. On y retourne avec `cd ~` ou simplement `cd`.

### Quelle est la difference entre `/` et `/root` ?
- `/` : repertoire racine du systeme, accessible a tous.
- `/root` : repertoire personnel de l'utilisateur `root` (administrateur). C'est son dossier prive, pas la racine du systeme.

### Caracteristiques des fichiers caches et comment les lister ?
Les fichiers caches commencent par un point (`.nomfichier`). Pour les afficher :
```
ls -a    # affiche tous les fichiers, y compris les caches
ls -la   # format long avec fichiers caches
```

### Que fait la commande `cd -` ?
Elle retourne dans le repertoire precedemment visite (avant le dernier `cd`).

---

## Explorer l'environnement

### A quoi servent `ls`, `less`, `file` ?
- `ls` : liste les fichiers et repertoires.
- `less` : affiche le contenu d'un fichier page par page (quitter avec `q`).
- `file` : determine le type d'un fichier (texte, binaire, image, etc.).

### Comment utiliser les options et arguments ?
Les commandes suivent la syntaxe :
```
commande [options] [arguments]
```
Exemple :
```
ls -l /home
```
`-l` est une option (format long), `/home` est l'argument (le repertoire cible).

### Format long de `ls` et comment l'afficher ?
```
ls -l
```
Chaque ligne affiche : permissions, nombre de liens, proprietaire, groupe, taille, date, nom du fichier.
```
-rw-r--r-- 1 user group 1024 Jan 01 12:00 fichier.txt
```

### Que fait la commande `ln` ?
Elle cree des liens entre fichiers :
- `ln fichier lien_physique` : cree un lien physique (hard link).
- `ln -s fichier lien_symbolique` : cree un lien symbolique (soft link).

### Que trouve-t-on dans les repertoires les plus importants ?

| Repertoire | Contenu |
|------------|---------|
| `/bin`     | Commandes essentielles du systeme |
| `/etc`     | Fichiers de configuration |
| `/home`    | Repertoires personnels des utilisateurs |
| `/var`     | Donnees variables (logs, bases de donnees) |
| `/tmp`     | Fichiers temporaires |
| `/usr`     | Programmes et ressources utilisateurs |
| `/root`    | Repertoire personnel de root |

### Qu'est-ce qu'un lien symbolique ?
Un lien symbolique (soft link) est un raccourci qui pointe vers un autre fichier ou repertoire. Si le fichier cible est supprime, le lien devient invalide.

### Qu'est-ce qu'un lien physique (hard link) ?
Un lien physique est une reference directe aux donnees d'un fichier sur le disque. Plusieurs noms peuvent pointer vers les memes donnees. La suppression d'un lien ne detruit pas les donnees tant qu'il reste au moins un lien.

### Difference entre lien physique et lien symbolique ?

| Critere              | Lien physique       | Lien symbolique        |
|----------------------|---------------------|------------------------|
| Pointe vers          | Les donnees (inode) | Un chemin de fichier   |
| Fichier supprime     | Donnees conservees  | Lien brise             |
| Fonctionne entre FS  | Non                 | Oui                    |
| Visible avec `ls -l` | Comme un fichier    | Affiche `->` cible     |

---

## Manipulation de fichiers

### A quoi servent `cp`, `mv`, `rm`, `mkdir` ?
- `cp source destination` : copie un fichier ou repertoire.
- `mv source destination` : deplace ou renomme un fichier.
- `rm fichier` : supprime un fichier (`rm -r` pour un repertoire).
- `mkdir nom` : cree un nouveau repertoire.

### Que sont les wildcards et comment fonctionnent-ils ?
Les wildcards sont des caracteres speciaux pour filtrer des noms de fichiers :
- `*` : remplace n'importe quelle suite de caracteres. Exemple : `ls *.txt`
- `?` : remplace un seul caractere. Exemple : `ls fichier?.txt`
- `[abc]` : remplace un caractere parmi ceux listes. Exemple : `ls fichier[123].txt`

---

## Travailler avec les commandes

### A quoi servent `type`, `which`, `help`, `man` ?
- `type commande` : indique si la commande est un builtin, un alias ou un programme externe.
- `which commande` : affiche le chemin complet d'une commande externe.
- `help commande` : affiche l'aide d'un builtin du shell.
- `man commande` : affiche le manuel complet d'une commande.

### Quels sont les differents types de commandes ?
1. **Builtins** : commandes integrees au shell (ex : `cd`, `echo`, `pwd`).
2. **Programmes externes** : executables situes dans le systeme (ex : `/bin/ls`).
3. **Alias** : raccourcis definis par l'utilisateur.
4. **Fonctions shell** : fonctions definies dans les scripts ou la configuration.

### Qu'est-ce qu'un alias ?
Un alias est un raccourci pour une commande plus longue. On le definit ainsi :
```
alias ll='ls -la'
```

### Quand utiliser `help` plutot que `man` ?
- `help` : pour les **builtins** du shell (commandes integrees comme `cd`, `echo`).
- `man` : pour les **commandes externes** et les programmes installes sur le systeme.

---

## Lire les pages de manuel (man)

### Comment lire une page de manuel ?
```
man nom_commande
```
Navigation dans la page :
- Fleche bas / Espace : avancer
- Fleche haut : reculer
- `/mot` : rechercher un mot
- `q` : quitter

### Quelles sont les sections d'une page man ?
Chaque page man est organisee en sections :
- **NAME** : nom et breve description.
- **SYNOPSIS** : syntaxe d'utilisation.
- **DESCRIPTION** : explication detaillee.
- **OPTIONS** : liste des options disponibles.
- **EXAMPLES** : exemples d'utilisation.
- **SEE ALSO** : references connexes.

### Quels sont les numeros de section pour les commandes utilisateur, appels systeme et fonctions de bibliotheque ?

| Section | Contenu                        |
|---------|--------------------------------|
| 1       | Commandes utilisateur          |
| 2       | Appels systeme (syscalls)      |
| 3       | Fonctions de bibliotheque C    |
| 4       | Fichiers speciaux              |
| 5       | Formats de fichiers            |
| 8       | Commandes d'administration     |

Pour acceder a une section specifique :
```
man 2 open    # appel systeme open()
man 3 printf  # fonction de bibliotheque printf()
```
