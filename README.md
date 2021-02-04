# La Programmation Linéaire

![Poster](https://github.com/walidzbiri/programmation-linear/blob/main/1.PNG)

C'est la résolution d'un problème mathématique qui consiste à **optimiser** (maximiser ou minimiser) une fonction linéaire de plusieurs variables qui sont reliées par des relations linéaires appelées contraintes.


## Pourquoi la programmation linéaire ?

La programmation linéaire est une technique d’optimisation fondamentale qui est utilisée depuis des décennies dans les domaines à forte intensité de sciences et de mathématiques. Elle est précise, relativement rapide et adaptée à une gamme d’applications pratiques.

## La programmation linéaire en Python

La méthode la plus populaire pour résoudre des problémes de programmation linéaire est appelée: la méthode **Simplex**.
Il faut pas oublier de mentionner que presque toutes les bibliothèques de programmation linéaire sont natives et écrites en Fortran, C ou C++. Car la programmation linéaire nécessite un travail intensif en calcul à cause des matrices larges. Les outils Python ne sont que des wrappers autour du code C native.
Dans cet article on va utiliser la librairie: **PuLP**. 

## Exemple de résolution d'un probléme de programmation linéaire en python
![Prob](https://github.com/walidzbiri/programmation-linear/blob/main/Capture%20d%E2%80%99%C3%A9cran%20(80).png)

On cherche x et y telles que toute les conditions sont vérifiées et en même temps la solution doit correspondre à la valeur maximale de z.


On peut visualiser le probléme de cette maniére:

![Visu](https://github.com/walidzbiri/programmation-linear/blob/main/Capture%20d%E2%80%99%C3%A9cran%20(78).png)
```python
from pulp import LpMaximize, LpProblem, LpStatus, LpVariable
# Creation du probléme
prob = LpProblem(name="small-problem", sense=LpMaximize)
# Initialisation des variables de décision
x = LpVariable(name="x", lowBound=0,cat="Integer")
y = LpVariable(name="y", lowBound=0,cat="Integer")
# Ajouter les contraintes au probléme
prob += (2 * x + y <= 20, "contrainte_rouge")
prob += (-4 * x + 5 * y <= 10, "contrainte_bleue")
prob += (-x + 2 * y >= -2, "contrainte_jaune")
# Ajouter la fonction objective au probléme
prob += x + 2 * y
# Afficher le probléme avant le résoudre:
print(prob)
# Résoudre le probléme
solution = prob.solve()

print(f"status: {prob.status}, {LpStatus[prob.status]}")
print(f"Valeur de la fonction objectif: {prob.objective.value()}")

print("la valeur des variables aprés résolution")
for var in prob.variables():
	print(f"{var.name}: {var.value()}")
```
Par défaut PuLP utilise le solver **CBC(Coin-or branch and cut)**. C'est un solver de programmation linéaire OpenSource écrit en C++. Donc en gros la plupart des librairies sont des wrappers (enveloppes) d'un code native soit C++, C ou Fortran.
Aprés exécution du code au dessus on aura comme résultat:
```sh
small-problem:
MAXIMIZE
1*x + 2*y + 0
SUBJECT TO
contrainte_rouge: 2 x + y <= 20

contrainte_bleue: - 4 x + 5 y <= 10

contrainte_jaune: - x + 2 y >= -2

VARIABLES
0 <= x Integer
0 <= y Integer
_______________________
status: 1, Optimal
Valeur de la fonction objectif: 19.0
la valeur des variables aprés résolution
x: 7.0
y: 6.0
_______________________
```
