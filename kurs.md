R: Databehandling, analyse og visualisering
========================================================
author: Eivind Moe Hammersmark
date: 06 januar, 2021
autosize: true
font-family: "TW Cen MT"

Hvorfor R?
================================================================================
left: 40 %
Tre sentrale arbeidsoppgaver i Oslo Economics: 
- Databehandling
- Analyse
- Visualisering

For de to første punktene er det særlig viktig med dokumenterbarhet og etterprøvbarhet. 

Det er her R-script (og do-filer og python-script) kommer inn.

Også for visualisering (f.eks. diagrammer eller kart) kan det være nyttig å ha et script.


Hvorfor R?
================================================================================

|  | Databehandling | Analyse        | Diagrammer    | Kart      |
|---------|----------------|----------------|---------------|-----------|
| Excel   | omg just don't | omg just don't | Ja            | Begrenset |
| Stata   | Ja             | Ja             | Ja            | Begrenset |
| Python  | Ja             | Ja             | Ja            | Ja        |
| R       | Ja             | Ja             | Ja            | Ja        |
| QGIS    | Begrenset      | Nei            | Nei           | Ja        |

Velg **R** eller Python for allsidighet, dokumenterbarhet og etterprøvbarhet. Velg **R** eller Stata dersom du først og fremst skal kjøre regresjoner. 

Velg Excel for å lage diagrammer, og dersom du trenger kjappe og enkle analyser. Velg QGIS for å lage pene kart, men vær oppmerksom på at det kan være tidkrevende dersom du trenger å gjøre det på nytt (f.eks. på andre data).

**R kan gjøre stort sett alt som Excel, Stata, Python og QGIS kan.**

Hva vil du lære på dette kurset?
================================================================================

- **R basics**: Kodeprinsipper og vanligste funksjoner
- **Arbeidsflyt**: Veien fra rådata til analyseresultater
- Databehandling og -manipulering
- Enkle regresjonsanalyser m/eksport av resulater til en tabell
- Hvordan skrive pene og forståelige R-script
- Scriptet nedlasting av data vha. API
- Lage kart og diagrammer

Vi bruker det innebygde datasettet `mtcars` for å illustrere funksjonalitet.

Objekter
================================================================================

R er et *objektorientert* språk. De vanligste objekttypene er vector, matrix, data.frame og list. Vi lager objekter ved hjelp av en "venstre-pil".


```r
n1 <- 420
```

Objekter forblir lagret i minnet fram til R avsluttes, eller til man fjerner objektet manuelt ved hjelp av `rm()`. En av fordelene med R (sammnelignet med Stata) er at det er mulig å ha mange datasett i minnet samtidig.

Operatorer
================================================================================
`=` brukes inni funksjoner, ikke til å lage objekter:


```r
n2 <- paste("4", "2", "0", sep = "") # "limer" sammen 4, 2 og 0
```

Dobbelt likhetstegn brukes for å sammenligne noe (som i Stata)


```r
n1 == as.integer(n2) 
```

```
[1] TRUE
```

Ellers fungerer de vanlige matematiske operatorene som forventet

```r
(200 + 15 - 5) * 2
```

```
[1] 420
```

Objekttyper
================================================================================
Vi har altså `vector`, `matrix`, `data.frame` og `list`. `matrix` består av én eller flere `vector`. 


```r
v1 <- c(3, 4) # c()-funksjonen lager vektorer
matrix(c(v1, v1), nrow = 2, ncol = 2)
```

```
     [,1] [,2]
[1,]    3    3
[2,]    4    4
```

`data.frame` er en slags `matrix` med kolonnenavn (men er egentlig basert på `list`)


```r
class(mtcars)
```

```
[1] "data.frame"
```

```r
typeof(mtcars) # typeof gir den underliggende datastrukturen
```

```
[1] "list"
```

Hente ut elementer fra objekter
================================================================================
Det finnes hovedsakelig to funksjoner for å hente ut elementer av objekter, `$` og `[]`. Førstnevnte fungerer bare på `data.frame` og `named list`. Syntaksen er `mtcars$kolonnenavn` og `mtcars[, kolonne]`. I sistnevnte kan også rader spesifiseres.


```r
# Alle rader, kolonnen "cyl"
mtcars$cyl 
mtcars[, "cyl"] # merk anførselstegn her
# Rad 1 til 3, kolonne "cyl"
mtcars$cyl[1:3]
mtcars[1:3, "cyl"]
mtcars[1:3, 2] # Kan også bruke kolonnens plassing ("cyl" er kolonne nr. 2)
```

Klamme-syntaksen kan også brukes til å lage "delmengder":


```r
mtcars[mtcars$cyl > 4, ] # henter ut alle rader hvor "cyl" er større enn 4
mtcars[mtcars$cyl > 4, "cyl"] # henter ut alle verdier av "cyl" som er større enn 4
```

"list"-objekter
================================================================================
Et `list`-objekt er en liste med objekter, for eksempel en liste med `data.frame`. `list`-objekter er hierarkiske og kan ha mange nivåer.


```r
l1 <- list(3, 4) # `list` med ett nivå
l2 <- list(list(3, 4), list(5, 6)) # `list` med to nivåer
```

Syntaksen for å hente ut elementer fra en liste er litt spesiell. Dersom listen er navngitt, kan `$` benyttes. Hvis ikke, brukes doble klammer:


```r
testlist <- list("element1" = 3, "element2" = 4) # navngitt liste 
testlist[[1]]
```

```
[1] 3
```

```r
testlist$element1
```

```
[1] 3
```
Lister er nyttige dersom du trenger å utføre samme operasjon på flere objekter. For eksempel samme regresjon på flere datasett:


```r
mtlist <- list(mtcars, mtcars[mtcars$cyl > 4, ])
lapply(mtlist, function(x) {
    reg <- lm(mpg ~ cyl + hp + wt, data = x)
    summary(reg)
  })
```

Pakker
================================================================================

![Pakker](stickers.png)

De viktigste funksjonene er integrert i R, men en del funksjoner krever at du installerer og laster inn *pakker* ved hjelp av `library`-funksjonen.


```r
install.packages("dplyr") # Merk: anførselstegn
library(dplyr) # Merk: ikke anførselstegn
filter(mtcars, cyl >= 8) # Kun biler med svær motor
```

Det er også mulig å benytte spesifikke funksjoner uten å laste inn pakken først:


```r
dplyr::filter(mtcars, cyl >= 8)
```

Tidyverse
================================================================================
Vi kan også bruke innebygget R-funksjonalitet for å filtrere data:


```r
a <- mtcars[mtcars$cyl >= 8, ] 
b <- dplyr::filter(mtcars, cyl >= 8)
identical(a, b) # Sjekk om vi får samme resultat
```

```
[1] TRUE
```

`dplyr`-syntaksen er bygget rundt **verb**, og beskriver nærmest med ord hva som skjer med dataene (på ængelsk da): "apply a filter to `mtcars` that keeps cars with `>=` 8 cylinders".

`dplyr` er en del av `tidyverse`, en samling med pakker som er skapt for databehandling, og deler samme grammatikk, datastruktur og underliggende filosofi. Vi kommer stort sett til å forbli innenfor `tidyverse` i dette kurset. 

Vanligste funksjoner
================================================================================
La oss ta et steg tilbake og se på de mest brukte og nyttigste funksjonene i R. Mye av koden vi skriver vil dreie seg om databehandling.
Andre oppgaver, som f.eks. regresjoner krever mindre kode, og er relativt enkelt å utføre. Nedenfor ser vi på de vanligste og mest generelle innebygde funksjonene


```r
summary(mtcars) # Oppsummering av data (ekvivalent med -summarize- i Stata)
table(mtcars$cyl) # Tabulering av data (ekv. med -tabulate- i Stata)
```
