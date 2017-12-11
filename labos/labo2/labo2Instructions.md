---
title: "T.P. Variables aléatoires et inférence statistique (Labo 2) : Instructions"
author: "Dominique Goyette et Marc-André Désautels"
date: "2017-12-11"
output:
  html_document:
    keep_md: true
    toc: yes
    toc_float: yes
  pdf_document:
    toc: no
    latex_engine: pdflatex
subtitle: "201-9F6-ST : Statistiques appliquées à l'informatique"
institute: "Cégep Saint-Jean-sur-Richelieu"
urlcolor: blue
---



# Instructions:

Le but de ce T. P. est de vous familiariser avec le langage `R`. Il vous faudra trouver et utiliser les commandes appropriées
pour répondre aux questions. Vous devez vous aider de la documentation fournie dans le logiciel `RStudio` ou de la recherche `Google`. De plus, ce document contient de multiples exemples qui vous permettront de répondre aux questions se trouvant dans votre T.P.

## Installer `R` et `RStudio`

Vous pouvez télécharger `R` aux adresses suivantes:

- Pour [Linux](http://cran.utstat.utoronto.ca/bin/linux/)
- Pour [(Mac) OS X](http://cran.utstat.utoronto.ca/bin/macosx/)
- Pour [Windows](http://cran.utstat.utoronto.ca/bin/windows/)

Une fois le logiciel `R` installé, vous pouvez télécharger et installer le logiciel `RStudio` à l'adresse suivante:

- Pour [Linux, (Mac) OS X et Windows](https://www.rstudio.com/products/rstudio/download/)

# Les lois de probabilités

Chaque distribution en `R` possède quatre fonctions qui lui sont associées. Premièrement, la fonction possède un _nom racine_ (qui correspond au nom de la `loi`), par exemple le _nom racine_ pour la distribution *binomiale* est `binom`. Cette racine est précédée par une de ces quatre lettre:

- `p` pour *probabilité*, qui représente la fonction de répartition
- `q` pour *quantile*, l'inverse de la fonction de répartition
- `d` pour *densité*, la fonction de densité de la distribution
- `r` pour *random* ou *simulation*, une variable aléatoire suivant la distribution spécifiée.

Pour la loi binomiale (_nom racine_ `binom`) par exemple, ces fonctions sont `pbinom`, `qbinom`, `dbinom` et `rbinom`.

Nous avons donc:

|Loi: `loi`|Densité|Fonction de répartition|Quantile|Simulation|
|:--------:|:-----:|:---------------------:|:------:|:--------:|
|Notations|$f(x)$ ou $P(X=x)$|$F(x)$|valeur liée à $F(x)$|$x_1$, $x_2$, ..., $x_n$|
|Commandes|`dloi`|`ploi`|`qloi`|`rloi`|

Les noms de lois les plus célèbres sont : `norm` (pour la loi normale), `norm` (pour la loi binomiale), `unif` (pour la loi uniforme), `geom` (pour la loi géométrique), `pois` (pour la loi de Poisson), `t` (pour la loi de Student), `chisq` (pour la loi du Chi-deux), `exp` (pour la loi exponentielle), `f` (pour la loi de Fisher)...

## Commandes

Si la loi de $X$ dépend d’un ou de plusieurs paramètres, disons `par1` et `par2`, alors la densité de $X$ en $x$ est donnée par la commande : `dloi(x, par1, par2)`

Quelques exemples sont décrits ci-dessous:

|Loi|Binomiale|Géométrique|Poisson|
|:----------------:|:-----------------------:|:------------------------------:|:-------------------------:|
|Paramètres|$n\in\mathbb{N}$, $p\in]0,1[$|$p\in]0,1[$|$\lambda>0$|
|$X\sim$|$B(n;p)$|$G(p)$|$Po(\lambda)$|
|Ch($X$)|$\left\{0,1,\ldots,n\right\}$|$\mathbb{N}$|$\mathbb{N}$|
|$P(X=x)$|$C_x^np^xq^{n-x}$|$p(1-p)^x$|$e^{-\lambda}\frac{\lambda^x}{x!}$|
|Commandes|`dbinom(x,n,p)`|`dgeom(x,p)`|`dpois(x,lambda)`|

|    Loi   |     Uniforme    |      Exponentielle     |                  Normale                          |
|:----------------:|:-----------------------:|:------------------------------:|:-------------------------:|
|Paramètres|$(a,b)\in\mathbb{R}^2$|$p\in]0,1[$|$\lambda>0$|
|$X\sim$ | $U([a,b])$ | $E(\lambda)$ | $N(\mu,\sigma^2)$ |
|Ch($X$) | $[a,b]$ | $[0,\infty]$ | $\mathbb{R}$ |
|$P(X=x)$ | $\frac{1}{b-a}$ | $\lambda^{-\lambda x}$ | $\dfrac{1}{\sqrt{2\pi \sigma^2}}e^{\frac{-(x-\mu)^2}{2\sigma^2}}$|
|Commandes | `dunif(x,a,b)` | `dexp(x,lambda)` | `dnorm(x,mu,sigma)` |

## Exemples de calculs

Soit $X$ une variable aléatoire telle que $X\sim B(8,0.3)$.

1. Pour calculer $P(X=4)$, nous devons utiliser la commande suivante:

```r
dbinom(4,8,0.3)
```

```
## [1] 0.1361367
```
Ceci signifie que $P(X=4)=0.1361367$.

2. Pour calculer $P(X\leq 4)$, nous devons utiliser la commande suivante:

```r
pbinom(4,8,0.3)
```

```
## [1] 0.9420324
```
Ceci signifie que $P(X\leq 4)=0.9420324$.

3. Pour calculer $P(X> 4)$, nous pouvons utiliser une des commandes suivantes:

```r
pbinom(4,8,0.3,lower.tail = FALSE)
```

```
## [1] 0.05796765
```

```r
1-pbinom(4,8,0.3)
```

```
## [1] 0.05796765
```
Ceci signifie que $P(X>4)=0.0579676$.

4. Pour calculer $P(X\geq 4)=1-P(X\leq 3)$, nous pouvons utiliser la commande suivante:

```r
1-pbinom(3,8,0.3)
```

```
## [1] 0.1941043
```
Ceci signifie que $P(X\geq 4)=0.1941043$.

## Représentation graphique

### Les lois de probabilités discrètes

Nous pouvons représenter graphiquement la loi binomiale. Soit $X~B(8,0.3)$. Nous aurons:


```r
n <- 8
p <- 0.3
fbinom <- data.frame(x = 0:n, y = dbinom(0:n, n, p))
ggplot(fbinom, aes(x = x, y = y)) +
  geom_bar(width = 0.05, stat = "identity", fill = "blue") +
  labs(
    x = "Nombre de succès",
    y = "Probabilité",
    title = "Fonction de densité de B(8,0.3)"
  )
```

![](labo2Instructions_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

### Les lois de probabilités continues

Nous pouvons représenter graphiquement la loi normale. Soit $X\sim N(5,1.5^2)$. Nous aurons:


```r
ggplot(data = data.frame(x = c(0, 10)), aes(x)) +
  stat_function(fun = dnorm, args = list(mean = 5, sd = 1.5), colour = "blue") +
  labs(
    x = "x",
    y = "f(x)",
    title = "Fonction de densité de N(5,1.5^2)"
  )
```

![](labo2Instructions_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

Nous pouvons également superposer plusieurs fonctions de densité. Par exemple, nous allons représenter la loi $N(10, 3^2)$ et la loi $N(8,5^2)$ sur le même graphique.


```r
ggplot(data = data.frame(x = c(-5, 20)), aes(x)) +
  stat_function(fun = dnorm, args = list(mean = 10, sd = 3), colour = "blue") +
  stat_function(fun = dnorm, args = list(mean = 8, sd = 5), colour = "red") +
  labs(
    x = "x",
    y = "f(x)",
    title = "Les densités de N(10,3^2) et de N(8,5^2)"
  )
```

![](labo2Instructions_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

Nous pouvons aussi superposer une variable aléatoire discrète et une variable aléatoire continue. Dans l'exemple suivant, nous avons la loi $B(100,0.2)$ et son approximation par la loi normale $N(20,4^2)$.


```r
n <- 100
p <- 0.2
m <- n*p
s <- sqrt(n*p*(1-p))
fbinom <- data.frame(x = 0:n, y = dbinom(0:n, n, p))
ggplot(fbinom, aes(x = x, y = y)) +
  geom_bar(width = 0.1, stat = "identity", colour = "blue") +
  stat_function(fun = dnorm, args = list(mean = m, sd = s), colour = "red") +
  labs(
    x = "Nombre de succès",
    y = "Probabilité",
    title = "La loi B(100,0.2) et la loi N(20,4^2)"
  )
```

![](labo2Instructions_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

## Créer un tableau de fréquences

Pour créer un tableau de fréquences pour une variable aléatoire qualitative ou quantitative discrète, nous utilisons la commande `table` ou la commande `freq` de la librairie `questionr`.

### Variable aléatoire qualitative ou quantitative discrète

Créez un tableau de la variable `cut` de la base de données `diamonds`.

```r
table(diamonds$cut)
```

```
## 
##      Fair      Good Very Good   Premium     Ideal 
##      1610      4906     12082     13791     21551
```

Nous pouvons également utiliser la commande `freq` de la librairie `questionr`.

```r
freq(diamonds$cut,
     exclud = NA,
     digits = 2,
     cum = TRUE,
     total = TRUE)
```

```
##               n      %   %cum
## Fair       1610   2.98   2.98
## Good       4906   9.10  12.08
## Very Good 12082  22.40  34.48
## Premium   13791  25.57  60.05
## Ideal     21551  39.95 100.00
## Total     53940 100.00 100.00
```

L'option `exclud = NA` permet d'exclure les valeurs manquantes, l'option `cum = TRUE` permet d'afficher les pourcentages cumulés et l'option `total = TRUE` permet d'ajouter le total.

### Variable aléatoire quantitative continue

Nous allons créer un tableau de fréquences d'une variable quantitative continue. Pour débuter, nous devons transformer cette variable quantitative en variable qualitative en la découpant en classe à l'aide de la commande `cut`. La commande `seq(from = 0, to = 20000, by = 2000)` permet de découper nos valeurs en partant de 0 jusqu'à 20 000 par sauts de 2 000. Nous incluons la côté gauche et excluons le côté droit.


```r
Prix <- cut(diamonds$price, breaks = seq(from = 0, to = 20000, by = 2000), 
            include.lowest = TRUE, right = FALSE)
```

Nous voulons ensuite renommer nos classes à l'aide de la commande `levels`.


```r
levels(Prix) <- c("0 à 2000$","2000 à 4000$","4000 à 6000$","6000 à 8000$",
                  "8000 à 10000$","10000 à 12000$","12000 à 14000$","14000 à 16000$",
                  "16000 à 18000$","18000 à 20000$")
```

Nous pouvons maintenant afficher le tableau à l'aide de la commande `freq`.


```r
freq(Prix,
     exclud = NA,
     digits = 2,
     cum = TRUE,
     total = TRUE)
```

```
##                    n      %   %cum
## 0 à 2000$      24203  44.87  44.87
## 2000 à 4000$   10357  19.20  64.07
## 4000 à 6000$    7827  14.51  78.58
## 6000 à 8000$    3947   7.32  85.90
## 8000 à 10000$   2383   4.42  90.32
## 10000 à 12000$  1759   3.26  93.58
## 12000 à 14000$  1305   2.42  96.00
## 14000 à 16000$  1017   1.89  97.88
## 16000 à 18000$   830   1.54  99.42
## 18000 à 20000$   312   0.58 100.00
## Total          53940 100.00 100.00
```

## Tableau de contingence

Nous voulons représenter le tableau de contingence de la variable `color` et de la variable `cut`. Nous utilisons la commande `table` et la commande `addmargins` pour ajouter une colonne et une ligne total.


```r
addmargins(table(diamonds$color,diamonds$cut))
```

```
##      
##        Fair  Good Very Good Premium Ideal   Sum
##   D     163   662      1513    1603  2834  6775
##   E     224   933      2400    2337  3903  9797
##   F     312   909      2164    2331  3826  9542
##   G     314   871      2299    2924  4884 11292
##   H     303   702      1824    2360  3115  8304
##   I     175   522      1204    1428  2093  5422
##   J     119   307       678     808   896  2808
##   Sum  1610  4906     12082   13791 21551 53940
```

Nous pouvons utiliser la commande `prop` avec la commande `table` pour obtenir un tableau des fréquences relatives.


```r
prop(table(diamonds$color,diamonds$cut),digits=2)
```

```
##        
##         Fair   Good   Very Good Premium Ideal  Total 
##   D       0.30   1.23   2.80      2.97    5.25  12.56
##   E       0.42   1.73   4.45      4.33    7.24  18.16
##   F       0.58   1.69   4.01      4.32    7.09  17.69
##   G       0.58   1.61   4.26      5.42    9.05  20.93
##   H       0.56   1.30   3.38      4.38    5.77  15.39
##   I       0.32   0.97   2.23      2.65    3.88  10.05
##   J       0.22   0.57   1.26      1.50    1.66   5.21
##   Total   2.98   9.10  22.40     25.57   39.95 100.00
```

# Estimation de paramètres et tests d'hypothèses

## Estimation de paramètres

### Les intervalles de confiance sur une moyenne

Nous allons trouver un intervalle de confiance au niveau de 95% de la moyenne du prix des diamants.

```r
ConfPrice95 <- t.test(diamonds$price,conf.level = 0.95)
tidy(ConfPrice95)
```

```
##   estimate statistic p.value parameter conf.low conf.high
## 1   3932.8  228.9525       0     53939 3899.132  3966.467
##              method alternative
## 1 One Sample t-test   two.sided
```

Au niveau de confiance de 95%, le prix moyen des diamants se situe entre 3899.1319579 et 3966.4674859.

Nous allons trouver un intervalle de confiance au niveau de 99% de la moyenne du prix des diamants.

```r
ConfPrice99 <- t.test(diamonds$price,conf.level = 0.99)
tidy(ConfPrice99)
```

```
##   estimate statistic p.value parameter conf.low conf.high
## 1   3932.8  228.9525       0     53939 3888.552  3977.047
##              method alternative
## 1 One Sample t-test   two.sided
```
Au niveau de confiance de 99%, le prix  moyen des diamants se situe entre 3888.5522068 et 3977.047237.

### Les intervalles de confiance sur une proportion

Trouvons la proportion de diamants de type `Ideal`.

```r
prop.table(table(diamonds$cut))
```

```
## 
##       Fair       Good  Very Good    Premium      Ideal 
## 0.02984798 0.09095291 0.22398962 0.25567297 0.39953652
```

La proportion est donc de $0.3995365$. Nous allons faire trouver un intervalle de confiance au niveau de 95% de la proportion dans la population des diamants de type `Ideal`.

```r
ConfCut95 <- prop.test(with(diamonds,table(cut!="Ideal")),conf.level = 0.95)
tidy(ConfCut95)
```

```
##    estimate statistic p.value parameter  conf.low conf.high
## 1 0.3995365  2177.245       0         1 0.3954011 0.4036863
##                                                 method alternative
## 1 1-sample proportions test with continuity correction   two.sided
```

Au niveau de confiance de 95%, la proportion moyenne des diamants de type `Ideal` se trouve entre 0.3954011 et 0.4036863.

Pour trouver un intervalle de confiance à 99%.

```r
ConfCut99 <- prop.test(with(diamonds,table(cut!="Ideal")),conf.level = 0.99)
tidy(ConfCut99)
```

```
##    estimate statistic p.value parameter  conf.low conf.high
## 1 0.3995365  2177.245       0         1 0.3941077 0.4049901
##                                                 method alternative
## 1 1-sample proportions test with continuity correction   two.sided
```

Au niveau de confiance de 99%, la proportion moyenne des diamants de type `Ideal` se trouve entre 0.3941077 et 0.4049901.

# Test d'hypothèses

## Le test d'hypothèses sur une moyenne

Nous pouvons faire un test d'hypothèses bilatéral de niveau de confiance 95% sur la moyenne du prix des diamants. Par exemple, nous allons tenter de vérifier si le prix des diamants est **différent** de 3 900$.

```r
PrixDiff <- t.test(diamonds$price, 
            mu = 3900,
            alternative = "two.sided",
            paired = FALSE, 
            var.equal = FALSE, 
            conf.level = 0.95)
tidy(PrixDiff)
```

```
##   estimate statistic    p.value parameter conf.low conf.high
## 1   3932.8  1.909474 0.05620629     53939 3899.132  3966.467
##              method alternative
## 1 One Sample t-test   two.sided
```
Au niveau de confiance de 95%, nous ne pouvons pas conclure que le prix des diamants est différent de 3 900$ car nous obtenons une __p-value__ de $5.6206287$%. 

Nous pouvons vérifier si le prix des diamants est **plus grand** que 3 900$ au niveau de confiance de 90%.

```r
PrixPlusGrand <- t.test(diamonds$price, 
                  mu = 3900,
                  alternative = "greater",
                  paired = FALSE, 
                  var.equal = FALSE, 
                  conf.level = 0.90)
tidy(PrixPlusGrand)
```

```
##   estimate statistic    p.value parameter conf.low conf.high
## 1   3932.8  1.909474 0.02810314     53939 3910.786       Inf
##              method alternative
## 1 One Sample t-test     greater
```
Au niveau de confiance de 90%, nous pouvons conclure que le prix des diamants est plus grand que 3 900$ car nous obtenons une __p-value__ de $2.8103143$%. 

## Le test d'hypothèses sur une proportion

Nous pouvons faire un test d'hypothèses unilatéral de niveau de confiance 95% sur la proportion de diamants de type `Ideal`. Par exemple, nous allons tenter de vérifier si la proportion des diamants de type `Ideal` est **plus petite** que 0,405.

```r
IdealPlusPetit <- prop.test(with(diamonds,table(cut!="Ideal")),
                    p = 0.405,
                    alternative = "less",
                    conf.level = 0.95)
tidy(IdealPlusPetit)
```

```
##    estimate statistic     p.value parameter conf.low conf.high
## 1 0.3995365  6.658899 0.004933094         1        0 0.4030197
##                                                 method alternative
## 1 1-sample proportions test with continuity correction        less
```
Au niveau de confiance de 95%, nous pouvons conclure que la proportion de diamants de type `Ideal` est plus petite que 0,405 car nous obtenons une __p-value__ de $0.4933094$%.


## Les tests d'hypothèses sur une différence de deux moyennes

Nous pouvons faire un test d'hypothèses sur la différence entre le prix moyen des diamants de coupe `Ideal` et de coupe `Premium` au niveau de confiance  de 99%.

```r
IdealPremiumDiff <- t.test(formula = price ~ cut,
                      data = diamonds,
                      subset = cut %in% c("Ideal", "Premium"),
                      alternative = "two.sided",
                      paired = FALSE,
                      var.equal = FALSE,
                      conf.level = 0.99)
tidy(IdealPremiumDiff)
```

```
##   estimate estimate1 estimate2 statistic       p.value parameter conf.low
## 1 1126.716  4584.258  3457.542  24.91787 1.718905e-135  26552.16 1010.236
##   conf.high                  method alternative
## 1  1243.196 Welch Two Sample t-test   two.sided
```
Au niveau de confiance de 99%, nous pouvons conclure que la moyenne de prix des diamants `Ideal` est différente de la moyenne de prix des diamants `Premium` car nous obtenons une __p-value__ de $1.7189047\times 10^{-133}$%.

Pour faire un test d'hypothèses sur une différence de moyennes lorsque les échantillons sont pairés, nous allons utiliser une base de données disponible dans `R`, la base de données `immer`. Celle-ci donne la production d'orge pour les années 1931 et 1932. On peut la visualiser en utilisant la commande `head`.

```r
head(immer)
```

```
##   Loc Var    Y1    Y2
## 1  UF   M  81.0  80.7
## 2  UF   S 105.4  82.3
## 3  UF   V 119.7  80.4
## 4  UF   T 109.7  87.2
## 5  UF   P  98.3  84.2
## 6   W   M 146.6 100.4
```

Nous allons faire un test d'hypothèses bilatéral sur la différence de production d'orge entre les années 1931 et 1932 au niveau de confiance de 95%.

```r
BarleyPaired <- t.test(immer$Y1, 
                       immer$Y2,
                       paired=TRUE)
tidy(BarleyPaired)
```

```
##   estimate statistic     p.value parameter conf.low conf.high
## 1 15.91333  3.323987 0.002412634        29 6.121954  25.70471
##          method alternative
## 1 Paired t-test   two.sided
```
Au niveau de confiance de 95%, nous pouvons conclure que la moyenne de production d'orge est différente entre 1931 et 1932 car nous obtenons une __p-value__ de $0.2412634$%.

## Les tests d'hypothèses sur une différence de deux proportions

Nous pouvons faire un test sur la différence de poportions entre les diamants de coupe `Ideal` et les diamants de couleur `E`.

```r
PropPremiumE <- prop.test(with(diamonds,table(cut == "Premium",color == "E")))
tidy(PropPremiumE)
```

```
##   estimate1 estimate2 statistic      p.value parameter    conf.low
## 1 0.8141921 0.8305417  18.35038 1.837823e-05         1 -0.02372478
##      conf.high
## 1 -0.008974267
##                                                                 method
## 1 2-sample test for equality of proportions with continuity correction
##   alternative
## 1   two.sided
```
Au niveau de confiance de 95%, nous pouvons conclure que la proportion de diamants `Ideal` et de diamants de couleur `E` est différente car nous obtenons une __p-value__ de $0.0018378$%.

## Le test du $\chi^2$ pour une variable

Voici le tableau représentant la variable `cut`.

```r
table(diamonds$cut)
```

```
## 
##      Fair      Good Very Good   Premium     Ideal 
##      1610      4906     12082     13791     21551
```

Nous voulons faire un test du $\chi^2$ pour savoir si toutes les modalités de la variable `cut` sont présentes de façon égales.

```r
ChiCut <- chisq.test(x = table(diamonds$cut))
tidy(ChiCut)
```

```
##   statistic p.value parameter                                   method
## 1  22744.55       0         4 Chi-squared test for given probabilities
```

Au niveau de confiance de 95%, nous pouvons conclure que la variable `cut` ne suit pas une loi untiforme car nous obtenons une __p-value__ de $0$%.

## Le test du $\chi^2$ pour deux variables

Voici le tableau représentant la variable `cut` et la variable `color`.

```r
table(diamonds$cut,diamonds$color)
```

```
##            
##                D    E    F    G    H    I    J
##   Fair       163  224  312  314  303  175  119
##   Good       662  933  909  871  702  522  307
##   Very Good 1513 2400 2164 2299 1824 1204  678
##   Premium   1603 2337 2331 2924 2360 1428  808
##   Ideal     2834 3903 3826 4884 3115 2093  896
```

Nous voulons faire un test du $\chi^2$ pour savoir si la  variable `cut` dépend de la variable `color`.

```r
ChiCutColor <- chisq.test(x = table(diamonds$cut, diamonds$color))
tidy(ChiCutColor)
```

```
##   statistic      p.value parameter                     method
## 1  310.3179 1.394512e-51        24 Pearson's Chi-squared test
```

Au niveau de confiance de 95%, nous pouvons conclure que les variables `cut` et `color` sont dépendantes car nous obtenons une __p-value__ de $1.3945121\times 10^{-49}$%.
