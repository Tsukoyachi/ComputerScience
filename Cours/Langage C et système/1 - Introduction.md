# Introduction
---
### Lessons date :
- 03/10/22

### Teachers :
- Stéphane Lavirotte

### Tags :
#C #système

---

# Les paradigmes de Programmation

Définition paradygme : Le **paradigme de programmation** est la façon (parmi d'autres) d'approcher la programmation informatique et de formuler les solutions aux problèmes et leur formalisation dans un langage de programmation approprié.

Les langages va utiliser plusieurs concept et paradygme.

Il y a beaucoup plus de paradigmes que de concept.

Un paradygme est un ensemble de concept.

Avec n concepts on peut faire 2^n paradygmes.

Il y a plusieurs concept majeurs tel que : 
(à rattraper)

### Familles de programmation
Programmation impérative :
	- Programmation Structurée :
		- Eviter la programmation spaghetti (ex : goto)
	- Programmation procédurale : C, Ada, Pascal, ...
	- Programmation Orientée Objet : C++, Java; C#, ...

Programmation Déclarative :
	- Programmation Fonctionnelle
	- Programmation Logique
	- Programmation par contraintes

![[Pasted image 20221003082025.png | center | 705]]

## Langage C : Programmation Procédurale

### Pourquoi apprendre le C :
- A la base de nombreux langages (syntaxiquement)
	- C++, C#, Java, PHP, Javascript
- A la base, le langage pour coder Unix
	- Noyau, compilateur, outils système, bibliothèques, ...
- Linux Torvalds: "Nothing better than C" (2012)
	- "Si vous pensez comme un ordinateur, écrire en C a du sens"
- Un des langages les plus efficaces
	- Efficacité au niveau CPU
	- Consommation mémoire
- Un des langages les plus utilisés encore de nos jours
- Des centaines de millions de lignes écrites en C
	- Noyau linux : ~30 millions de lignes de code; 98.5% en C
	- Compilateur gcc : ~10 millions de lignes de code; 47.7% en C

### Un peu d'histoire :
- Racines
	- Algol 60
	- BCPL (1967)
	- B (auteur : K. Thomson ~ 1970)
- Auteur
	- Denis Ritchie (Bell Labs 1972)
	- Première spécification du langage dans le livre de Kernigham et Ritchie en 1978 (version K&R)
- Plusieurs versions de C disponibles :
	- Traditionnel : celui qui est décrit dans la version originale de K&R 1978 - Version Obsolète
	- ANSI C et ISO C : sur-ensembles de K&R C (1983 -> 1990)
	- Version la plus courante
		- C99 (1999) - La version que nous utiliserons dans ce cours
		- C11 (08/12/2011) - La dernière incarnation du langage (peu efficace)

### Buts du langage
- Langage de programmation
	- "facile" à apprendre
	- facile à implémenter
- Langage de programmation pour implémenter des systèmes d'exploitation :
	- conçu à l'origine pour programmer le noyau d'Unix v6
	- proche de la sémantique du processeur
	- simple et efficace
	- accès facile aux mécanismes de bas niveau
	- pointeurs (typés, mais non contrôlés)
	- contrôles de types permissifs (outil lint)
	- pas de contrôle de type à l'exécution
- Permettre l'écriture de programmes portables
	- PCC (Portable C Compiler)
	- Utilisation d'une bibliothèque standard
	- les Entrées, Sorties ne sont pas dans le langage
	- Pas de chaînes de caractères à proprement parler (mais de nombreuse fonctions pour les manipuler)
	- pas de concurrence dans le langage (mais accessible au travers l'utilisation de fonctions de la bibliothèque) (définition non exacte, concurrence ~= fait de laisser plusieurs flots d'exécution accéder à une même référence en même temps)
- Fournir un ensemble d'outils systèmes bien féfini autour du langage
	- compilation séparée (on parle du système, ce qui permet d'éviter la définition d'extensions incompatibles)
	- options de compilation (on retrouve les mêmes options sur la plupart des implémentation du langage)

### Evaluation du langage
- Avantage :
	- Efficace
		- Registres, pointeurs, opérations sur les bits
		- pas de contrôle à l'exécution
		- ...
	- Grande liberté du programmeur (=> le programmeur est responsable)
	- Bibliothèque très étendue
		- Concurrence
		- Entrée, sortie
		- Manipulation de chaînes de caractères
		- ...
	- => Des centaines de millions de lignes écrites en C
	- Interface claire avec Unix
		- Donne l'impression que l'on est sur Unix, même si ce n'est pas le cas.
		- => permet l'écriture de programmes indépendants de l'OS cible (dans une certaine mesure)
	- Bon langage d'assemblage portable utilisé comme langage cible par de nombreux compilateur (C++, Objective C, Scheme, ...)
	- Vraiment portable
		- Unix en est la preuve (pratiquement écrit entièrement en C)
		- Linux qui tourne sur à peu près toutes les architectures matérielles (PC, Arm, Sun Sparc, PowerPC, 68000 machines, ...)
		- Les outils de projet GNU de la FSF
		- **(à rattraper..., il manque un point)**
- Inconvénients :
	- Syntaxe à deux niveau (préprocesseur)
	- Grande liberté du programmeur (=> le programmeur doit être responsable)
	- Les erreurs de compilation n'aident pas toujours
		- Contrôle de type souvent trop permissifs
		- lint (pour K&R)
		- C'est moins vrai en C ANSI
		- Malheureusement compatibilité K&R peut être un problème
		- Utilisation des pointeurs peut être "délicate"
	- Langage ancien
		- Pas de concepts modernes (généricité, objets, modules, ...)
		- La modularité est très simpliste
			- Basée sur les fichiers
			- Pas adapté pour les gros projets en équipes (quoi que, le noyau Linux...)

### Implémentation du langage C
- DEC PDP-11 en 1975
- PCC (Portable C Compiler) en 1978
	- implémentation publique
	- la source de la plupart des compilateurs pré-ANSI
- Aujourd'hui **à rattraper** (1 ligne)
- Plusieurs normes :
	- Norme ANSI X3J11
- Il y a aussi une normes ISO **à rattrapper** (2 lignes)

- Aujourd'hui
	- Les compilateurs sont conformes à la norme ISO / C99
	- ** à rattraper** (quelques lignes)

### Quelques Exemples 

1. Le classique "Hello world"

```C
#include <stdio.h>
main()
{
	printf("Hello, world!\n")
}
```

Compilation : 

```
$ gcc -o hello hello.c
```

Exécution :

```
$ hello
Hello, world!
$
```

2. Programme multi-fichiers 

```C
extern void say_hello(void) /* Fichier main.c */
{
	say_hello();
}
```

```C
#include <stdio.h> /* Fichier hello.c */
void say_hello(void){
	printf("Hello, world!\n");
}
```

Compilation :

```
$ gcc -o hello main.c hello.c
```

Exécution :

```
$ gcc -c main.c
$ gcc -c hello.c
$ gcc -o hello main.o hello.o
```

```ad-warning
title: Compilateur laxiste

Si dans l'exemple 2, nous n'avions pas mis le extern void... et le \#include ...
le code aurait compilé sans problème, mais c'est un code non valide, pour éviter cela on peut mettre tous les warnings avec **-Wall** et on peut ajouter **-std=c99** pour être sûr de compiler du C99.
```

```ad-note
title: Retour sur Hello World

Si on compile le hello workd avec -Wall, il faut un type de retour à la fonction main.
L'on ajoutera donc "return 0" à la fonction main et l'on changera son type de sortie de void à int et le code sera correct.
```

4. La commande cat

```C
#include <stdio.h>
int main()
{
	int c;
	c = getchar();
	while (c != EOF) {
		putchar(c); // Imprime sur la sortie standards
		c = getchar();
	}
	return 0;
}
```

- Ce programme :
	- n'utilise pas le style c
	- Ne profite pas du fait que C est un langages d'expressions

On aurait dû faire :
```C
#include <stdio.h>
int main()
{
	int c;
	c = getchar();
	while ((c = getchar()) != EOF) {
		putchar(c); // Imprime sur la sortie standards
	}
	return 0;
}
```

5. Comptage de caractère :

```C
#include <stdio.h>
int main()
{
	long nbcar = 0;

	while(getChar() != EOF){
		nbChar++;
	}
	printf("I've read %ld characters\n", nbChar);

	return 0;
}
```

```C
#include <stdio.h>
int main()
{
	long nbcar = 0;

	for(nbCar = 0; getchar() != EOF ; nbChar++){
	/*rien à faire*/
	}
	printf("I've read %ld characters\n", nbChar);
}
```

6. Equation du second degré
**check le slide pour l'avoir et le comprendre**