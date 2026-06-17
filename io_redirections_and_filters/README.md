# I/O Redirections and Filters

## Introduction

Ce document resume les notions de redirection des entrees/sorties (I/O) et les filtres courants utilises dans un terminal Linux/Unix.

## La sortie standard (stdout)

Par defaut, le resultat d'une commande s'affiche a l'ecran. On appelle ca la sortie standard.

### Rediriger vers un fichier

| Symbole | Fonction |
|---|---|
| `>` | Envoie le resultat dans un fichier, en ecrasant son contenu existant |
| `>>` | Envoie le resultat dans un fichier, en l'ajoutant a la fin (sans effacer) |

Exemples :

```
ls > liste.txt        # cree ou remplace liste.txt
ls >> liste.txt       # ajoute a la suite de liste.txt
```

Si le fichier n'existe pas encore, il est cree automatiquement dans les deux cas.

## L'entree standard (stdin)

Par defaut, une commande qui attend une saisie la recoit du clavier. On peut remplacer le clavier par un fichier grace a `<`.

```
sort < liste.txt
```

Ici, `sort` lit le contenu de `liste.txt` au lieu d'attendre une saisie clavier, puis affiche le resultat trie a l'ecran.

On peut combiner entree et sortie dans la meme commande :

```
sort < liste.txt > liste_triee.txt
```

L'ordre entre `<` et `>` n'a pas d'importance, mais ils doivent toujours venir apres le nom de la commande et ses arguments.

## Les pipelines (|)

Un pipeline permet de connecter la sortie d'une commande directement a l'entree d'une autre, sans passer par un fichier intermediaire. Le symbole est `|`.

```
ls -l | less
```

Exemples utiles :

| Commande | Effet |
|---|---|
| `ls -lt \| head` | Affiche les 10 fichiers les plus recents |
| `du \| sort -nr` | Classe les dossiers du plus gros au plus petit |
| `find . -type f \| wc -l` | Compte le nombre total de fichiers |

## Les filtres

Un filtre est un programme qui lit du texte en entree, le transforme, puis renvoie le resultat en sortie. Les filtres sont concus pour etre utilises dans des pipelines.

| Programme | Role |
|---|---|
| `sort` | Trie les lignes recues |
| `uniq` | Supprime les lignes en double (le texte doit deja etre trie) |
| `grep` | Garde uniquement les lignes contenant un motif donne |
| `fmt` | Reformate le texte pour une meilleure lisibilite |
| `pr` | Met le texte en pages, pret a etre imprime |
| `head` | Affiche les premieres lignes (defaut : 10) |
| `tail` | Affiche les dernieres lignes (defaut : 10) |
| `tr` | Remplace ou convertit des caracteres |
| `sed` | Editeur de flux, modifications de texte avancees |
| `awk` | Veritable langage de programmation pour traiter du texte |

## Options utiles a connaitre

- `cat -n fichier.txt` -> affiche le fichier avec un numero devant chaque ligne
- `sort -n fichier.txt` -> trie selon la valeur numerique plutot qu'alphabetique
- `head -n 5 fichier.txt` -> affiche seulement les 5 premieres lignes
- `tail -n 5 fichier.txt` -> affiche seulement les 5 dernieres lignes
- `tail -f fichier.txt` -> suit en direct les lignes ajoutees au fichier

## Resume des symboles

| Symbole | Fonction |
|---|---|
| `>` | Redirige la sortie vers un fichier (ecrase) |
| `>>` | Redirige la sortie vers un fichier (ajoute) |
| `<` | Redirige l'entree depuis un fichier |
| `\|` | Relie la sortie d'une commande a l'entree d'une autre |
