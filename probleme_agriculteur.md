# Probléme d'agriculteur

![Poster](https://github.com/walidzbiri/programmation-linear/blob/main/1.PNG)


## Résolution du probléme en python
```python
from pulp import LpMaximize, LpProblem, LpStatus, LpVariable
# Creation du probléme
prob = LpProblem(name="prob-agriculteur", sense=LpMaximize)
# Initialisation des variables de décision
x1 = LpVariable(name="x1", lowBound=0,cat="Integer")
x2 = LpVariable(name="x2", lowBound=0,cat="Integer")
x3 = LpVariable(name="x3", lowBound=0,cat="Integer")
x4 = LpVariable(name="x4", lowBound=0,cat="Integer")
x5 = LpVariable(name="x5", lowBound=0,cat="Integer")
# Ajouter les contraintes au probléme
prob += (x1+x2+x3+x4+x5 <= 16, "max_16_hectare_cultive")
prob += (x1+x2+2*x4 <= 8, "quantite_engres_e1_8")
prob += (x1+2*x3 <= 4, "quantite_engres_e2_4")
prob += (2*x2+x5 <= 9, "quantite_engres_e3_9")
# Ajouter la fonction objective au probléme
prob += 3*x1 + 4*x2 + x3 + 7*x4 + 2*x5
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
Aprés exécution du code au dessus on aura comme résultat:
```sh
prob-agriculteur:
MAXIMIZE
3*x1 + 4*x2 + 1*x3 + 7*x4 + 2*x5 + 0
SUBJECT TO
max_16_hectare_cultive: x1 + x2 + x3 + x4 + x5 <= 16

quantite_engres_e1_8: x1 + x2 + 2 x4 <= 8

quantite_engres_e2_4: x1 + 2 x3 <= 4

quantite_engres_e3_9: 2 x2 + x5 <= 9

VARIABLES
0 <= x1 Integer
0 <= x2 Integer
0 <= x3 Integer
0 <= x4 Integer
0 <= x5 Integer
_______________________
status: 1, Optimal
Valeur de la fonction objectif: 48.0
la valeur des variables aprés résolution
x1: 0.0
x2: 0.0
x3: 2.0
x4: 4.0
x5: 9.0
_______________________
```
Donc pour maximiser son bénéfice, l’agriculteur doit consacrer:
  - 2 hectares pour le céreale P3
  - 4 hectares pour le céreale P4
  - 9 hectares pour le céreale P5
Pour génerer un bénefice maximale de : 48 x 10² €
  
