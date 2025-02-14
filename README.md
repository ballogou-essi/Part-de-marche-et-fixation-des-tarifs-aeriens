# Analyse de l'effet de la part de marché des compagnies aériennes sur les tarifs aériens

**Objectif**

Cette étude examine si les transporteurs ayant une part de marché élevée pratiquent des tarifs plus élevés sur les routes aériennes aux États-Unis.

**Données**
 - Source : Rapport sur les tarifs des compagnies aériennes nationales (U.S. Department of Transportation)
 - Période : 1997-2000 (4 ans)
 - Nombre d'observations : 4596
Variables principales :
 - fare : Tarif moyen aller simple ($)
 - bmktshr : Part de marché du plus grand transporteur
 - passen : Nombre moyen de passagers par jour
 - dist : Distance (miles) et ldist, ldistsq : Log et log au carré de la distance
 - lfare : Log du tarif moyen
 - Variables indicatrices pour les années : y98, y99, y00
 - concen : Concentration du marché (bmktshr)
 - lpassen : Log du nombre de passagers

**Résultats**
 - Une part de marché élevée est associée à des tarifs plus élevés.
 - La distance a un effet non linéaire sur les tarifs :
 - Les courts trajets sont plus coûteux (coûts fixes)
 - L'effet s'atténue pour les longues distances
 - Les routes avec un trafic plus élevé ont des tarifs plus bas (effets d'échelle).
 - Les effets fixes individuels expliquent une grande partie de la variation des tarifs.

**Limites**
 - Possible endogénéité des variables (ex. distance corrélée à des facteurs non observés)
 - Données limitées aux itinéraires nationaux des États-Unis

**Packages R utilisés**
install.packages("tibble")       # Manipulation de tableaux de données
install.packages("dplyr")        # Traitement des données
install.packages("stargazer")    # Présentation des résultats de régression

library(wooldridge)  # Importation des données airfare
library(tibble)
library(dplyr)
library(stargazer)

*Source des données*

Dataset Airfare - Wooldridge
