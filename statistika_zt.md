## KMA/PAS

# Příprava na Zápočtový test

### ZS 2024/2025

```{r}
library(tidyverse)
library(ggplot2)
library(knitr)
library(dplyr)
library(readxl) 
library(readr) 
library(DescTools) # statistika
library(psych) # statistika
library(GGally) # funkce ggpairs()
rm(list=ls())
df <- read_excel("Data_vyzkum.xlsx")
```

# 1. Kategorická (kvalitativní) proměnná

-   **nominální** (neseřazená) = barva očí, typ ovoce, kraj...
-   **ordinální** (seřazená) - nelze změřit vzdálenost mezi kategoriemi = nálada, vzdělání ...

## 1. 1. Popisná statistika kategorické proměnné

-   určujeme:

### ABSOLUTNÍ ČETNOST

-   počet výskytů jednotlivých kategorií v datasetu

#### a) nominální proměnná

```{r, eval=FALSE}
absolutni_cetnost <- table(df$sloupec)
```

```{r}
absolutni_cetnost <- table(df$Bydliště)
absolutni_cetnost
```

#### b) ordinální proměnná

-   seřazení dat dle kategorií

```{r, eval=FALSE}
df$sloupec <- ordered(df$sloupec, levels = c("", "", "", ...))
```

```{r}
df$Vzdělání <- ordered(df$Vzdělání, levels = c("ZŠ", "SŠ", "VŠ"))
```

```{r, eval=FALSE}
absolutni_cetnost <- table(df$sloupec)
```

```{r}
absolutni_cetnost <- table(df$Vzdělání)
absolutni_cetnost
```

-   **kumulativní absolutní četnost**

```{r}
kumulativni_absolutni_cetnost <- cumsum(absolutni_cetnost)
kumulativni_absolutni_cetnost
```

### RELATIVNÍ ČETNOST

-   podíl jednotlivých kategorií vůči celkovému počtu dat v datasetu
-   absolutní četnost kategorie / počet hodnot ve sloupci
-   vyjádřeno jako procento/zlomek

#### a) nominální proměnná

```{r, eval=FALSE}
relativni_cetnost <- round(prop.table(table(df$sloupec)), 2) * 100 # V procentech.
relativni_cetnost
```

```{r}
relativni_cetnost <- round(prop.table(table(df$Bydliště)),2) * 100
relativni_cetnost
```

#### b) ordinální proměnná

```{r, eval=FALSE}
relativni_cetnost <- round(prop.table(table(df$sloupec)),3) * 100
relativni_cetnost
```

```{r}
relativni_cetnost <- round(prop.table(table(df$Vzdělání)),3) * 100
relativni_cetnost
```

-   **kumulativní relativní četnost**

```{r}
kumulativni_relativni_cetnost <- cumsum(relativni_cetnost)
kumulativni_relativni_cetnost
```

### Tabulka četností

```{r}
tabulka_cetnosti <- cbind("Absolutní četnost"=absolutni_cetnost, "Relativní četnost (%)" = relativni_cetnost)
tabulka_cetnosti
```

```{r}
tabulka_cetnosti <- cbind("Absolutní četnost"=absolutni_cetnost,
                          "Kumulativní absolutní četnost"=kumulativni_absolutni_cetnost,
                          "Relativní četnost (%)" = relativni_cetnost,
                          "Kumulativní relativní četnost (%)" = kumulativni_relativni_cetnost)
tabulka_cetnosti
```

## 1. 2. Grafické znázornění kategorické proměnné

### Sloupcový graf

```{r, eval=FALSE}
df <- data.frame(random_sloupec, kategorie)     
cetnost <- df %>% group_by(kategorie) %>% summarise(n=n())    # sloupec n s četnostmi kategorií
cetnost <- cetnost %>% arrange(desc(n))                       # seřazení sestupně
cetnost <- head(cetnost, 10)                                  # ponechá 10 kategoríí s největší četností
celkem <- nrow(df)                                            # počet hodnot ve sloupci 
cetnost <- cetnosti %>% mutate(relativni_cetnost=(n/celkem)*100)  # nový sloupec relativní četnost (%)  

cetnost %>% ggplot(aes(kategorie,relativni_cetnost, fill=kategorie)) +   # sloupcový graf               
  geom_col() +
  coord_flip()  +                                                        # převrácení na výšku
  labs(title = "", x = "", y = "")  +
  facet_wrap(~rozdeleni_dle_jine_kategorie) +
  theme(plot.title = element_text(face = "bold"))
```

```{r, eval=FALSE}
df %>% 
  ggplot(aes(x = kategorie)) +
  geom_bar(fill="deeppink") +
  labs(title = "", x = "", y = "") +
  theme(plot.title = element_text(face = "bold"))
```

```{r}
df %>% 
  ggplot(aes(x = Bydliště)) +
  geom_bar(fill="deeppink") +
  labs(title = "Bydliště (nominální) - Absolutní četnost", x = "Bydliště", y = "Četnost") +
  theme(plot.title = element_text(face = "bold"))
```

### Koláčový graf

```{r, eval=FALSE}
df %>% 
  ggplot(aes(x = "", y = hodnoty, fill = kategorie)) +
  geom_col() + 
  coord_polar("y") + 
  scale_fill_discrete(name = "kategorie") + 
  labs(title = "", y = "") +
  theme(plot.title = element_text(face = "bold"))
```

```{r, eval=FALSE}
df %>% 
  ggplot(aes(x = "", fill = Kategorie)) +
  geom_bar(width = 1, color = "white") +
  coord_polar("y")+
  scale_fill_discrete(name = "Kategorie") +
  labs(title="", y="") +
  theme_void() +
  theme(plot.title = element_text(face = "bold"))
```

```{r}
df %>% 
  ggplot(aes(x = "", fill = Vzdělání)) +
  geom_bar(width = 1, color = "white") +
  coord_polar("y")+
  labs(title="Vzdělání - Absolutní četnost")+
  theme_void() +
  scale_fill_manual(
    values = c("VŠ" = "deeppink", 
               "SŠ" = "darkorchid", 
               "ZŠ" = "violet")) +
  labs(title = "Vzdělání - Absolutní četnost") +
  theme_void() +
  theme(plot.title = element_text(face = "bold"))
```

# 2. Číselná (kvantitativní) proměnná

-   **dělení**:

    a)  **nespojitá (diskrétní)**

    -   konkrétní, oddělené hodnoty (celá čísla)
    -   počet dětí, počet žáků

    b)  **spojitá**

    -   hodnoty mohou nabývat libovolného čísla v určitém intervalu
    -   výška, hmotnost, teplota, čas

## 2. 1. Popisná statistika číselné proměnné

### Diskrétní proměnná

-   tabulka absolutních a relativních četností, viz. kategoriální proměnná

### Spojitá proměnná

-   popisná statistika **polohy** (průměr, medián, modus, minimum, maximum), **variability** (rozptyl, směrodatná odchylka, rozmezí, kvartily, šikmost, kurtóza)

### Frekvenční rozdělení

-   četnost v jednotlivých intervalech

```{r, eval=FALSE}
hranice <- c(-Inf, , , , Inf) # stanovení hraničních bodů intervalů
sloupec_intervaly <- cut(df$sloupec, breaks = hranice, labels = c("Pod ...", "", "", "Nad ..."))
sloupec_absolutni_cetnost <- table(sloupec_intervaly)
sloupec_absolutni_cetnost

sloupec_relativni_cetnost <- prop.table(table(sloupec_intervaly))
sloupec_relativni_cetnost
```

```{r}
hranice <- c(-Inf, 18.5, 25, 30, Inf)
bmi_intervaly <- cut(df$BMI, breaks = hranice, labels = c("Pod 18.5", "18.5-25", "25-30", "Nad 30"))

# ABSOLUTNÍ ČETNOST
bmi_absolutni_cetnost <- table(bmi_intervaly)

# RELATIVNÍ ČETNOST
bmi_relativni_cetnost <- round(prop.table(table(bmi_intervaly)),2) * 100

# TABULKA ČETNOSTÍ
tabulka_cetnosti <- cbind("Absolutní četnost"=bmi_absolutni_cetnost, "Relativní četnost (%)" = bmi_relativni_cetnost)
tabulka_cetnosti
```

### STATISTIKY POLOHY

```{r, eval=FALSE}
# PRŮMĚR -  součet hodnot / počet hodnot ; citlivý na odlehlé hodnoty
prumer <- mean(df$sloupec)

# MEDIÁN = 2. KVARTIL; prostřední hodnota seřazených dat ; robustní vůči odlehlým hodnotám
median <- median(df$sloupec)

# MODUS = hodnota s nejvíce výskyty, nemá modus, unimodální, bimodální, multimodální
modus <- function(x) {
  unikatni_hodnoty <- unique(x) 
  unikatni_hodnoty[which.max(tabulate(match(x, unikatni_hodnoty)))]
  }
modus(df$sloupec)

# MINIMUM a MAXIMUM
minimum <- min(df$sloupec)
maximum <- max(df$sloupec)

# 1. KVARTIL = DOLNÍ KVARTIL ; 25% dat je menší než tento počet 
dolni_kvartil <- quantile(df$sloupec, 0.25)

# 3. KVARTIL = HORNÍ KVARTIL; 75% dat je menší než tento počet 
horni_kvartil <- quantile(df$sloupec, 0.75)
```

```{r}
summary(df$Hmotnost)
```

```{r}
describe(df$Hmotnost)
```

### STATISTIKY VARIABILITY

```{r, eval=FALSE}
# ROZPTYL = průměrná kvadratická odchylka od průměru, jak moc jsou hodnoty rozptýlené kolem průměru
rozptyl <- var(df$sloupec)

# SMĚRODATNÁ ODCHYLKA = odmocnina z rozptylu; jak moc se průměrně liší hodnoty od průměru
smerodatna_odchylka <- sd(df$sloupec)

# VARIAČNÍ KOEFICIENT = relativní rozptyl dat vůči průměru, v %, možnost srovnávat různé datasety, jednotky
variacni_koeficient <- sd(df$sloupec) / mean(df$sloupec) * 100
```

## 2. 2. Grafické znázornění číselné proměnné

### Frekvenční polygon

-   diskrétní proměnné

```{r, eval=FALSE}
# potřebná tabulka četností
sloupec_cetnost <- as.data.frame(table(df$sloupec))

# přejmenování sloupců
names(sloupec_cetnost) <- c("sloupec", "Četnost")

# graf
sloupec_cetnost %>%
  ggplot(aes(x = sloupec, y = Četnost)) +
  geom_line(aes(group = 1), color="deeppink") +
  geom_segment(aes(x = sloupec,
                   xend = sloupec,
                   y = 0,
                   yend = Četnost),
               linetype = "dashed",
               color="deeppink") +
  labs(title = "Frekvenční polygon pro sloupec", x = " ", y = "Četnost") +
  theme(plot.title = element_text(face = "bold")) 
```

```{r}
deti_cetnost <- as.data.frame(table(df$Dětí))
names(deti_cetnost) <- c("Počet dětí", "Četnost")
deti_cetnost %>%
  ggplot(aes(x = `Počet dětí`, y = Četnost)) +
  geom_line(aes(group = 1), color="deeppink") +
  geom_segment(aes(x = `Počet dětí`,
                   xend = `Počet dětí`,
                   y = 0,
                   yend = Četnost),
               linetype = "dashed",
               color = "deeppink") +
  labs(title = "Počet dětí - Frekvenční polygon", x = "Počet dětí", y = "Četnost") +
  theme(plot.title = element_text(face = "bold")) 
```

### Histogram

-   spojité proměnné

```{r, eval=FALSE}
df %>%
  ggplot(aes(x = sloupec)) +
  geom_histogram(breaks = seq(od, do, delka_intervalu),                 # dílky histogramu     
                 color="black",
                 fill = "deeppink") +
  scale_x_continuous(breaks = seq(od, do, delka_intervalu)) +           # osa histogramu
  labs(title = "", x = "", y = "Četnost") +
  theme(plot.title = element_text(face = "bold")) 
```

```{r}
df %>%
  ggplot(aes(x = BMI)) +
  geom_histogram(breaks = seq(10, 45, 5),          
                 color="black",
                 fill = "deeppink") +
  scale_x_continuous(breaks = seq(10, 45, 5)) +
  labs(title = "BMI - histogram", x = "BMI", y = "Četnost") +
  theme(plot.title = element_text(face = "bold")) 
  #stat_bin(binwidth = 5, geom = "text", aes(label = after_stat(count)), vjust = -0.5)
```

```{r}
df %>%
  ggplot(aes(
    x = BMI,
    fill = cut(BMI,
               breaks = c(-Inf, 17.5, 25, 30, Inf),
               labels = c("Podváha", "Normální váha", "Nadváha", "Obezita")))) +
  geom_histogram(breaks = seq(12.5, 42.5, 2.5), 
                 color = "black") +
  scale_x_continuous(breaks = seq(12.5, 42.5, 2.5)) +
  scale_fill_manual(values = c("Podváha" = "darkviolet", 
                               "Normální váha" = "violet", 
                               "Nadváha" = "deeppink", 
                               "Obezita" = "magenta")) +
  labs(title = "BMI - histogram", x = "BMI", y = "Četnost", fill = "Kategorie BMI") + 
  theme(plot.title = element_text(face = "bold")) 
```

### Boxplot (Krabicový graf)

```{r, eval=FALSE}
df %>% 
  ggplot(aes(x="", y=sloupec)) +
  stat_boxplot(geom = "errorbar", width = 0.15, fill = "deeppink") +                  # udělá "tykadla"
  geom_boxplot(outlier.shape = 20, outlier.size = 3) +
  theme(legend.position="right") +
  labs(title= "", x="-", y="sloupec") +
  theme(plot.title = element_text(face = "bold")) +
  coord_flip()
```

```{r}
df %>% 
  ggplot(aes(x="", y=BMI)) +
  stat_boxplot(geom = "errorbar", width = 0.15) + 
  geom_boxplot(outlier.shape = 20, outlier.size = 3, fill = "deeppink") +
  theme(legend.position="right") +
  labs(title= "BMI - Boxplot", x="", y="BMI") +
  theme(plot.title = element_text(face = "bold")) +
  coord_flip()
```

```{r}
df %>% 
  ggplot(aes(x="", y=Hmotnost)) +
  stat_boxplot(geom = "errorbar", width = 0.15) + 
  geom_boxplot(outlier.shape = 20, outlier.size = 3, fill = "deeppink") +
  theme(legend.position="right") +
  labs(title= "Hmotnost - Boxplot", x="", y="Hmotnost") +
  theme(plot.title = element_text(face = "bold")) +
  coord_flip()
```

```{r}
df %>%
  ggplot(aes(x="", y=Hmotnost)) +
  stat_boxplot(geom = "errorbar", width = 0.15, coef=500) +
  geom_boxplot(coef=500, fill = "deeppink") + 
  theme(legend.position="right") +
  labs(title= "Hmotnost - boxplot", x="", y="Hmotnost") +
  theme(plot.title = element_text(face = "bold")) +
  coord_flip()

# parametr coef=500 rozšíří oblast, v níž se hodnoty neberou jako odlehlé (standard je coef=1.5)
```

### Jádrový odhad hustoty - graf

-   je metoda pro odhad hustoty pravděpodobnosti spojité náhodné proměnné

```{r, eval=FALSE}
df %>%
  ggplot(aes(x=sloupec)) +
  geom_histogram(aes(y=..density..), fill="magenta", alpha=0.25, binwidth = 10) +
  geom_density(color="deeppink") +
  labs(title="Jádrový odhad hustoty", x="sloupec", y="Hustota") +
  theme(plot.title = element_text(face = "bold")) 
```

**Graf s jedním jádrovým odhadem hustoty**

```{r}
df %>%
  ggplot(aes(x=Výška)) +
  geom_histogram(aes(y=..density..), fill="magenta", alpha=0.25, binwidth = 10) +
  geom_density(color="deeppink") +
  labs(title="Jádrový odhad hustoty", x="Výška", y="Hustota") +
  theme(plot.title = element_text(face = "bold")) 
```

**Graf s více jádrovými odhady hustoty**

```{r}
df %>%
  ggplot(aes(x = Výška)) +
  geom_histogram(aes(y = ..density..), fill = "magenta", alpha = 0.25, binwidth = 10) +
  
  geom_density(aes(color = "bw = Default")) + # hodnota bw se určuje pomocí Silvermanova pravidla
  geom_density(aes(color = "bw = 4"), bw = 4) +
  
  scale_color_manual(values = c("bw = Default" = "darkcyan", "bw = 4" = "deeppink"), name = "") +
  labs(title = "Jádrový odhad hustoty", x = "Výška", y = "Hustota") +
  theme(plot.title = element_text(face = "bold")) +
  facet_wrap(~Bydliště)
```

### QQ-Plot

```{r, eval=FALSE}
df %>% 
  ggplot(aes(sample = sloupec)) +
  geom_qq(color="deeppink") +
  stat_qq_line(color="deeppink") +
  labs(title = "QQ-plot pro sloupec", x = "Teoretické kvantily", y = "Kvantily dat") +
  theme(plot.title = element_text(face = "bold")) 
```

```{r}
df %>% 
  ggplot(aes(sample = Výška)) +
  geom_qq(color="deeppink") +
  stat_qq_line(color="deeppink") +
  labs(title = "QQ-plot pro Výšku", x = "Teoretické kvantily", y = "Kvantily dat") +
  theme(plot.title = element_text(face = "bold")) 
```

#### Odebrání nulových hodnot

```{r, eval=FALSE}
df_bez_nulovych_hodnot <- df %>% filter(sloupec != 0)
```

#### Diskrétní proměnná

```{r}
df %>% 
  ggplot(aes(sample = Dětí)) +
  geom_qq(color="magenta") +
  stat_qq_line(color="magenta") +
  labs(title = "QQ-plot pro Počet dětí", x = "Teoretické kvantily", y = "Kvantily dat") +
  theme(plot.title = element_text(face = "bold")) 
```

# 2. Normalita dat

-   jak jsou různé hodnoty rozděleny, jak často se hodnoty vyskytují v souboru dat
-   **Normální rozdělení (Gaussovo)**
    -   symetrické, data soustředěna kolem průměru
    -   výška lidí, IQ, krevní tlak
    -   grafy:
        1.  Histogram: symetrický tvar ve formě zvonu, s vrcholem uprostřed, pokles četnosti na krajích
        2.  Boxplot: medián uprostřed boxu, stejná délka whiskerů na obou stranách 3.QQ-Plot: bodová křivka na čáře, pokud se odchylují na koncích - extrémní hodnoty?

### Shapiro-Wilkův test normality

-   **Nulová hypotéza**: Data mají normální rozdělení.
-   **Alternativní hypotéza**: Data nemají normální rozdělení.
-   **Hladina významnosti**: 0.05 (případně 0.1 nebo 0.01)

```{r, eval=FALSE}
shapiro.test(df$sloupec)
```

```{r}
shapiro.test(df$Výška)
```

-   **W =** : Čím je hodnota blíže k 1, tím více jsou data podobná normálnímu rozdělení
-   **p-value =** : určuje, zda můžeme zamítnout nulovou hypotézu
    a.  p-value \> hladina významnosti - data jsou normálně rozdělena
    b.  p-value \< hladina významnosti - data nejsou normálně rozdělena

# 4. Typy rozdělení pravděpodobnosti

-   více v **PAS_ulohy_pravdepodobnost.html**
-   **dělení**: \## 4.1. Diskrétní rozdělení pravděpodobnosti
-   náhodné proměnné mohou nabývat konečný/ spočetně nekonečný počet hodnot
-   hodnoty jsou oddělené a nepřetržité (celá čísla)
-   počet dětí, hod kostkou
-   typy:
    -   **Uniformní rozdělení**

        -   všechna data mají stejnou pravděpodobnost
        -   hod kostkou

    -   **Hypergeometrické rozdělení**

        -   Máme skupinu 20 karet, z nichž 5 je červených a 15 modrých. Pokud náhodně vybereme 5 karet (bez náhrady), jaká je pravděpodobnost, že mezi vybranými kartami bude právě 2 červené? dhyper()

    -   **Binomické rozdělení**

        -   dva možné výsledky
        -   výhra/prohra dbinom() - Vypočítává pravděpodobnostní funkci (PMF) pro binomické rozdělení. pbinom() - Vypočítává kumulativní distribuční funkci (CDF) pro binomické rozdělení. qbinom() - Vypočítává kvantily pro binomické rozdělení. rbinom() - Generuje náhodná čísla z binomického rozdělení.

    -   **Poissonovo rozdělení**

        -   počet výskytů za určitý čas/plochu
        -   počet nehod za den
        -   Průměr počtu těžkých zranění v jednom ročníku hokejové ligy je 5. Zjistěte, jaká je pravděpodobnost, že počet těžkých zranění bude více než 4. ppois()

# 5.Intervaly spolehlivosti

Odhadněte výšku studentů Přírodovědecké fakulty UJEP za předpokladu, že výška má normální rozdělení.

```{r}
# BODOVÝ ODHAD STŘEDNÍ HODNOTY
vysky <- c(172, 183, 197, 180, 170, 170, 177, 170, 168, 178, 184, 169)
mean(vysky)
```

#### Interval spolehlivosti využívající Studentovo rozdělení (malý vzorek, neznáme rozptyl celé populace)

```{r}
# Intervalový odhad (spolehlivost 0.95)
MeanCI(vysky, method="classic", conf.level = 0.95, sides="two.sided")
# S 95% spolehlivostí je střední hodnota výšky žáků mezi __ cm a __ cm.
```

#### Interval spolehlivosti využívající BOOTSTRAP (opakované vzorkování dat)

```{r}
# Intervalový odhad (spolehlivost 0.95)
MeanCI(vysky, method="boot", n=1000, conf.level = 0.95, sides="two.sided")
# S 95% spolehlivostí je střední hodnota výšky žáků mezi __ cm a __ cm.
```

```{r}
#Bodový odhad směrodatné odchylky
sd(vysky)

# Interval spolehlivosti pro rozptyl
VarCI(vysky, method="classic", conf.level = 0.95, sides="two.sided")

# Interval spolehlivosti pro směrodatnou odchylku
sqrt(VarCI(vysky, method="classic", conf.level = 0.95, sides="two.sided"))
```

#### Interval spolehlivosti pro rozdíl (funkce MeanDiffCI)

```{r, eval=FALSE}
# interval spolehlivosti pro rozdíl
MeanDiffCI(neco_A,neco_B, method="classic", conf.level = 0.95, sides="two.sided")
```

#### prop.test

-   pro kvalitativní data, podíl určité kategorie v populaci (podíl leváků)
-   ukázka:

```{r}
# Předpokládejme, že mezi 200 dotazovanými (náhodně vybranými z dané populace) je 26 leváků.
# Sestrojte 95% oboustranný interval spolehlivosti pro podíl leváků v celé dané populaci.
# O jaké pravděpodobnostní rozdělení jde (u každého dotazovanéno rozlišujeme pouze to, zda je, či není levák)? binomické

#p = 26/200 = 13/100 = 0.13 ... podíl leváků v našem vzorku = bodový odhad posílu leváků v populaci

prop.test(x=26, n=200, alternative="two.sided", conf.level = 0.95)
```

#### t.test

-   pro kvantitativní data
-   testování hypotézy o střední hodnotě ... zda je rovna, porovnání mezi dvěma nezávislými/závislými skupinami
-   **příklad**: Lze výrobcem udávanou spotřebu 8.8 l/100 km považovat za pravdivou? Předpokládejte, že data mají normální rozdělení a volte α = 0.05.

```{r}
spotreba <- c(8.8, 8.9, 9.0, 8.7, 9.3, 9.0, 8.7, 8.8, 9.4, 8.6, 8.9)
MeanCI(spotreba, method="classic", conf.level = 0.95, sides="two.sided")
t.test(spotreba, mu = 8.8, conf.level = 0.95, alternative = "two.sided")
```

**p-value \< a** : zamítneme nulovou hypotézu - \>Střední hodnota spotřeby není 8.8 **p-value \> a**: přijmeme nulovou hypotézu.

# 6. Regresní analýza

= **Analýza vztahů**, zkoumá, jak jedna proměnná ovlivňuje druhou - regresní, korelační metody

```{r}
deti <- read_excel("DataDetiMin.xlsx")
```

## Bodový graf dvou proměnných

```{r, eval=FALSE}
df %>%
  ggplot(aes(x=, y=)) +
  geom_point(color="deeppink") +
  labs(title = "Bodový graf -  a ", x = "", y = "") +
  theme(plot.title = element_text(face = "bold")) 
```

```{r}
deti %>%
  ggplot(aes(x=VYSKA, y=HMOTN)) +
  geom_point(color="deeppink") +
  labs(title = "Bodový graf - výška a hmotnost", x = "Výška (cm)", y = "Hmotnost (kg)") +
  theme(plot.title = element_text(face = "bold")) 
```

## Typ

1.  **Lineární** : body vykazují rovnou čáru
2.  **Nelineární** : zakřivený tvar (parabola, ...)

## Korelace

1.  **Pozitivní** : rostoucí trend
2.  **Negativní** : klesající trend
3.  **Žádná**: žádný zjevný trend

### Pearsonův korelační koreficient

-   vyjadřuje sílu a směr lineárního stavu

-   r náleží **\<-1 ; 1\>** :

    -   **r = 1** : perfektní pozitivní lineární korelace
    -   **r = -1** : perfektní negativní lineární korelace
    -   **r = 0** : žádná lineární korelace
    -   **0 \< r \< 1** : pozitivní lineární korelace; roste jedna -\> roste druhá
    -   **-1 \< r \< 0**: negativní lineární korelace; roste jedna -\> klesá druhá

```{r, eval=FALSE}
cor(x=df$sloupec1,y=df$sloupec2, method="pearson", use="pairwise.complete.obs")
```

```{r}
# POZITIVNÍ LINEÁRNÍ KORELACE
cor(x=deti$VYSKA,y=deti$HMOTN, method="pearson", use="pairwise.complete.obs")
```

### Korelační matice

```{r, eval=FALSE}
vyber <- df[c("","","","")]
ggpairs(vyber, title="Korelogram pro data o ...")
```

```{r}
vyber <- deti[c("VYSKA", "HMOTN", "SKOKD", "LEHSE", "CLUNB", "CEVTR", "SUMA")]
ggpairs(vyber, title="Korelogram pro data o sportovním výkonu dětí")
```

## Kontigenční tabulka

```{r, eval=FALSE}
kontingencni_tabulka <- table(df$sloupec1, df$sloupec2)
addmargins(kontingencni_tabulka, margin = c(1,2))
```

```{r}
# SOUVISLOST HORMONÁLNÍ ANTIKONCEPCE A VÝSKYTU TROMBÓZY
kontingencni_tabulka <- table(df$`Hormonální antikoncepce`, df$Trombóza)
addmargins(kontingencni_tabulka, margin = c(1,2))
```

###Kontingenční tabulka s procentuálními podíly (součty v řádku)

```{r}
# procenta řádkových součtů
kontingencni_procenta_radky <- round(prop.table(kontingencni_tabulka, margin = 1),3) * 100
kontingencni_procenta_radky
```

###Kontingenční tabulka s procentuálními podíly (součty ve sloupci)

```{r}
# procenta sloupcových součtů
kontingencni_procenta_sloupce <- round(prop.table(kontingencni_tabulka, margin = 2),3) * 100
kontingencni_procenta_sloupce
```

### Grafické znázornění podílů kontigenční tabulky

```{r}
df %>%
  ggplot(aes(x = `Hormonální antikoncepce`, fill = Trombóza)) +
  geom_bar(position = "fill")+
  labs(title = "Výskyt trombózy dle hormonální antikoncepce",
       x = "Hormonální antikoncepce",
       y = "Podíl",
       fill = "Trombóza (True/False)") +
  theme(plot.title = element_text(face = "bold")) +
  scale_fill_manual(values = c("True" = "deeppink", "False" = "darkcyan"))
```

## Chí kvadrát test

-   závislost dvou kategoriálních veličin
-   **Nulová hypotéza**: Veličiny jsou nezávislé.
-   **Alternativní hypotéza**: Veličiny nejsou nezávislé.
-   **Hladina významnosti**: 0.05 (případně 0.1 nebo 0.01)

```{r}
chisq.test(kontingencni_tabulka)
```

-   **p-value =** : určuje, zda můžeme zamítnout nulovou hypotézu
    a.  p-value \> hladina významnosti - nulová hypotéza platí
    b.  p-value \< hladina významnosti - zamítáme nulovou hypotézu

### Regresní přímka

Popíšeme závislost proměnných pomocí **lineární funkce** (metodou nejmenších čtverců):

```{r}
deti %>% 
  ggplot(aes(x = VYSKA, y = HMOTN)) +
  geom_point() +
  labs(title = "Výška a hmotnost žáků ZŠ - bodový graf s regresní přímkou", x = "Výška (cm)", y = "Hmotnost (kg)") +
  theme(plot.title = element_text(face = "bold")) +
  geom_smooth(method = "lm", se = TRUE, color = "magenta") # Přidání regresní přímky (parametr lm značí lineární regresi, parametrem se=FALSE nezobrazíme intervaly spolehlivosti)
```

#### Rovnice regresní přímky

Zjistíme rovnici této lineární funce, tj. regresní přímky:

```{r}
# parametry lm se zadávají v pořadí "závisle proměnná" (u nás na ose y)" ~ "nezávisle proměnná" (u nás na ose x)
regrese <- lm(HMOTN ~ VYSKA , data = deti) 
regrese
```

**(Intercept)** : průsečík s osou y\
**SLOUPEC** : směrnice přímky\
**sloupec na ose y = SLOUPEC \* sloupec na ose x + (Intercept)**

```{r}
intercept <- coef(regrese)[1]     # Průsečík s osou y
smernice <- coef(regrese)[2]      # Směrnice
cat("Rovnice regresní přímky: HMOTN =", intercept, "+", smernice, "* VYSKA")
```

**Vyhodnocení regresního modelu:**

```{r}
summary(regrese)
```

**RESIDUALS** - reziduály; rozdíly skutečných a očekávaných hodnot

### PŘÍKLAD

Vytvořte regresní model, který popíše závislost proměnné *Proměnná* na zbývajících proměnných, a interpretujte ho.

```{r, eval=FALSE}
# Překódování proměnné Proměnná (1 = Yes, 0 = No)
df$Proměnná <- ifelse(df$Proměnná == "Yes", 1, 0)
```

```{r, eval=FALSE}
# Vytvoření regresního modelu a vypsání jeho parametrů
regresni_model <- lm(Proměnná ~ Zbývající_proměnná + Zbývající_proměnná + Zbývající_proměnná, data=df)
regresni_model
```
