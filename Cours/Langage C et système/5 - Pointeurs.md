	# 5 - Pointeurs
---
![[05 Pointeurs.pdf]]

## Définition

C'est un élément qui contient l'adresse d'un objet C d'un type donné.

Donc un pointeur est typé, un pointeur contenant l'adresse d'un entier sera de type pointeur vers entier.

Permet de désigner une zone de mémoire alloué dynamiquement.

## Utilité

Ils permettent :
- de passer des objets par référence
- de rendre l'écriture de code plus compact
- de rendre l'écriture de code plus efficace
Mais ils rendent également le code moins lisible :-(

## Exemple
```C
int *p1, *p2; /* pointeurs sur entiers */
int *p1, p2; /* Attention: un pointeur, un int */
struct {int x, y;} *ps; /* pointeur sur structure */
void *r; /* pointeur sur void: adresse brute */
int *s[10]; /*tableau de 10 pointeurs sur int */
int (*s)[10] /* pointeur sur tableau de 10 int */
```
Un pointeur peut désigner n'importe quelle variable (statique, dynamique, constante). Il peut aussi dénoter l'adresse d'une fonction :

```C
int (*pfunc) (void); /* pointeur sur une fonction sans paramètre qui renvoie un entier */
int (*T[5]) (void) /* tableau de 5 pointeurs de ce type, PAS COMPRIS */
```

## Opérations sur les pointeurs
Deux opérations possible sur les pointeurs :
- \*p permet l'indirection (déréférence) (on accèse à l'élément dans la case mémoire associé à l'adresse p)
- &v renvoie l'adresse de la variable v (référence)

Exemple :
![[Pasted image 20221107081607.png | center ]]

Ici faire x = ... est équivalent à faire \*p = ...

### Cas particulier sur les pointeurs sur structure
```C
struct {
	int a, b;
} x, *p = &x;
```
- x est une structure
- \*p désigne cette structure
- l'accès au champ a de x : x.a
- l'accès au même champ avec p : (\*p).a

Notation spéciale :
- Pour simplifier (\*p).a on peut aussi écrire p->a cela fait la même chose.

## Paramètre de type pointeur

Avant si l'on voulait faire une fonction pour échanger les valeurs de deux variables on avait ce souci :
![[Pasted image 20221107082051.png |center]]

Comme les fonctions fonctionnent par passage de valeur, ici x et y n'était pas modifiés. Avec les pointeurs cela fonctionne en faisant :
![[Pasted image 20221107082144.png |center]]
En effet ici, on passe la valeur de l'adresse de x et de y donc dans la fonction on ne modifie pas les adresse mais le contenu des cases mémoire liés aux adresse de x et de y.

## Opérations arithmétiques sur les pointeurs

### Comparaison
- == et !=
- mais aussi <,<=, > et >=

### Addition / soustraction d'un entier
- pointeur + entier -> pointeur
- pointeur - entier -> pointeur
- permet de calculer des décalage d'adresses

### Différence entre deux pointeurs de même type
- pointeur - pointeur -> pointeur
- permet de calculer le nombre d'éléments de ce type entre les adresses

### Conversions : 
- conversion d'un void \* vers (ou depuis) un pointeur quelconque : Toujours OK
- les autres conversion nécessitent un cast

## Pointeurs et tableaux

- En C, pointeurs et tableaux sont deux concepts proches.
- En fait, le nom d'un tableau correspond à l'adresse de la première case du tableau.
- L'arithmétique de pointeurs permet d'indexer un tableau avec des pointeurs.

![[Pasted image 20221107082733.png | center ]]

## Pointeurs sur fonction
```C
int plus(int op1, int op2) {return op1 + op2;}
int minus(int op1, int op2) {return op1 - op2;}
...
int (*operation) (int, int); /* pointeur sur fonction */
int a, b, res;

a = read_operand();
switch (getchar()) {
	case '+': operation = plus; break;
	case '-': operation = minus; break;
	...
}
b = read_operand();
res = (*operation)(a, b); /* En C ANSI: operation(a,b) */
```
```C
struct Func {
	char nom[10];
	double (*f)(double);
};

struct Func T[] = {
	{"sinus", sin},
	{"cosinus", cos},
	...
};

double (*fct) (double) = chercher(T, "cosinus");
printf("cosinus(42) = %lf\n",fct(42));
```

## Exemple d'utilisation des pointeurs en simplifiant strcat (très intéressant)

![[Pasted image 20221107083837.png]]
![[Pasted image 20221107083853.png]]

Donc finalement on est passé d'une version complexe à un code lisible en deux ligne en améliorant notre compréhension de C :
![[Pasted image 20221107084224.png]]

## Autre exemple avec strcmp
![[Pasted image 20221107083922.png]]

## Tableau de chaîne de caractères

![[Pasted image 20221107084257.png]]
![[Pasted image 20221107084309.png]]
![[Pasted image 20221107084321.png]]

## Allocation mémoire dynamique
Allocation mémoire
```C
void *malloc(size_t size);
void *calloc(size_t nelems, size_t size);
void *realloc(void *ptr, size_t new_size);
```
- Ces fonctions renvoient un pointeur sur la zone allouée ou NULL si l'allocation est impossible.

Libération mémoire
```C
void free(void *ptr);
```
- Le pointeur ptr doit avoir été obtenu par une des 3 fonctions précédentes.

### Allocation dynamique : Liste chaînée
```C
typedef struct {/* Les infos que l'on met dans la liste */
	char name[30];
	...
} Info;

struct elem { /* maillon de la liste chaînée */
	Info i;
	struct elem *next;
};

/* Allouer un nouvel élement */
struct elem *new_info(Info inf) {
	struct elem *p = (struct elem *) malloc(sizeof(elem));
	if(p == NULL){
		fprintf(stderr, "allocation error\n");
		exit(1);
	}
	p->i = inf;
	p->next = NULL;
	return p;
}

/* Insertion d'un nouvel élément devant la liste L */
struct elem *new = new_info(les_infos);

new->next = L;
L = new;

/* Recherche dans la liste chaînée */
struct elem *search_info(struct elem *list, char who[])
{
	struct elem *p;

	for(p = list; p != NULL; p = p->next)
		if(strcmp(p->i.name, who) == 0)
			return p;
	return NULL; /* not found */
}
```

### Allocateur basique, exemple à check personnellement car intéressant apparemment :
![[Pasted image 20221107085819.png]]

## Erreur à ne pas commettre avec les pointeurs 

- Inutile de faire un malloc(50) autant faire un tableau de 50 élément, car faire un malloc à un coup double par rapport à un tableau et un malloc est source de bug.
- Il ne faut pas perdre une adresse allouée car il faudra faire un free dessus au bout d'un moment pour éviter une fuite mémoire.
- Il ne faut pas retourner un tableau dans une fonction car on perd les valeurs faites en local, **pas compris la suite de l'explication**.
- Il faut toujours allouer un pointeur pour pouvoir faire quelque chose dessus car ici le gets **pas compris la suite**.
- Bon déjà un malloc de taille fixe comme pour la première erreur est inutile. Et pb de fuite mémoire que j'ai pas compris.
- Ici le sizeof ne renvoie pas la longueur de la chaine mais la taille mémoire de la chaîne, ici il faudrait donc se servir de strlen...
- Mais dans le f7 strlen seul ne suffit pas car on oublie le '/0' donc il faut ajouter un +1
- **à rattrapper**
- **à rattrapper**
- Ici on voit les différences entre pointeurs et tableau.
- Si a < b dans cette exemple on utilise une zone désallouée donc ça marche pas et dans le compilateur il n'y a pas d'erreur car le compilateur ne check pas quand à lieu les appels à malloc et à free.
- On appelle deux fois free sur la même zone mémoire.