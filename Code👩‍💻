Les transporteurs ayant une part de marché élevée (forte concentration) pratiquent-ils des tarifs plus élevés ?

Section 1: Introduction (Objectif, modèle..)
Description des données: Rapport sur les tarifs des compagnies aériennes nationales publié par le ministère des Transports des États-Unis.
année: 1997, 1998, 1999, 2000 (4ans)
```{r}
library(wooldridge)
data("airfare") 

install.packages("tibble")       
install.packages("dplyr")      
install.packages("stargazer") 

library(tibble)      
library(dplyr)       
library(stargazer) 
```


```{r}
aifaire <- as_tibble(airfare)

xxx <- aifaire %>% select(lfare, concen, dist) 

tableau1 <- stargazer(
  as.data.frame(xxx),
  type = "html",
  title = "Tableau 1 : Statistiques descriptives des principales variables",
  covariate.labels = c(
    "Log-tarif moyen aller simple (en dollars)", 
    "Part de marché (%)", 
    "Distance (en miles)"
  ),
  summary.stat = c("N", "mean", "median", "sd", "min", "max"),
  digits = 2,
  notes = c(
    "On utilisera 'log' pour réduire les écarts",
    "1 mile ≈ 1,609 kilomètres."
  ),
  notes.align = "l",  # Aligne les notes à gauche
  out = "tableau1_notes.html"
)
```
```{r}
# Pour la petite visualisation de la base
head(airfare)
#Sources
# Liste des variables
# Nombre d'observation et la période
```
# Panel
```{r}
library(plm)
# Convertir en Pdata.frame
p_airfare <- pdata.frame(airfare, index = c("id", "year"))
```
# Modèle
lfare ~ concern + ldist + ldistsq + lpassen

# Regression OLS
```{r}
OLS <- lm (lfare ~ concen 
                 + lpassen
                 + ldist 
                 + ldistsq 
                 + y98 + y99 + y00, 
                 
                 data = p_airfare) # quand je met ldist seul, la relation avec lfare est positive mais des que j'ajoute ldstsq la relation devieny néga
                 
summary(OLS)
```
# Estimateur within car effets fixes: indiv + temp

```{r}
# W : il enlève ldis et LES Variable indica. car elle est invariante dans le temps pour chaque individu. 
# Pour trouver les coef ici de ldist; il faudrait faire ldist*y.. mais c'est inutile car il ne seront pas significatif

Within <- plm (lfare ~ concen 
                 + lpassen
                 + ldist 
                 + ldistsq 
                 + y98 + y99 + y00,
                     
                data = p_airfare,
                model = "within",
                effect = "twoways"
                )
summary(Within)
```

# Constante
```{r}

# Moyennes des variables
mean_y <- mean(p_airfare$lfare, na.rm = TRUE)
mean_concen <- mean(p_airfare$concen, na.rm = TRUE)
mean_lpassen <- mean(p_airfare$lpassen, na.rm = TRUE)

# Coefficients estimés du modèle
coef_within <- coef(Within)

# Calcul de la constante
constante <- mean_y - (coef_within["concen"] * mean_concen +
                       coef_within["lpassen"] * mean_lpassen)


# Affichage de la constante
# Affichage de la constante sans le nom
constante_valeur <- unname(constante)
constante_valeur
```

# Estimateur GLS : effets aléatoire : les 02 effets (indiv + temp) sont aléatoire
```{r}
# Le nombre de coef des nouvelle variable (en plus de la constant), doit etre inférieur à l'anné étudier sinon ca fait errer. car il y a trop de variable ou pou peu d'année à cause de Swamy-Arora 

GLS <- plm (lfare ~ concen 
                 + lpassen
                 + ldist 
                 + ldistsq 
                 + y98 + y99 + y00,
                  
                data = p_airfare,
                model = "random",
                effect = "twoways",
                random.method = "amemiya"
                )
summary(GLS)

GLS2 <- plm (lfare ~ concen 
                 + lpassen
                 + ldist 
                 + ldistsq 
                 + y98 + y99 + y00,
                  
                data = p_airfare,
                model = "random",
                effect = "time",
                random.method = "amemiya"
                )
summary(GLS2)


GLS3 <- plm (lfare ~ concen 
                 + lpassen
                 + ldist 
                 + ldistsq 
                 + y98 + y99 + y00,
                  
                data = p_airfare,
                model = "random",
                effect = "individual",
                random.method = "amemiya"
                )
summary(GLS3)
#et donc vaut mieux use la méthode amemiya => prend en compre l'autocorrelation et hétérodéscasticité, et donc swamy-Arora
```
