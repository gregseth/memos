
Excel
=====

Formules utiles
---------------

  Recherche de la valeur dans une colonne `X` correspondant à une valeur 
`"Needle"` sur la même ligne dans une autre colonne `A`  :

    =INDEX($X:$X;MATCH("Needle";$A:$A;0))

  Définition d'une plage nommée (Win: `Formules > Gestionnaire de noms`  ; Mac:
`Insert > Name > Define…`) qui s'adapte automatiquement à la taille des données
de la colonne  :

    =OFFSET(Sheet1!$A$1;0;0;COUNTA(Sheet1!$A:$A);1)

  Création d'une liste de validation (`Données > Validation des données`) avec
autocomplétion (l'autocomplétion ne se fait pas lors de la saisie, mais c'est le
contenu de la liste déroulante qui est restreint aux valeurs corresondant à la
saisie). Dans l'exemple, la liste des valeurs possibles est en colonne `A` et la
zone de saisié validée en `B1`  :

    =OFFSET($A:$A;MATCH($B1&"*";$A:$A;0)-1;;COUNTIF($A:$A;$B1&"*"))


Équivalence anglais/français de formules usuelles
-------------------------------------------------

   Anglais       | Français             
 :---------------|:---------------
  COUNTA         | NB.VAL              
  INDEX          | INDEX              
  MATCH          | EQUIV              
  OFFSET         | DECALER    
                 


Formats personnalisés
---------------------

### Opérateurs numériques

 - `0` : Substitue un chiffre, ou un 0 en absence de chiffre ;
 - `#` : Substitue un chiffre, n'affiche rien en absence de chiffre ;
 - ` ` (espace) : Divise par 1000 le nombre le précédant ;
 - `/` : Affiche les décimales d'un nombre sous forme fractionnaire, si le 
	diviseur est précisé c'est lui qui est utilisée, si c'est `?` le diviseur
	est sélectionné automatiquement ;
 - `?` : Sélectionne le nombre maximum de chiffres du dénominateur
	de la fraction.


### Opérateurs de date et heure

 - `[]` : Permet d'afficher une durée à la place d'une date, par
	exemple, 1h15 formattée `m` affiche 1 alors que formattée `[m]` elle est
	affichée 75.

#### Date 

 - `aa` : année sur deux chiffres ;
 - `aaa` ou `aaaa` : année sur quatre chiffres ;
 - `m` : mois ;
 - `mm` : mois sur deux chiffres ;
 - `mmm` : trois premières lettres du mois ;
 - `mmmm` : nom du mois complet ;
 - `j` : jour du mois ;
 - `jj` : jour du mois sur deux chiffres ;
 - `jjj` : trois premières lettres du jour de la semaine ;
 - `jjjj` : jour de la semaine ;

#### Heure
 
 - `h` : heure ;
 - `hh` : heure sur deux chiffres ;
 - `m` minutes ;
 - `mm` : minutes sur deux chiffres ;
 - `s` : secondes ;
 - `ss` : secondes sur deux chiffres ;


### Opérateurs de chaînes de caractères

 - `@` : Représente le texte de la cellule ;
 - `\` : Affiche le caracère situé immédiatement après l'opérateur ;
 - `""` : Affiche la chaîne située entre les guillemets ;
 - `_` : Affiche un espace de la taille exacte du caractère situé immédiatement 
	après l'opérateur ;
 - `*` : Répète le caractère situé immédiatement après l'opérateur jusqu'à la
	fin de la cellule ;
 


### Structure d'une règle

 - `;` : Séparateur de règles.

  Une rèlge de mise en forme peut être soit simple, soit composée de quatre 
parties spécifiques. Dans ce dernier cas chaque partie est séparée par 
l'opérateur `;` et représente respectivement :

 - Le format d'un nombre positif ;
 - Le format d'un nombre négatif ;
 - Le format de la valeur 0 ;
 - Le format d'une chaîne de caractères.


### Mise en forme conditionnelle

  Chaque règle peut être précédé d'une valeur entre crochets indiquant le nom
d'une couleur ou l'index d'une couleur, par exemple `[Vert]` ou `[Couleur25]`,
la couleur est alors donnée au texte de la cellule. 

  La couleur peut elle même être précédée d'une valeur entre crochet précisant
la condition que doit remplir la valeur de la cellule pour que le couleur soit
appliquée. La valeur commence par un opérateur, suivi de la valeur à laquelle
comparer le contenu de la cellule.

  Les opérateurs possibles sont :
 
 - `<=` : Inférierur ;
 - `<` : Strictement inférierur ;
 - `>=` : Supérieur ;
 - `>` : Strictement supérieur ;
 - `=` : Égal ;
 - `<>` : Différent.
 
### Exemples

#### Nombres

  Valeur de la cellule | Chaîne de mise en forme | Résultat
 ---------------------:|------------------------:|------------:
  14                   |                   `000` |         014
  14                   |                   `##0` |          14
  123000               |                  `# \K` |        123K
  .75                  |                 `?/100` |      75/100
  .75                  |                   `?/?` |         3/4
  3.75                 |                   `?/?` |        15/4
  3.75                 |                `#\ ?/?` |       3 3/4

#### Texte

  Texte de la cellule  | Chaîne de mise en forme | Résultat
 :---------------------|------------------------:|:------------
  abcd                 |                 `@\-\-` | abcd--
  abcd                 |                 `@"--"` | abcd--
  abcd                 |                 `@_-\-` | abcd -
  abcd                 |                   `@*-` | abcd-------
