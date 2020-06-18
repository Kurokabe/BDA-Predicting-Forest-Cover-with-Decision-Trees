# BDA-Predicting-Forest-Cover-with-Decision-Trees

## Description du dataset
Le dataset contient 581'012 ligne de données représentant des parcelles forestières. Il y a 7 types de parcelles : 
1) Spruce/Fir			
2) Lodgepole Pine		
3) Ponderosa Pine		
4) Cottonwood/Willow	
5) Aspen				
6) Douglas-fir			
7) Krummholz			

Chaque ligne contient les 54 features suivantes : 
1) Elevation
2) Aspect
3) Slope
4) Horizontal_Distance_To_Hydrology
5) Vertical_Distance_To_Hydrology
6) Horizontal_Distance_To_Roadways
7) Hillshade_9am
8) Hillshade_Noon
9) Hillshade_3pm
10) Horizontal_Distance_To_Fire_Points
11) Wilderness_Area_[1-4]
12) Soil_Type_[1-40]
13) Cover_Type

## Description of the features used
Globalement, nous utilisons toutes les features. Cependant, certaines ont un impacte plus grands sur la prédiction que d'autres. Voici une idée de l'ordre d'importances avec les pourcentages :

|  %     | Feature                         |
| :----: | ------------------------------- |
| 42.93  | Elevation                       |
| 14.46  | Wilderness_Area_3               |
| 4.78   | Horizontal_Distance_To_Roadways |
| 3.53   | Soil_Type_3                     |
| 3.04   | Soil_Type_11                    |
| ...    | ...                             |

## Questions de recherches
Nous nous sommes demandés quelles sont les données pouvant être utile à visualiser et comment les visualiser. Après implémentation, nous avons vu que le dataset n'était pas équilibré et avons décidé d'afficher cette répartition. Puisqu'un decision tree est utilisé, il est aussi possible d'afficher sous forme de graphe les features les plus utiles.

On s'est ensuite intéressé à l'utilité des features. Est-ce que ne pas utiliser la totalité des features permet d'améliorer le modèle ?

Ensuite par rapport à l'utilisation des arbres de décisions, est-ce qu'avoir autant de données est utile ? Nous avons décidé d'observer le changement dans la précision du modèle lorsqu'on utilise moins de données.

## Algorithmes
Nous avons utilisé deux méthodes pour la prédiction. Celles-si sont les arbres de décisions et les forêts d'arbres décisionnels. Ceux deux méthodes sont des algorythmes supervisés.

### Decision Tree
L'apprentissage par arbres de décisions utilise ces derniers établir une prédiction en prenant une série de décsion en fonction des features et de leur importance.

### Random Forest
Dans les algorithmes de forêts d'arbres décisionnels on utilise plusieurs arbres de décision qui sont tous formés de manière légèrement différente. Ils sont tous pris en compte pour la classification finale.

## Optimizations
L'optimisation des résultats s'est fait en faisant varier les hyper-parameters. Pour cela, un ParamGridBuilder est créé permettant d'indiquer les valeurs des paramètres à tester. Les différentes combinaisons de paramètres sont ensuite testé sur le pipeline fourni et les meilleurs paramètres sont affiché avec l'accuracy.
Dans notre cas, les meilleurs paramètres sont les suivants : 
* Impurity : Entropy
* MaxBins : 40
* MaxDepths : 10
* MinInfoGain : 0.0
Pour une accuracy de **0.7491701323972682**

## Tests et evaluations
Pour tester et évaluer nos modèles, nous avons utilisé l'accuracy qui se calcule en mesurant le nombre de prédictions correctes sur le nombre total. 

Nous obtenons une accuracy qui atteint environ les 74%.

## Résultats des questions
### Quelles données peuvent être utiles à visualiser et comment les visualiser ?
Concernant la répartition du dataset, on peut constater une très grande disproportion. 
![](https://i.imgur.com/rOlgQSY.png)
Deux classes consistent en 85% du dataset alors que les 15% restants sont répartis parmis les 5 autres classes. On peut en déduire qu'avoir une grande quantité de donnée c'est bien, mais encore faut-il que cela soit réparti de manière uniforme. Si une classe est peu représenté, le modèle peut ne pas bien apprendre de celle-ci.

| Index | Probability | Name              |
| ----- | ----------- |------------------ |
| 1     | 0.365       | Spruce/Fir        |
| 2     | 0.488       | Lodgepole Pine    |
| 3     | 0.062       | Ponderosa Pine    |
| 4     | 0.005       | Cottonwood/Willow |
| 5     | 0.016       | Aspen             |
| 6     | 0.029       | Douglas-fir       |
| 7     | 0.035       | Krummholz         |

Pour l'utilité des features : 
![](https://i.imgur.com/GDsUzqR.png)
On peut voir que 2 features sur 54 permet de discriminer 60% des données. La principale de ces features étant l'élévation. On voit donc que beaucoup de features n'apportent finalement que peu d'informations.

En affichant un graphe avec en ordonné l'élévation et en abscisse le Cover_Type, on peut voir que si une élévation est au dessus de 3750m, on est certains à 100% que c'est le Cover_Type 7. Similairement pour une élévation en dessous de 2000, on sait que c'est un Cover_type 3 ou 6. Entre les deux c'est incertain.

![](https://i.imgur.com/S37wu3K.png)

Si l'on affiche avec en abscisse l'Horizontal_Distance_To_Roadways, on distingue mieux les régions d'élévations selon le type de couverture.

![](https://i.imgur.com/1bMZZn7.png)

### Est-ce que toutes les features sont utiles ? Peut-on en supprimer afin d'améliorer le modèle ?
Après avoir exécuté le modèle en augmentant le nombre de features utilisés : 

![](https://i.imgur.com/Arfp3JH.png)

On peut voir que la précision varie très peu selon si l'on utilise 1 features ou les 54 features à disposition. Néanmoins, avec 6 features, on arrive à un très bon résultat puisqu'on peut représenter 70% des données. Plus on ajoute de features, plus elles peuvent se contredire entre-elle dans les decisions tree ou overfiter sur les données d'entrainement.

### Quel est l'impact de la quantité de données sur les performances de l'arbre de décision ?
Nous avons essayé de faire varier la taille des données d'entraînement et de test et voici les résultats : 

![](https://i.imgur.com/ppIthfj.png)

On voit là aussi que les résultats varient peu entre 20% des données pour l'entraînement ou 90%. On peut déduire que pour ce cas d'utilisation, environ 100'000 données sont largement suffisantes pour avoir de bon résultat. Avoir plus de données n'améliore pas significativement le modèle et cause un temps d'entraînement plus long.

## Améliorations futures possibles

### Utiliser un dataset equilibré
Nous avons remarqué que le dataset n'est pas du tout équilibré. Il serait donc envisageable de supprimer certaines données afin de l'équilibrer. Ceci aurait également pour conséquence de réduire grandement le temps d'apprentissage.
De plus nous avons constaté que la quantié de données n'a pas un impact très grand sur les performances du modèle.
