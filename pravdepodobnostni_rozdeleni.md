# Teorie

## Spojitá rozdělení v úlohách

### 1. Normállní (Gaussovské):

-   většinou v **přirozených lidských procesech** (výška, hmotnost, IQ, průměrný výdělek)
-   **dnorm(x, mean = , sd = ), pnorm, qnorm, rnorm**
-   **parametry**:
    -   **mean**: střední hodnota (průměr)
    -   **sd**: standardní odchylka (sqrt(rozptyl))
-   **úloha**: *Výška dospělých mužů v určitém regionu má normální rozdělení s průměrem 175 cm a standardní odchylkou 7 cm. Jaká je pravděpodobnost, že náhodně vybraný muž bude mít výšku menší nebo rovnou 180 cm?*

#### Graf hustoty pravděpodobnosti

```{r}
curve(dnorm(x,175,7),from=140,to=210, main="Hustota pravděpodobnosti normálního rozdělení",col="magenta",lwd = 4,ylab="f(x)")
```

#### Graf distribuční funkce

```{r}
curve(pnorm(x,175,7),from=140,to=210, main="Distribuční funkce normálního rozdělení",col="deeppink",ylab="F(x)", lwd = 4)
```

### 2. Lognormální:

-   proměnné, **jejichž logaritmus** má normální rozdělení, hodnoty jsou nezáporné, často finanční aktiva, růst populace, ...
-   **dnorm(log(x), mean =, sd = ), pnorm(log(x)), qnorm(log(x)), rnorm(log(x))**
-   **parametry**:
    -   **mean**: střední hodnota (průměr)
    -   **sd**: standardní odchylka (sqrt(rozptyl))
-   **úloha**: *Cena akcie má lognormální rozdělení s průměrem logaritmu 5 a standardní odchylkou logaritmu 0.3. Jaká je pravděpodobnost, že cena akcie bude menší nebo rovná 1000?*

#### Graf hustoty pravděpodobnosti

```{r}
curve(dnorm(log(x),5,0.3),from=50,to=500, main="Hustota pravděpodobnosti lognormálního rozdělení",col="magenta",ylab="Pravděpodobnost", lwd = 4)
```

#### Graf distribuční funkce

```{r}
curve(pnorm(log(x),5,0.3),from=50,to=500, main="Distribuční funkce lognormálního rozdělení",col="deeppink",ylab="Pravděpodobnost", lwd = 4)
```

### 3. Exponenciální:

-   čas **do náhodné události** (do poruchy, doba mezi telefonáty, doba čekání)
-   **dexp(x, rate = ), pexp, qexp, rexp**\
-   **parametry**:\
    -   **rate** (intenzita selhání) = *lambda = (-log(1 - P)/počet jednotek; lambda = 1/u*\
    -   **lower.tail** = TRUE/FALSE (TRUE ... \<;selhání , FALSE ... \>;přežití)\
-   **úloha**: *Průměrná doba čekání na autobus je 10 minut. Jaká je pravděpodobnost, že budeš muset čekat více než 15 minut?*

#### Graf hustoty pravděpodobnosti

```{r}
curve(dexp(x, rate = 1/10),from=0,to=20, main="Hustota pravděpodobnosti exponenciálního rozdělení",col="magenta",ylab="Pravděpodobnost", lwd = 4)
```

#### Graf distribuční funkce

```{r}
curve(pexp(x, rate = 1/10),from=0,to=20, main="Distribuční funkce exponenciálního rozdělení",col="deeppink",ylab="Pravděpodobnost", lwd = 4)
```

### 4. Uniformní (Rovnoměrné):

-   všechny hodnoty v daném intervalu mají **stejnou** pravděpodobnost výskytu
-   **dunif(x, min =, max =), punif, qunif, runif**
-   **parametry**:
    -   **min** ... dolní mez intervalu
    -   **max** ... horní mez intervalu
-   **úloha**: *Náhodně vybíráme číslo z intervalu 0 až 50. Jaká je pravděpodobnost, že vybrané číslo bude mezi 10 a 30?*

#### Graf hustoty pravděpodobnosti

```{r}
curve(dunif(x,min = 0, max = 50),from=-5,to=55, main="Hustota pravděpodobnosti uniformního rozdělení",col="magenta",ylab="Pravděpodobnost", lwd = 4)
```

#### Graf distribuční funkce

```{r}
curve(punif(x,min = 0, max = 50),from=-5,to=55, main="Distribuční funkce uniformního rozdělení",col="deeppink",ylab="Pravděpodobnost", lwd = 4)
```

# Diskrétní rozdělení

### 5. Bernoulliho:

-   náhodná proměnná, která může nabývat **dvou hodnot** (úspěch/neúspěch, true/false) v **jednom pokusu**
-   **dbinom(x, size = 1, prob =), pbinom, qbinom, pbinom**
-   **parametry**:
    -   **size**: = 1!; počet pokusů
    -   **prob**: pravděpodobnost úspěchu v jednom pokusu
-   **úloha**: *V testu je pravděpodobnost úspěchu 0.7. Jaká je pravděpodobnost, že v jednom pokusu bude úspěch?*

### 6. Binomické:

-   počet úspěchů v určitém počtu **nezávislých Bernoulliho pokusů**, každý pokus má **dvě možnosti**
-   **dbinom(x, size =, prob =), pbinom, qbinom, rbinom**
-   **parametry**:
    -   **size**: počet pokusů
    -   **prob**: pravděpodobnost úspěchu v jednom pokusu
-   **úloha**: *Ve firmě probíhá kampaň, která má 80% úspěšnost. Kolik je pravděpodobnost, že z 15 náhodně vybraných lidí se maximálně 10 zúčastní kampaně?*

### 7. Poissonovo:

-   počet **vzácných událostí** v pevném intervalu času nebo prostoru; počet telefonátů za den
-   **dpois(x, lambda =), ppois, qpois, rpois**
-   **parametry**:
    -   **lambda**: průměrný počet událostí za jednotku času (=1/průměr)
-   **úloha**: *V restauraci se v průměru objeví 4 zákazníci za hodinu. Jaká je pravděpodobnost, že za jednu hodinu přijde 6 nebo méně zákazníků?*

### 8. Geometrické:

-   počet nezávislých Bernoulliho pokusů, než nastane **první úspěch**; kolik pokusů je potřeba, než ...
-   **dgeom(x, prob =), pgeom, qgeom, rgeom**
-   **parametry**:
    -   **prob**: pravděpodobnost úspěchu v každém pokusu
-   **úloha**: *Při hodu mincí je pravděpodobnost úspěchu (padajícího „hlavy“) 0.4. Jaká je pravděpodobnost, že první „hlavu“ hodíš až při 5. pokusu?*

### 9. Hypergeometrické:

-   počet úspěchů ve vzorku vybraném z **konečné populace**, kde každý prvek může být buď **úspěchem** nebo **neúspěchem**
-   **dhyper(x, m = , n = , k = ), phyper, qhyper, rhyper**
-   **parametry**:
    -   **m**: počet úspěšných složek v populaci
    -   **n**: počet neúspěšných složek v populaci
    -   **k**: počet pokusů
-   **úloha**: *V krabici je 10 červených a 15 modrých kuliček. Pokud náhodně vybereme 5 kuliček, jaká je pravděpodobnost, že 3 z nich budou červené?*

### Písmenka před funkcemi - kdy jakou použít

-   **d = density** ... hustota pravděpodobnosti; když zjišťujeme, jak pravděpodobná je konkrétní hodnota x
-   **p = probability** ... distribuce; když zjišťujeme pravděpodobnost, že hodnota je menší nebo rovná x
-   **q = quantile** ... když zjišťujeme hranici (kvantil) pro danou pravděpodobnost; jaká hodnota x odpovídá tomu, že pravděpodobnost P(X\<=x) = 0.95
-   **r = random** ... když chceme náhodná čísla z daného rozdělení

# Normální (Gaussovské rozdělení)

## Stanovení kvartilů - Úloha 0

Nechť náhodná proměnná má normální rozdělení **N(2, 5)**. Stanovte kvartily této proměnné a kvartilové rozpětí. Interpretujte kvartilové rozpětí.

```{r}
dolni_kvartil <- qnorm(0.25, mean = 2, sd = sqrt(5))
horni_kvartil <- qnorm(0.75, mean = 2, sd = sqrt(5))
kvartilove_rozpeti <- horni_kvartil - dolni_kvartil
kvartilove_rozpeti

```

Kvartilové rozpětí **(IQR)** této náhodné proměnné je **3.536**. To znamená, že polovina hodnot náhodné proměnné s normálním rozdělením **N(2, 5)** leží mezi hodnotami **0.232** a **3.768**. Tento interval obsahuje **50%** všech hodnot této proměnné. V tomto případě je IQR relativně **široký**, což naznačuje **vyšší variabilitu hodnot** v tomto intervalu.

## Úloha 1

Firma se rozhodla podrobit své zaměstnance IQ testu. Průměrný výsledek byl **115**, rozptyl **256**. Hodnoty jsou popsány **Gaussovským rozdělením**. Určete pravděpodobnost, že hodnota IQ testu při náhodně vybraném zaměstnanci nabude hodnoty:\
a) 120 a menší\
b) větší než 105\
c) v rozpětí 100 a 130;\

```{r}
# průměr = střední hodnota = u
mean <- 115

# směrodatná odchylka = sd = odmocnina(rozptyl = o)
sd <- sqrt(256)
```

**a)**

```{r}
pnorm(120, mean = mean, sd = sd)
```

**b)** V těchto případech využijeme **1 - pnorm(P(x \<= 105))**

```{r}
1 - pnorm(105, mean=mean, sd=sd)
```

**c)** V těchto případech vyjádříme interval jako:\
**P(dolní mez \<= x \<= horní mez)** = P(x \<= **horní mez**) - P(x \<= **dolní mez**)

```{r}
pnorm(130, mean = mean, sd = sd) - pnorm(100, mean = mean, sd = sd)
```

## Úloha 2

Mějme náhodnou proměnnou **X \~ N(2,9)**. Určete pravděpodobnost, že tato proměnná nabyde hodnoty:\
a) větší než **0.5**\
b) menší než **-1.2**\
c) v hranicích od **-0.5 do 1**

```{r}
# Náhodná proměnná X ~ N(2,9);
# Rozdělení = normální (N)
# Střední hodnota = 2
# Rozptyl = 9

mean = 2
sd = sqrt(9)
```

**a)**

```{r}
1 - pnorm(0.5, mean = mean, sd = sd)
```

**b)**

```{r}
pnorm(-1.2, mean = mean, sd = sd)
```

**c)**

```{r}
pnorm(1, mean = mean, sd = sd) - pnorm(-0.5, mean = mean, sd = sd)
```

## Úloha 3

Náhodná proměnná X má \*\*Gaussovské rozdělení\*\* s **u = 23** a \*\*o**2 = 25**. Určěte:\
a) P(X \> 18)\
b) P(8 \< X \< 22)\
c) Stanovte, pro kterou hodnotu x platí P(X\<x) = 0.95\
d) Stanovte, pro kterou hodnotu x je splněné P(X\>x) = 0.75\
e) Určete hranice intervalu ekvidistantní od střední hodnoty, ve kterých se s pravděpodobností 0.95 bude nacházet hodnota náhodné proměnné X

```{r}
# u = střední hodnota = průměr = 23
mean <- 23

# o**2 = rozptyl = směrodatná odchylka ** 2
sd <- sqrt(25)
```

**a)**

```{r}
1 - pnorm(18, mean = mean, sd = sd)
```

**b)**

```{r}
pnorm(22, mean = mean, sd = sd) - pnorm(8, mean = mean, sd = sd)
```

**c)** Hledáme kvantil normálního rozdělení

```{r}
qnorm(0.95, mean = mean, sd = sd)
```

**d)** Převedeme 1 - 0.75 = 0.25 (Ekvivalent, na nějž je aplikovatelná funkce qnorm).

```{r}
qnorm(0.25, mean = mean, sd = sd)
```

**e)** Chceme interval symetrický kolem u = 23, ve kterém se nachází 95% všech hodnot X. Hledáme dolní hranici *a* a horní hranici *b*, mezi kterými je **95%** pravděpodobnosti. Z obou stran vezmeme 2.5 % \~ 0.025 (1 - 0.95 / 2; aby se zachovala symetrie).

```{r}
a <- qnorm((0 + 0.025), mean = mean, sd = sd)
b <- qnorm((1- 0.025), mean = mean, sd = sd)
print(c(a,b))
```

## Úloha 4

Náhodná proměnná U vznikla normováním náhodné proměnné **X \~ (20,25)**. Jen na základě informace, že P(U \< 1.645) = 0.95 najděte x, pro které platí **P(X \< x) = 0.95**.

```{r}
# stačí vědět, že rozdělení je normální a průměr a směrodatnou odchylku. U bychom potřebovali u matematického výpočtu normování, funkce qnorm to dělá za nás.
mean <- 20
sd <- sqrt(25)
qnorm(0.95, mean = mean, sd = sd)
```

## Úloha 5

Dluhopisový fond na rozvíjejících se trzích má průměrný roční výnos **10%** a směrodatnou odchylku **15%**, v obou případech per annum (ročně). Investiční společnost investuje do dluhopisového fondu **200 mil. euro**. Předpokládá se **Gaussovské rozdělení výnosů** dluhopisového fondu. Spočítejte pravděpodobnost, že:\
a) Se investorovi zhodnotí jeho investice o víc než **6 mil. euro**.\
b) Ztráta investora bude víc než **18.4 mil.** euro.

------------------------------------------------------------------------

**průměrný výnos** = 10% = 0.1\
**směrodatná odchylka výnosů** = 15% = 0.15\
**investice** = 200 000 000\
**průměrný zisk** = průměrný výnos \* investice = 0.1 \* 200 000 000 = 20 000 000\
**směrodatná odchylka zisku** = směrodatná odchylka výnosů \* investice = 0.15 \* 200 000 000 = 30 000 000

```{r}
mean <- 20000000
sd <- 30000000
```

**a)**

```{r}
1 - pnorm(6000000, mean = mean, sd = sd )
```

**b)**

```{r}
pnorm(-18400000, mean = mean, sd = sd)
```

## Úloha 6

Měření ampérmetrem je zatížené systematickou chybou **20 mA** a náhodné chyby meření mají **Gaussovo rozdělení** se směrodatnou odchylkou **5mA**. Vykonáme na něm **jedno** měření. S jakou pravděpodobností se bude lišit chyba naměřené hodnoty maximálně o **12 mA** od:\
a) střední hodnoty očekávané chyby měření\
b) skutečné měřené hodnoty\
c) Jaká je s pravděpodobností **0.95** maximální odchylka chyby měření od **její** (ne celkové!) střední hodnoty?\
-----------------------------------\
**celková chyba** = systematická chyba + náhodná chyba\
**systematická (konstantní) chyba** = 20 mA\
**střední hodnota náhodné chyby** = 0 mA (implicitně z Gaussova rozdělení - symetrické)\
**směrodatná odchylka náhodné chyby** = 5 mA\
**střední hodnota očekávané chyby** = 0 + 20 = 20 mA

```{r}
mean <- 20
sd <- 5
```

**a)** rozdíl o max. 12 mA od střední hodnoty (=20): interval \<20-12 ; 20+12\> = \<8; 32\>

```{r}
pnorm(32, mean = mean, sd = sd) - pnorm(8, mean = mean, sd = sd)
```

**b)**

```{r}
pnorm(12, mean = mean, sd = sd) - pnorm(-12, mean = mean, sd = sd)
```

**c)** Musíme zjistit kvantil pro 97.5% (2.5% z každé strany)

```{r}
mean <- 0
qnorm(0.975, mean = mean, sd = sd) 
```

## Úloha 7

Náhodná proměnná má **Gaussovské rozdělení** se střední hodnotou **u = 0**. Je známo, že pro ní platí **P{\|X\|\<=5}**. Stanovte:\
a) směrodatnou odchylku náhodné proměnné X\
b) pravděpodobnost **P{X\>-2}**\
c) pravděpodobnost **P{\|X\|\<=2.5}**

**a)**

```{r}
mean <- 0
sd <- 5 / qnorm(1 - (1 - 0.95) / 2)
sd

```

**b)**

```{r}
1 - pnorm(-2, mean = mean, sd = sd )
```

**c)**

```{r}
pnorm(2.5, mean=mean, sd = sd) - pnorm(-2.5, mean = mean, sd = sd)
```

## Úloha 8

Čas nepřímého buňkového dělení (mitóza) má **normální rozdělení** se střední hodnotou **jedna hodina** s variancí (česky rozptyl) **25**. Jaká je pravděpodobnost, že dělení nějaké buňky\
a) Se uskuteční dříve než za **45 minut**?\
b) Bude trvat déle než **65 minut**?\
c) Jaký čas je potřebný k dokončení mitózy přibližně **99%** ze všech buněk?

```{r}
mean <- 60 #minut
sd <- sqrt(25)
```

**a)**

```{r}
pnorm(45, mean = mean, sd = sd)
```

**b)**

```{r}
1 - pnorm(65, mean = mean, sd = sd)
```

**c)**

```{r}
qnorm(0.99, mean = mean, sd = sd)
```

## Úloha 9

Rychlost přenosu souboru z fakultního serveru na osobní počítač v studentském domově během pracovního dne večera má **normální rozdělení**se střední hodnotou **60 kB za sekundu** a směrodatnou odchylkou **4kB za sekundu**.Vypočítejte pravděpodobnost, že:\
a) Soubor bude přenesený rychlostí **70 kB** za sekundu nebo větší.\
b) Soubor bude přenesený rychlostí menší než **58 kB** za sekundu.\
c) Pokud má soubor velikost **1 MB**, jaký je průměrný čas na jeho přenos?\
d) Pokud má soubor velikost **1 MiB**, jaký je průměrný čas na jeho přenos?\

```{r}
mean <- 60
sd <- 4
```

**a)**

```{r}
1 - pnorm(70, mean = mean, sd = sd)
```

**b)**

```{r}
pnorm(58, mean = mean, sd = sd)
```

**c)**

```{r}
# 1 MB = 1 000 kB
velikost_souboru <- 1000
velikost_souboru/mean
```

**d)**

```{r}
# 1 MiB = 1 024 kB
1024/mean
```

## Úloha 10

Výška má v populaci **normální rozdělení N(175, 81)**.\

```{r}
mean <- 175
sd <- sqrt(81)
```

Jaká je pravděpodobnost, že výška náhodně vybraného jedince bude: a) menší než **150 cm**?\

```{r}
pnorm(150, mean = mean, sd = sd)
```

b)  v rozsahu **180-200 cm**?\

```{r}
pnorm(200, mean = mean, sd = sd) - pnorm(180, mean = mean, sd = sd)
```

c)  vyšší než **210 cm**?\

```{r}
1 - pnorm(210, mean = mean, sd = sd)
```

d)  menší než Vaše výška? \

```{r}
pnorm(175, mean = mean, sd = sd)
```

Jakou výšku musí mít osoba, abychom mohli říci, že **95 %** populace má výšku nižší než tato osoba?\

```{r}
qnorm(0.95, mean = mean, sd = sd)
```

Jakou výšku musí mít osoba, abychom mohli říci, že **75 %** populace je vyšší než tato osoba? 

```{r}
qnorm(0.25, mean = mean, sd = sd)
```

Určete hranice intervalu ekvidistantně vzdálené od střední hodnoty, ve kterých se s pravděpodobností 0,95 bude nacházet hodnota výšky v této populaci.\

```{r}
horni_kvartil <- qnorm(1-0.025, mean = mean, sd = sd)
dolni_kvartil <- qnorm(0+0.025, mean = mean, sd = sd)
print(c(dolni_kvartil, horni_kvartil))
```

# Lognormální rozdělení

## Úloha 11

Rychlost přenosu v kB má **lognormální rozdělení LN(3.75, 0.50)**. Vypočítej pravděpodobnost, že soubor bude přenesený:\
a) Rychlostí 70 kB za sekundu a vyšší\
b) Rychlostí menší než 58 kB za sekundu\

```{r}
mean <- 3.75
sd <- sqrt(0.5)
```

**a)**

```{r}
1 - pnorm(log(70), mean = mean, sd = sd)
```

**b)**

```{r}
pnorm(log(58), mean = mean, sd = sd)
```

## Úloha 12

Náhodná proměnná X má **lognormální rozdělení** s parametry **u = 5** a o**2 = 1. Určete pravděpodobnost, že:\
a) Náhodná proměnná X nabyde hodnoty** menší než 100**.\
b) Náhodná proměnná X nabyde hodnoty v intervale** od 50 do 100\*\*.\

```{r}
mean <- 5
sd <- sqrt(1)
```

**a)**

```{r}
pnorm(log(100), mean = mean, sd = sd)
```

**b)**

```{r}
pnorm(log(100), mean = mean, sd = sd) - pnorm(log(50), mean = mean, sd = sd)
```

## Úloha 13

Měsíční příjmy určité skupiny domácností (v Kč) jsou náhodnou proměnnou, která má **lognormální rozdělení** **LN(7,9)**. Odhadněte podíl domácností, které mají měsíční příjem **vyšší než 20 000 Kč**.

```{r}
mean <- 7
sd <- sqrt(9)

1 - pnorm(log(20000), mean = mean, sd = sd)
```

# Exponenciální rozdělení

## Úloha 14

Určitá součástka se pokazí v záruční době **1000 dní** s pravděpodobností **0.1**. Je známo, že doba životnosti takovéhle součástky se řídí **exponenciálním rozdělením** pravděpodobnosti.\
a) Stanovte střední dobu životnosti této součástky.\
b) Vypočítejte pravděpodobnost toho, že se součástka nepokazí po dobu jednoho nepřestupného roku.\
c) Vypočítejte pravděpodobnost toho, že se součástka nepokazí po dobu prvních **200 dní**, ale pokazí se v průběhu dalších **500 dní**.

Pro exponenciální rozdělení potřebujeme parametr **labmba** (intenzita selhání) = -log(1 - pravděpodobnost)/počet jednotek

```{r}
lambda = -log(0.9)/1000
```

**a)**

```{r}
# střední = průměrná doba životnosti
1/lambda
```

**b)**

```{r}
pexp(365, rate = lambda, lower.tail = FALSE)
```

**c)**

```{r}
pexp(200, rate = lambda, lower.tail = FALSE) * (pexp(700, rate = lambda, lower.tail = TRUE) - pexp(200, rate = lambda, lower.tail = TRUE))
```

## Úloha 15

Doba životnosti výrobku má **exponenciální rozdělení** se střední hodnotou **2000** dní.Vypočítejte pravděpodobnost toho, že:\
a) Výrobek bude funkční aspoň 3000 dní.\
b) Výrobek nebude funkční déle, než je jeho průměrná doba životnosti.\
c) Stanovte maximální záruční dobu, kterou chce poskytnout jeho výrobce, pokud připouští maximálně 5% reklamačních výrobků.

```{r}
mean <- 2000
lambda <- 1/mean
```

**a)**

```{r}
1 - pexp(3000, rate = lambda)
```

**b)**

```{r}
pexp(2000, rate = lambda)
```

**c)** Hledáme 95% kvantil životnosti

```{r}
qexp(0.95, rate = lambda)
```

## Úloha 16

Musíte si zavolat. Předpokládejme, že délka hovoru v minutách je **exponenciální** náhodná proměnná s parametrem **lambda = 1/10**. Někdo přijde do telefoní budky těsně před vámi. Najděte pravděpodobnost, že budete muset čekat:\
a) Méně než **5** minut.\
b) **5 - 10** minut.\
c) Více než **10** minut.\
d) Vypočítejte taktéž očekávanou hodnotu a rozptyl.

```{r}
lambda <- 1/10
```

**a**

```{r}
pexp(5, rate = lambda)
```

**b)**

```{r}
pexp(10, rate = lambda) - pexp(5, rate = lambda)
```

**c)**

```{r}
pexp(10, rate = lambda, lower.tail = FALSE) # Nebo 1 - pexp(10, lambda)
```

**d)**

```{r}
mean <- 1/lambda #očekávaná hodnota = střední hodnota = průměr
variance <- 1/(lambda**2) #rozptyl
mean
variance

```

## Úloha 17

Doba do poruchy má **exponenciální rozdělení** s intenzitou poruch **0.02**. Najděte\
a) Střední dobu do poruchy.\
b) Pravděpodobnost, že po dobu 80 hodin nedojde k poruše.

```{r}
lambda <- 0.02
```

**a)**

```{r}
1/lambda
```

**b)**

```{r}
1 - pexp(80, rate = lambda)
```

## Úloha 18

Doba bezporuchového chodu zařízení má **exponenciální rozdělení** se střední hodnotou **700** hodin. Určete dobu, během níž nedojde s pravděpodobností **0.8** k poruše.

```{r}
mean <- 700
lambda <- 1/mean
qexp(0.8, rate = lambda)
```

## Úloha 19

Předpokládáme, že životnost počítačového komponentu (v letech) je náhodná proměnná s **exponenciálním rozdělením** pravděpodobnosti se střední hodnotou rozptylem **4**.\
a) Stanovte pravděpodobnost, že životnost komponentu překročí **4** roky.\
b) Určete pravděpodobnost, že životnost komponentu překročí **4** roky a nepřekročí **8** let.

```{r}
mean <- 4
sd <- 4
lambda <- 1/mean
```

**a)**

```{r}
1 - pexp(4, rate = lambda ) 
```

**b)**

```{r}
pexp(8, rate = lambda) * (1 - pexp(4, rate = lambda))
```

## Úloha 20

Dlouhodobým pozorováním se zjistilo, že pravděpodobnost doby čekání na opravu jistého zařízení má **exponenciální rozdělení** s počátkem v **1** a se střední hodnotou **9**.\
b) Stanovte **intenzitu (= lambda)** doby čekání.\
a) Vypočítejte pravděpodobnost ukončení opravy v kratší době než **4** minuty.\

**a)**

```{r}
posun <- 1
mean <- 9
lambda <- 1/mean
```

**b)**

```{r}
pexp(4 - posun, rate = lambda)
```

# Uniformní rozdělení

## Úloha 21

Na posilnění identifikace zaměstnance se směrem firmy je každých **20** minut promítaná krátká prezentace o její misi a vizi. Určete pravděpodobnost, že pokud náhodně přijdete do promítací místnosti:\
a) Nebudete na prezentaci čekat déle než **5** minut.\
b) Budete čekat víc než **10** minut.\
c) Budete čekat **5-10** minut.

Jde o **Uniformní rozdělení** (čas mezi prezentacemi je pravidelný, náhodně vcházíme do místnosti)

```{r}
min <- 0
min <- 20
```

**a)**

```{r}
punif(5, min=0, max=20)
```

**b)**

```{r}
1 - punif(10, min=0, max=20)
```

**c)**

```{r}
punif(10, min=0, max=20) - punif(5, min=0, max=20)
```

# Bernoulliho rozdělení

## Úloha

V továrně na výrobu součástek má každý výrobek pravděpodobnost **0.98**, že projde kontrolou kvality bez závady. Jaká je pravděpodobnost, že v jednom pokusu (kontroly kvality) výrobek bude bez závady?

```{r}
size <- 1
prob <- 0.98
pbinom(1, size = 1, prob = 0.98)
```

# Binomické rozdělení

## Úloha

Univerzitní student Roman někdy zaspí a nestihne cvičení, které začíná v 9 hodin. Pravděpodobnost, že zaspí, je **0,4**. Předpokládejme, že semestr má **12** týdnů.\
a) Vypočítej **střední hodnotu** a **standardní odchylku**.\
b) Určete pravděpodobnost, že v semestru přijde na cvičení Roman pozdě v důsledku zaspání v **polovině** nebo více případů.\
c) Určete pravděpodobnost, že Roman splní povinnou docházku na cvičení, pokud jsou tolerovány **tři** absence.\
**a)**

```{r}
size <- 12
prob <- 0.4
# střední hodnota
mean <- size * prob
# standardní odchylka
sd <- sqrt(size * prob * (1 - prob))
```

**b)**

```{r}
1 - pbinom(5, size = 12, prob = 0.4)
```

**c)**

```{r}
pbinom(3, size = 12, prob = 0.4)
```

### Pravděpodobnostní funkce

```{r}
library(tidyverse)
pocet_pokusu <- 0:12
pravdepodobnost <- dbinom(pocet_pokusu, 12, 0.4)

df <- data.frame(x = pocet_pokusu, px = round(pravdepodobnost,5))

df %>% 
  ggplot(aes(x = x, y = px)) +
  geom_segment(aes(x = x, y = 0, xend = x, yend = px), color = "magenta") +
  scale_x_continuous(breaks=seq(0, 12, 1)) +
  labs(title = "Pravděpodobnostní funkce - binomické rozdělení", x = "Počet pokusů", y = "Pravděpodobnost") +
  theme(plot.title = element_text(face = "bold"))
```

### Distribuční funkce

```{r}
distribucni_funkce <- stepfun(pocet_pokusu,c(0,cumsum(pravdepodobnost)),right=TRUE)
plot(distribucni_funkce, verticals = FALSE, do.points = TRUE, xlab = "Počet pokusů", ylab = "Kumulativní pravděpodobnost", main = "Distribuční funkce - binomické rozdělení", font.main = 2, col = "deeppink")
axis(1, at = seq(0, max(pocet_pokusu), by = 1), labels = seq(0, max(pocet_pokusu), by = 1))
```

## Úloha

Student píše test, který obsahuje **15** otázek, ke každé otázce existují **čtyři** možné odpovědi, z nichž právě **jedna** je správná. Jaká je pravděpodobnost, že student odpoví správně na alespoň **pět** otázek (a test úspěšně splní), pokud problematiku vůbec neovládá a odpovědi volí náhodně?

```{r}
size <- 15 # počet pokusů
prob <- 0.25 # 1 ze 4 odpovědí je správná
1 - pbinom(4, size = 15, prob = 0.25)
```

# Poissonovo rozdělení

## Úloha

Průměr počtu těžkých zranění v **jednom** ročníku hokejové ligy je **5**. Zjistěte, jaká je pravděpodobnost, že počet těžkých zranění bude **více než 4**.

```{r}
1 - ppois(4, lambda = 5)
```

# Geometrické rozdělení

## Úloha

Představte si, že při každém hodu kostkou máte pravděpodobnost **1/6**, že padne šestka.\
a) Jaká je pravděpodobnost, že šestka padne poprvé až při **4.** hodu?\

```{r}
# 3 neúspěchy před úspěchem
dgeom(3, prob = 1/6)
```

b)  Jaká je pravděpodobnost, že šestka padne dříve než při **3.** hodu?

```{r}
# 0 nebo 1 neúspěch před úspěchem
pgeom(1, prob = 1/6)
```

# Hypergeometrické rozdělení

## Úloha

Mezi **stovkou** výrobků je **20** zmetků. Náhodně vybereme **deset** výrobků a sledujeme počet zmetků mezi vybranými. Jaká je pravděpobnost, že mezi vybranými výrobky budou **3** zmetky?

```{r}
pocet_hledanych <- 20 # počet zmetků
pocet_zbylych <- 80 # počet nezmetků (100 - 20)
pocet_vybranych <- 10 # "vybereme 10 výrobků"
dhyper(3, 20, 80, 10)
```
