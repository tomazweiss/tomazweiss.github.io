---
title: "Oglaševane cene stanovanj"
excerpt_separator: "<!--more-->"
categories:
  - blog
tags:
  - R
  - ggplot2
  - real estate
  - nepremicnine
classes: wide
---


  - [Podatki](#podatki)
  - [Prodaja](#prodaja)
      - [Ljubljana in Maribor glede na velikost
        stanovanja](#ljubljana-in-maribor-glede-na-velikost-stanovanja)
      - [Ljubljana po upravnih enotah](#ljubljana-po-upravnih-enotah)
  - [Oddaja](#oddaja)
      - [Ljubljana in Maribor glede na velikost
        stanovanja](#ljubljana-in-maribor-glede-na-velikost-stanovanja-1)
      - [Ljubljana po upravnih enotah](#ljubljana-po-upravnih-enotah-1)
      - [Donos iz oddajanja](#donos-iz-oddajanja)

Cene nepremičnin so v zadnjem času v Sloveniji zelo aktualna tema.
Poglejmo si, kaj o njih pravijo podatki z našega največjega
nepremičninskega portala.

## Podatki

Podatki so bili pridobljeni med marcem in novembrom 2019. Pri tem je
bila uporabljena skipta, napisana v programskem jeziku
[R](https://www.r-project.org/){:target="_blank"} (paket
[`rvest`](http://rvest.tidyverse.org/){:target="_blank"}) in schedulirana v Amazonovem
oblaku [(AWS EC2)](https://aws.amazon.com/ec2/){:target="_blank"} na virtualnem Linux
strežniku, ki je vsak večer s portala pobirala nove oglase.

V tako pridobljenih podatkih so prisotne določene pomanjkljivosti:

  - Zajeti niso bili oglasi, ki so se na isti dan pojavili in tudi
    izbrisali in tako v večernih urah več niso bili vidni. To se
    največkrat dogaja pri oglasih za oddajo v Ljubljani.
  - Nekatere nepremičnine se v oglasih pojavijo večkrat. Če npr. za
    stanovanje ni interesa, lahko prodajalec po nekem času oglas vnese
    znova, z enako ali z znižano ceno. Isto stanovanje se lahko pojavi
    večkrat tudi v primerih, ko ga prodaja več agencij hkrati.
    Stanovanje, ki se oddaja, se lahko pojavi večkrat tudi v primeru, ko
    se je v zajetem obdobju izpraznilo in so lastniki iskali novega
    najemnika.

Predpostavljamo, da zgornje pomanjkljivosti bistveno ne kvarijo slike
trga. Opozoriti velja tudi, da gre tukaj za oglaševane in ne nujno za
realizirane cene. Dejanske realizirane vrednosti kupoprodajnih poslov so
dostopne v GURS-ovi [Evidenci trga
nepremičnin](http://prostor3.gov.si/ETN-JV/){:target="_blank"}. Veliko zanimivih
podatkov, ki izvirajo iz te evidence, najdemo v [Periodičnih poročilih o
slovenskem trgu
nepremičnin](http://www.trgnepremicnin.si/sl/vsebine-portala/periodicna-porocila){:target="_blank"}.

V analizi si bomo pogledali oglase za stanovanja v Ljubljani in v
Mariboru. Cene se v izbranem obdobju niso opazno spreminjale. Spodnja
tabela prikazuje število oglasov iz vzorca za vsako od zajetih
kombinacij. Skupaj je v vzorcu 12.105 oglasov.

| vrsta posla | lokacija  | št. oglasov |
| :---------- | :-------- | ----------: |
| prodaja     | Ljubljana |       6.705 |
| prodaja     | Maribor   |       1.190 |
| oddaja      | Ljubljana |       3.701 |
| oddaja      | Maribor   |         509 |

Vidimo lahko, da se je v Ljubljani pojavilo 5,6-krat več oglasov za
prodajo in 7,3-krat več oglasov za oddajo kot v Mariboru. Razlika v
številu prebivalcev teh dveh mest je približno
[3-kratna](https://en.wikipedia.org/wiki/List_of_cities_and_towns_in_Slovenia){:target="_blank"}.

## Prodaja

### Ljubljana in Maribor glede na velikost stanovanja

#### Absolutna cena

Porazdelitve cen bomo prikazali s t. i. grafom gostote verjetnosti (ang.
density plot). To je zglajena zvezna verzija
[histograma](https://en.wikipedia.org/wiki/Histogram){:target="_blank"}, ocenjena iz
podatkov. Na osi x so prikazane cene, višina krivulje pri posamezni ceni
pa je sorazmerna deležu oglasov s to ceno. Z navpičnimi črtkanimi črtami
označimo mediane posameznih
skupin.

<img src="{{site.url}}/assets/images/lj_vs_mb-1.png" style="display: block; margin: auto;" />


Mediane in povprečja posameznih kombinacij prikažimo še v
tabeli:

| velikost   | mediana Ljubljana | mediana Maribor | povprečje Ljubljana | povprečje Maribor | razmerje median |
| :--------- | ----------------: | --------------: | ------------------: | ----------------: | --------------: |
| garsonjera |            98.000 |          42.000 |             101.598 |            44.154 |            2,33 |
| 1-sobno    |           125.000 |          52.495 |             125.159 |            54.905 |            2,38 |
| 1.5-sobno  |           133.000 |          59.000 |             133.884 |            62.459 |            2,25 |
| 2-sobno    |           159.000 |          78.000 |             169.979 |            83.286 |            2,04 |
| 2.5-sobno  |           180.000 |          89.250 |             192.458 |            91.476 |            2,02 |
| 3-sobno    |           215.000 |         106.500 |             245.448 |           118.618 |            2,02 |

Manjša stanovanja so v Ljubljani približno 2,3-krat dražja kot v
Mariboru, večja stanovanja pa približno 2-krat.

Cene stanovanj lahko primerjamo s plačami. Po
[podatkih](https://pxweb.stat.si/SiStatDb/pxweb/sl/10_Dem_soc/10_Dem_soc__07_trg_dela__10_place__01_07010_place/0701041S.px/){:target="_blank"}
Statističnega urada je povprečna neto plača v obdobju med januarjem in
avgustom 2019 v občini Ljubljana znašala **1.265** evrov, v občini
Maribor pa **1.070** evrov. Plače v Ljubljani so torej v povprečju za **18
%** višje kot v Mariboru.

#### Cena na kvadratni meter

Cene lahko izrazimo tudi kot cene na kvadratni meter
stanovanja.

<img src="{{site.url}}/assets/images/lj_vs_mb_m2-1.png" style="display: block; margin: auto;" />

| velikost   | mediana Ljubljana | mediana Maribor | povprečje Ljubljana | povprečje Maribor | razmerje median |
| :--------- | ----------------: | --------------: | ------------------: | ----------------: | --------------: |
| garsonjera |             3.484 |           1.551 |               3.532 |             1.511 |            2,25 |
| 1-sobno    |             3.130 |           1.363 |               3.199 |             1.335 |            2,30 |
| 1.5-sobno  |             3.000 |           1.349 |               3.047 |             1.312 |            2,22 |
| 2-sobno    |             2.841 |           1.293 |               2.944 |             1.367 |            2,20 |
| 2.5-sobno  |             2.772 |           1.273 |               2.897 |             1.295 |            2,18 |
| 3-sobno    |             2.784 |           1.321 |               2.942 |             1.373 |            2,11 |

Vidimo, da cena na kvadratni meter z velikostjo stanovanja pada,
razmerja v cenah med krajema pa ostajajo podobna.

Zgornje cene za Ljubljano lahko primerjamo z
[zgodovinskimi](https://www.slonep.net/info/cene-nepremicnin/preglednica-cetrtletnih-cen-stanovanj-v-ljubljani){:target="_blank"}.

### Ljubljana po upravnih enotah

Poglejmo si sedaj cene v Ljubljani še glede na upravno
enoto.

#### Absolutna cena

<img src="{{site.url}}/assets/images/lj_po_ue-1.png" style="display: block; margin: auto;" />

Mediane cen po posameznih kombinacijah so
naslednje:

| velikost   | Lj. Moste-Polje | Lj. Šiška | Lj. Bežigrad | Lj. Vič-Rudnik | Lj. Center |
| :--------- | --------------: | --------: | -----------: | -------------: | ---------: |
| garsonjera |          85.000 |    95.000 |      104.000 |        109.500 |    116.000 |
| 1-sobno    |         115.000 |   117.404 |      129.000 |        128.000 |    139.000 |
| 1.5-sobno  |         125.000 |   128.779 |      133.000 |        133.500 |    145.000 |
| 2-sobno    |         149.000 |   155.000 |      155.000 |        168.000 |    199.000 |
| 2.5-sobno  |         165.000 |   170.000 |      179.000 |        198.000 |    237.000 |
| 3-sobno    |         187.000 |   199.000 |      211.500 |        225.200 |    295.000 |

Povprečja po posameznih kombinacijah so
naslednja:

| velikost   | Lj. Moste-Polje | Lj. Šiška | Lj. Bežigrad | Lj. Vič-Rudnik | Lj. Center |
| :--------- | --------------: | --------: | -----------: | -------------: | ---------: |
| garsonjera |          86.809 |    97.853 |      109.022 |        109.012 |    116.262 |
| 1-sobno    |         115.709 |   118.454 |      130.971 |        128.264 |    141.702 |
| 1.5-sobno  |         125.951 |   131.322 |      134.093 |        138.367 |    150.238 |
| 2-sobno    |         149.047 |   163.884 |      159.902 |        177.428 |    211.194 |
| 2.5-sobno  |         165.025 |   183.564 |      185.163 |        204.396 |    239.147 |
| 3-sobno    |         191.774 |   215.542 |      225.332 |        246.230 |    317.675 |

#### Cena na kvadratni meter

Zaradi večje preglednosti relativne cene predstavimo samo v tabelah.

Mediane cen po posameznih kombinacijah so
naslednje:

| velikost   | Lj. Moste-Polje | Lj. Šiška | Lj. Bežigrad | Lj. Vič-Rudnik | Lj. Center |
| :--------- | --------------: | --------: | -----------: | -------------: | ---------: |
| garsonjera |           3.242 |     3.262 |        3.560 |          3.576 |      4.285 |
| 1-sobno    |           2.979 |     3.196 |        3.105 |          3.177 |      3.523 |
| 1.5-sobno  |           2.805 |     3.088 |        2.879 |          3.083 |      3.492 |
| 2-sobno    |           2.628 |     2.789 |        2.758 |          2.950 |      3.370 |
| 2.5-sobno  |           2.558 |     2.703 |        2.683 |          2.932 |      3.421 |
| 3-sobno    |           2.543 |     2.568 |        2.692 |          2.919 |      3.268 |

Povprečja po posameznih kombinacijah so
naslednja:

| velikost   | Lj. Moste-Polje | Lj. Šiška | Lj. Bežigrad | Lj. Vič-Rudnik | Lj. Center |
| :--------- | --------------: | --------: | -----------: | -------------: | ---------: |
| garsonjera |           3.258 |     3.333 |        3.646 |          3.591 |      4.257 |
| 1-sobno    |           2.966 |     3.181 |        3.137 |          3.283 |      3.596 |
| 1.5-sobno  |           2.853 |     3.101 |        2.893 |          3.255 |      3.400 |
| 2-sobno    |           2.671 |     2.886 |        2.812 |          3.021 |      3.461 |
| 2.5-sobno  |           2.625 |     2.777 |        2.778 |          2.998 |      3.517 |
| 3-sobno    |           2.590 |     2.672 |        2.863 |          2.964 |      3.431 |

## Oddaja

Ponovimo sedaj celotno analizo še na oglasih za
oddajo.

### Ljubljana in Maribor glede na velikost stanovanja

#### Absolutna cena

<img src="{{site.url}}/assets/images/lj_vs_mb_odd-1.png" style="display: block; margin: auto;" />

Točne vrednosti median in povprečij prikazanih skupin vidimo v naslednji
tabeli:

| velikost   | mediana Ljubljana | mediana Maribor | povprečje Ljubljana | povprečje Maribor | razmerje median |
| :--------- | ----------------: | --------------: | ------------------: | ----------------: | --------------: |
| garsonjera |               450 |             265 |                 451 |               282 |            1,70 |
| 1-sobno    |               500 |             320 |                 512 |               332 |            1,56 |
| 1.5-sobno  |               585 |             335 |                 592 |               348 |            1,75 |
| 2-sobno    |               700 |             400 |                 739 |               395 |            1,75 |
| 2.5-sobno  |               750 |             500 |                 794 |               491 |            1,50 |
| 3-sobno    |               950 |             550 |               1.084 |               612 |            1,73 |

Razlike med krajema so pri oddaji manjše kot pri prodaji. Stanovanja so
v Ljubljani približno 1,7-krat dražja kot v
Mariboru.

#### Cena na kvadratni meter

<img src="{{site.url}}/assets/images/lj_vs_mb_m2_odd-1.png" style="display: block; margin: auto;" />

| velikost   | mediana Ljubljana | mediana Maribor | povprečje Ljubljana | povprečje Maribor | razmerje median |
| :--------- | ----------------: | --------------: | ------------------: | ----------------: | --------------: |
| garsonjera |             15,71 |           10,00 |               16,37 |             10,42 |            1,57 |
| 1-sobno    |             13,41 |            8,62 |               13,56 |              8,68 |            1,56 |
| 1.5-sobno  |             12,91 |            7,41 |               13,29 |              7,65 |            1,74 |
| 2-sobno    |             11,83 |            6,87 |               12,29 |              6,94 |            1,72 |
| 2.5-sobno  |             11,48 |            6,74 |               11,49 |              7,23 |            1,70 |
| 3-sobno    |             11,76 |            7,21 |               12,38 |              7,33 |            1,63 |

Za primerjavo so zgodovinske cene za oddajo v Ljubljani na voljo
[tukaj](https://www.slonep.net/info/cene-nepremicnin/preglednica-cetrtletnih-najemnin-stanovanj-v-ljubljani){:target="_blank"}.

### Ljubljana po upravnih enotah

#### Absolutna cena

<img src="{{site.url}}/assets/images/lj_po_ue_odd-1.png" style="display: block; margin: auto;" />

Mediane cen po posameznih kombinacijah so
naslednje:

| velikost   | Lj. Moste-Polje | Lj. Šiška | Lj. Bežigrad | Lj. Vič-Rudnik | Lj. Center |
| :--------- | --------------: | --------: | -----------: | -------------: | ---------: |
| garsonjera |             400 |       430 |          450 |            450 |        500 |
| 1-sobno    |             450 |       500 |          525 |            500 |        555 |
| 1.5-sobno  |             550 |       550 |          580 |            550 |        650 |
| 2-sobno    |             630 |       670 |          690 |            650 |        850 |
| 2.5-sobno  |             700 |       750 |          710 |            700 |        990 |
| 3-sobno    |             750 |       900 |          900 |            880 |      1.300 |

Povprečja po posameznih kombinacijah so
naslednja:

| velikost   | Lj. Moste-Polje | Lj. Šiška | Lj. Bežigrad | Lj. Vič-Rudnik | Lj. Center |
| :--------- | --------------: | --------: | -----------: | -------------: | ---------: |
| garsonjera |             402 |       450 |          438 |            458 |        507 |
| 1-sobno    |             453 |       489 |          518 |            512 |        572 |
| 1.5-sobno  |             563 |       579 |          580 |            552 |        692 |
| 2-sobno    |             634 |       681 |          710 |            707 |        876 |
| 2.5-sobno  |             689 |       827 |          727 |            705 |        991 |
| 3-sobno    |             816 |     1.007 |        1.010 |          1.013 |      1.323 |

#### Cena na kvadratni meter

Mediane cen po posameznih kombinacijah so
naslednje:

| velikost   | Lj. Moste-Polje | Lj. Šiška | Lj. Bežigrad | Lj. Vič-Rudnik | Lj. Center |
| :--------- | --------------: | --------: | -----------: | -------------: | ---------: |
| garsonjera |           14,58 |     15,38 |        15,38 |          15,62 |      17,19 |
| 1-sobno    |           12,86 |     13,33 |        12,92 |          13,61 |      14,90 |
| 1.5-sobno  |           12,99 |     12,22 |        12,89 |          12,99 |      14,99 |
| 2-sobno    |           10,87 |     11,34 |        11,40 |          11,67 |      13,68 |
| 2.5-sobno  |           10,79 |     11,12 |        11,54 |           9,92 |      13,55 |
| 3-sobno    |            9,86 |     11,44 |        11,75 |          10,67 |      13,51 |

Povprečja po posameznih kombinacijah so
naslednja:

| velikost   | Lj. Moste-Polje | Lj. Šiška | Lj. Bežigrad | Lj. Vič-Rudnik | Lj. Center |
| :--------- | --------------: | --------: | -----------: | -------------: | ---------: |
| garsonjera |           15,22 |     15,81 |        15,74 |          16,20 |      18,71 |
| 1-sobno    |           12,45 |     13,42 |        13,10 |          13,70 |      14,97 |
| 1.5-sobno  |           13,06 |     12,17 |        13,24 |          12,94 |      15,86 |
| 2-sobno    |           11,07 |     11,65 |        11,70 |          12,06 |      13,99 |
| 2.5-sobno  |           10,88 |     11,73 |        11,46 |          10,06 |      13,23 |
| 3-sobno    |           10,13 |     11,75 |        12,16 |          11,51 |      14,24 |

### Donos iz oddajanja

Izračunajmo še, kakšen bi bil donos iz oddajanja stanovanja, če bi ga
kupili in oddajali po medianah trenutnih
cen.

| lokacija  | velikost   | mediana oddaja \[EUR\] | mediana prodaja \[EUR\] | letni donos brez stroškov \[%\] | letni donos s stroški \[%\] |
| :-------- | :--------- | ---------------------: | ----------------------: | ------------------------------: | --------------------------: |
| Ljubljana | garsonjera |                    450 |                  98.000 |                            5,51 |                        3,58 |
| Ljubljana | 1-sobno    |                    500 |                 125.000 |                            4,80 |                        3,12 |
| Ljubljana | 1.5-sobno  |                    585 |                 133.000 |                            5,28 |                        3,43 |
| Ljubljana | 2-sobno    |                    700 |                 159.000 |                            5,28 |                        3,43 |
| Ljubljana | 2.5-sobno  |                    750 |                 180.000 |                            5,00 |                        3,25 |
| Ljubljana | 3-sobno    |                    950 |                 215.000 |                            5,30 |                        3,45 |
| Maribor   | garsonjera |                    265 |                  42.000 |                            7,57 |                        4,92 |
| Maribor   | 1-sobno    |                    320 |                  52.495 |                            7,31 |                        4,75 |
| Maribor   | 1.5-sobno  |                    335 |                  59.000 |                            6,81 |                        4,43 |
| Maribor   | 2-sobno    |                    400 |                  78.000 |                            6,15 |                        4,00 |
| Maribor   | 2.5-sobno  |                    500 |                  89.250 |                            6,72 |                        4,37 |
| Maribor   | 3-sobno    |                    550 |                 106.500 |                            6,20 |                        4,03 |

Pri izračunu letnega donosa v predzadnjem stolpcu nismo upoštevali davka
(25 %), stroškov zavarovanja, rezervnega sklada ter raznih popravil.
Letni donos v zadnjem stolpcu predpostavlja, da se za te stroške nameni
35 % najemnine.

![]({{site.url}}/assets/images/donosi-1.png)<!-- -->

Podatki potrjujejo [znano dejstvo](https://pro.finance.si/8862162){:target="_blank"}, da
je oddajanje donosnejše v Mariboru.
