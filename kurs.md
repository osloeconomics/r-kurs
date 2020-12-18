R: Databehandling, analyse og visualisering
========================================================
author: Eivind Moe Hammersmark
date: 18 desember, 2020
autosize: true
font-family: "TW Cen MT"

Hvorfor R?
========================================================
left: 40 %
I Oslo Economics har vi tre hovedanliggende når det gjelder kvantitative data: 
- **Databehandling**
- **Analyse**
- **Visualisering** av data og analyseresultater

For de to første punktene er det særlig viktig med dokumenterbarhet og etterprøvbarhet.
***
Velg Python eller R for allsidighet, dokumenterbarhet og etterprøvbarhet.

Velg Excel for modellering eller kjappe analyser.

| Verktøy | Databehandling | Analyse        | Diagrammer    | Kart      |
|---------|----------------|----------------|---------------|-----------|
| Excel   | omg just don't | omg just don't | Ja            | Begrenset |
| Stata   | Ja             | Ja             | Ja            | Begrenset |
| Python  | Ja             | Ja             | Ja            | Ja        |
| R       | Ja             | Ja             | Ja            | Ja        |
| QGIS^*    | Begrenset      | Nei            | Nei           | Ja        |  

Hva vil du lære på dette kurset?
========================================================

- **R basics**: Kodeprinsipper og vanligste funksjoner
- **Arbeidsflyt**: Veien fra rådata til analyseresultater
- Databehandling og -manipulering
- Enkle regresjonsanalyser m/eksport av resulater til en tabell
- Hvordan skrive pene og forståelige R-script
- Scriptet nedlasting av data vha. API
- Lage kart og diagrammer

Vi bruker det innebygde datasettet `mtcars` for å illustrere funksjonalitet

Pakker
========================================================
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
========================================================
Vi brukte `filter`-funksjonen fra `dplyr`-pakken til å lage et utvalg fra datasettet.
Vi kunne også brukt innebygget R-funksjonalitet, men `dplyr`-syntaksen er mer intuitiv:


```r
mtcars[mtcars$cyl >= 8, ]
# sammenlignet med
filter(mtcars, cyl >= 8)
```

`dplyr`-syntaksen er bygget rundt **verb**, og beskriver nærmest med ord hva som skjer med dataene: "filtrer data til biler med flere enn åtte sylindre".

`dplyr` er en del av `tidyverse`, en samling med pakker som er skapt for databehandling, og deler samme grammatikk, datastruktur og underliggende filosofi. Vi kommer stort sett til å forbli innenfor `tidyverse` i dette kurset. 

Objekter
=========================================================

R er et *objektorientert* språk. Et objekt kan være tall, tekststreng, datasett, vektor, etc.:


```r
n1 <- 420 # "numeric"-objekt
```

`=` brukes inni funksjoner, ikke til å lage objekter:


```r
n2 <- paste("4", "2", "0", sep = "") # "character"
```

Dobbelt likhetstegn brukes for å sammenligne noe (som i Stata)


```r
n1 == as.integer(n2) 
```

```
[1] TRUE
```
