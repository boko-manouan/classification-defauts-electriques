# Classification de défauts électriques sur une ligne de transport

Reconnaître le type d'un défaut (phase-terre, biphasé, triphasé…) à partir des courants et des tensions d'une ligne de transport simulée, et surtout vérifier si le score obtenu est fiable.

Avec un découpage classique des données, le modèle atteint **99,7 %** de précision. En regardant de plus près, ce score vient d'une fuite de données et d'un artefact de simulation. Évalué de façon plus honnête, avec un bruit de mesure réaliste, le modèle final atteint **82,7 %**, et ses erreurs restantes s'expliquent par la physique du réseau.

**[Voir le notebook](classification_defauts.ipynb)** · [Version Kaggle](https://www.kaggle.com/code/bokomanouan/mod-le-de-classification-de-d-fauts-lectriques)

## Résultats

| Étape | Précision |
|---|---|
| Premier modèle, découpage au hasard | 87,4 % |
| + courant résiduel `Isomme`, découpage au hasard | 99,7 % |
| Découpage temporel | 82,5 % |
| Découpage temporel + 1 % de bruit | 73,8 % |
| **Modèle final : 5 classes, bruit, découpage temporel** | **82,7 %** |

## Ce que montre le projet

- **Une fuite de données :** les lignes du fichier sont des instants successifs, espacés de 50 µs. Un découpage au hasard place des instants presque identiques dans l'entraînement et dans le test, ce qui gonfle le score.
- **La physique comme outil de vérification :** le courant résiduel (Ia + Ib + Ic) sert de variable d'entrée, la somme nulle des tensions montre qu'il s'agit de tensions composées, et les erreurs du modèle correspondent à l'élimination du défaut phase par phase.
- **Un bon score ne garantit pas une bonne raison :** la séparation des défauts triphasés avec et sans terre reposait sur un décalage de 0,2 % dû à la simulation, qui disparaît avec un bruit de mesure de 1 %.

## Données

[Electrical Fault Detection and Classification](https://www.kaggle.com/datasets/esathyaprakash/electrical-fault-detection-and-classification) (licence CC0) : une ligne de transport simulée sous MATLAB/Simulink, alimentée par quatre générateurs de 11 kV, avec des défauts provoqués au milieu de la ligne. Le fichier `data/classData.csv` contient 7 861 instants : les courants des trois phases, les tensions entre phases et le type de défaut (6 classes).

## Limites

- Un seul enregistrement court par type de défaut (environ 3 périodes) : le modèle n'est pas testé dans d'autres conditions (résistance de défaut, instant d'apparition, emplacement).
- Les étiquettes décrivent le scénario entier, et non l'état réel à chaque instant.

## Lancer le projet

```bash
git clone https://github.com/manouGitHub79/classification-defauts-electriques.git
cd classification-defauts-electriques
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate
pip install -r requirements.txt
```

Ouvrir ensuite `classification_defauts.ipynb` et exécuter toutes les cellules.

## Outils

Python · pandas · NumPy · scikit-learn · Matplotlib · seaborn · Git

## Auteur

Boko Manouan, étudiant en Master 2 EEA, parcours Énergie électrique, à l'Université de Lorraine · [LinkedIn](https://www.linkedin.com/in/boko-manouan)# Classification de défauts électriques sur une ligne de transport