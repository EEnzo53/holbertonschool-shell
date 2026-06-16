# Permissions Linux - Guide de reference

> Comprendre et manipuler les permissions de fichiers sous Linux/Unix : notation symbolique, notation octale, et `chmod`.

## Sommaire

- [Pourquoi des permissions ?](#pourquoi-des-permissions-)
- [Les 3 categories d'utilisateurs](#les-3-categories-dutilisateurs)
- [Les 3 types de droits](#les-3-types-de-droits)
- [Notation symbolique](#notation-symbolique)
- [Notation octale](#notation-octale)
- [Combinaisons usuelles](#combinaisons-usuelles)
- [Utiliser `chmod`](#utiliser-chmod)
- [Entrainement](#entrainement)

---

## Pourquoi des permissions ?

Linux est un systeme multi-utilisateur. Le systeme de permissions definit **qui a le droit de faire quoi** sur chaque fichier ou dossier, pour eviter que tout utilisateur ou processus puisse lire, modifier ou executer n'importe quoi.

---

## Les 3 categories d'utilisateurs

| Categorie | Anglais | Symbole `chmod` |
|---|---|---|
| Proprietaire | Owner / User | `u` |
| Groupe proprietaire | Group | `g` |
| Tous les autres | Others | `o` |

Chaque fichier appartient a **un seul utilisateur** et **un seul groupe** (modifiables via `chown` / `chgrp`).

---

## Les 3 types de droits

| Droit | Symbole | Sur un fichier | Sur un dossier |
|---|---|---|---|
| Lecture (Read) | `r` | Lire le contenu | Lister les fichiers |
| Ecriture (Write) | `w` | Modifier le contenu | Creer / supprimer des fichiers |
| Execution (Execute) | `x` | Lancer comme programme | Y acceder (`cd`) |

---

## Notation symbolique

Affichee par `ls -l` :

```
-rwxr-xr--
```

```
-   rwx    r-x    r--
^    ^      ^      ^
type owner group others
```

- 1er caractere -> type (`-` fichier, `d` dossier, `l` lien symbolique...)
- 3 blocs de 3 lettres -> proprietaire / groupe / autres, toujours dans l'ordre `r`, `w`, `x`
- `-` = droit absent

---

## Notation octale

Forme compacte utilisee par `chmod` (ex : `chmod 754`).

### Valeurs de base

| Droit | Valeur |
|---|---|
| `r` | 4 |
| `w` | 2 |
| `x` | 1 |
| *(aucun)* | 0 |

### Calcul par categorie

On additionne les valeurs des droits actives :

| Symbolique | Calcul | Octal |
|---|---|---|
| `---` | 0+0+0 | `0` |
| `--x` | 0+0+1 | `1` |
| `-w-` | 0+2+0 | `2` |
| `-wx` | 0+2+1 | `3` |
| `r--` | 4+0+0 | `4` |
| `r-x` | 4+0+1 | `5` |
| `rw-` | 4+2+0 | `6` |
| `rwx` | 4+2+1 | `7` |

### Assemblage final

Le code complet = concatenation des 3 chiffres, dans l'ordre **proprietaire -> groupe -> autres**.

**Exemple** -- `rwxr-xr--` :

| | Proprietaire | Groupe | Autres |
|---|---|---|---|
| Symbolique | `rwx` | `r-x` | `r--` |
| Chiffre | `7` | `5` | `4` |

-> `chmod 754`

---

## Combinaisons usuelles

> 512 combinaisons existent au total (8^3), mais voici les plus frequentes :

| Octal | Symbolique | Usage typique |
|---|---|---|
| `777` | `rwxrwxrwx` | Acces total -- a eviter sauf cas particulier |
| `755` | `rwxr-xr-x` | Scripts, programmes, dossiers partages en lecture |
| `700` | `rwx------` | Dossier strictement prive |
| `666` | `rw-rw-rw-` | Modifiable par tous -- rare en pratique |
| `644` | `rw-r--r--` | Fichiers standards (documents, code) |
| `600` | `rw-------` | Fichier sensible (ex : cle SSH privee) |
| `040` | `----r-----` | Lecture reservee au groupe uniquement |

---

## Utiliser `chmod`

**Mode numerique :**

```bash
chmod 644 fichier.txt
```

**Mode symbolique** (`+` ajoute, `-` retire, `=` fixe) :

```bash
chmod g+r fichier.txt     # ajoute la lecture pour le groupe
chmod o-w fichier.txt     # retire l'ecriture pour les autres
chmod u=rwx fichier.txt   # fixe exactement les droits du proprietaire
```

---

## Entrainement

| # | Question | Reponse |
|---|---|---|
| 1 | Octal de `rw-r-----` ? | <details><summary>voir</summary><code>640</code></details> |
| 2 | Symbolique de `chmod 711` ? | <details><summary>voir</summary><code>rwx--x--x</code></details> |
| 3 | `chmod` pour : proprietaire+groupe en lecture/execution, autres aucun acces ? | <details><summary>voir</summary><code>chmod 550</code></details> |

---

### A retenir

> **3 categories x 3 droits.** Chaque chiffre octal = somme des droits actives (`r`=4, `w`=2, `x`=1) pour une categorie donnee.

```
        chmod   7    5    4
                |    |    |
              owner group others
                |    |    |
              rwx   r-x  r--
```
