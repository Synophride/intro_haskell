## Haskell - "Apprentissage" 
Pas de "let" nécessaire pour déclarer des variables

Pas de construction spéciale pour déclarer des fonctions.
<nom> <params>*

Fonction à 0 argument = constante
Existence d'exceptions

Possibilité de définit des fonctions infixes avec les caractères \`
## Listes
[]
Séparation des éléments par ','
Concaténation de deux listes par l'op '++'
Ajout d'un élément en tête de liste par l'op ':' 

Fonctions 'a lst :
	*  head 'a lst -> 'a
	*  tail 'a lst -> 'a lst
	*  last 'a lst -> 'a     | Dernier élément
	*  init 'a lst -> 'a lst | Toute la liste sauf le dernier élément
	*  length 'a lst -> int
	*  reverse
	*  take int -> 'a lst -> 'a lst : extrait les <param> premiers éléments de la liste
	*  drop int -> 'a lst -> 'a lst : vire les n premiers éléments de la liste
	*  maximum
	*  minimum
	*  sum

Constructions de liste par range
[ 1 .. 100] -> range(1, 101) en python (les deux bornes sont incluses)
Fonctionne pour les caractères
Précision d'un pas possible, e.g
[ 1, 6..100] -> pas de cinq -> Séparation des deux premiers éléments 
-> Peut fonctionner avec les nombres flottants, mais c'est douteux à cause de la précision


Création de listes infinies (listes paresseuses)
	 * cycle : 'a list -> 'a list . Fait boucler la liste indéfiniment
	 * repeat : 'a -> 'a list : crée une liste infinie composée d'un seul élément = cycle [param]

Définition de liste par comprégension
[<formule en fonction d'un x> | x <- <liste>, <prédicat sur x>]

-> filtrage (en fonction de la liste)
signe  _ -> comme en caml

## tuples
Même syntaxe basique qu'en ml
(a, b, c)

fonctions
	* fst
	* snd

zip : 'a list -> b' list -> ('a * 'b) list


takeWhile <pred> <liste> -> rend les premiers éléments tels que le prédicat est vrai
permet de travailler sur des listes infinies, contrairement à filter

map
foldl
foldr
foldl1 -> même chose, mais prend le premier élément de la liste comme point de départ
scanl
scanr -> rapportent tous les états de l'accumulateur dans une liste





foldr1 
## Types
:t permet d'afficher le type

:t expr -> expr :: t
X :: T =" X a pour type T"
[T] = "T list" 

ce qui se situe à gauche du signe '=>' correspond à des restriction sur le type/les variables de type.

Souvent, les restrictions sont des conditions
d'appartenance à certaines classes

/= -> non égal ( et pas != ) 

Annotation de types
<e> :: <T> indique que e est de type t

data T = A | B -> Déclaration de type somme

data T = C1 Integer Float | C2 Float Float Char
-> Déclaration de type somme possédant des constructeurs, à peu près comme en Caml
Les constructeurs correspondent à des fonctions prenant un ensemble de valeurs des types spécifiés, et qui retournent une valeur de type T


data T = C1 ... | C2 ... deriving (Class)
-> indique que le type dérive de la classe Class.

Enregistrements :

data T = { c1 :: T1, -> champ 1 de type T1
       	   c2 :: T2
	   ...
	 } [deriving (...)]

>> Pour dériver de classes, le type doit respecter certaines contraintes (par exemple tous les champs doivent dériver de Eq pour que la classe soit Eq)



Types paramétrés:

Type Option t = None | Val t




## Classes Haskell
Correspondent à peu près à des interfaces Java.

Les contraintes de types peuvent correspondre à des classes haskell, dont les types des variables doivent dériver de certaines classes

Classes communes :

	* Enum -> types ordonnés séquentiellement
	  Implémentation de fonctions successeur et prédécesseur avec succ et pred

	* Show 
	show : Show -> str  fonction équivalant à tostring()
	read : str -> Obj  

	* Bounded : Types possédant une borne sup et une borne inf
	  maxBound :: <T>
	  minBound :: <T>
	  . Les tuples sont Bounded si leurs composantes le sont
	 
	 * ...


Création de classes:

class P t where  -> t représente une variable de type 
      f1 :: <type de la fonction>
      ...
      <f1> <param1> <param2> = <définition de la fonction f> (ou relation entre les opérateurs?) 
      
instance Class Type where
	 <f1> v1 v2 = <valeur>

Définit des instances d'une classe de type = implémentation de la classe

class <contrainte> where
      ...
-> Classes paramétrées par des types, et où on peut donner des contraintes sur ces types

e.g
class (Eq a) => Num a where
      ... 

type -> Création d'alias de types, e.g
type String =  [Char]

+ différence entre types concrets et types abstraits(paramétrisés, comme les listes)


## Filtrage par motif

Définition de fonctions, via filtrage 
Déclaration du filtrage de manière claire, correspondant au remplacement du paramètre par une constante:
e.g
```
f 0 = 1
f 1 = 1
f n = f (n-1) + f (n-2) 
```
ou
```
f [] = 0
f x::s = 1 + (f s)
```

+ possibilité de nommer les objets correspondant aux motifs, e.g
```
f l@(x::s)
```


## Gardes

f param
  | <pred> = <val>
  | ...
  | otherwise = <val>
 [where
	p1 = v1
	p2 = v2
  ]?
correspond plus ou moins à un switch avec des formules plutôt que des valeurs

+ possibilité d'ajouter une clause where pour définir des variables dans la garde
+importance de l'indentation des noms de var définies dans le where

Constructions "let... in ..."
```
let a = 3
    b = 5
in  a + b

; : séparateurs

## Case

case <exp> of <pattern> -> <value>
     	      <pattern> -> <value>


## Lambdas / fonctions anonymes

\<paramètres> -> <expression>

$: Opérateur d'application de fonction
-> affaiblit la précédence de l'application et évite d'écrire
  des parenthèses
  
sqrt 4 + 3 = (sqrt 4) + 3
sqrt $ 4 + 3 = sqrt (4 + 3)

## Modules

import <module> -> import des valeurs du module
import <module> (<f>, <g>) -> import des valeurs f et g du module
import qualified <module> as <alias> 

Créatio d'un module

module M
( T1(..)	-> Export de tous les constructeurs de T1
, T2(C1, C2) 	-> Export des constructeurs C1 et C2 de T2 uniquement 
, v1
, v2		-> Export de valeurs vi
)



-> wtf
## Foncteur

class Functor f where
    fmap :: (a -> b) -> f a -> f b
    (<$) :: a -> f b -> f a

