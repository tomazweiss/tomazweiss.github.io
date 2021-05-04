Oglaševane prodajne cene stanovanj skozi čas
================

Višanje cen nepremičnin je zadnje čase aktualna tema, zato si poglejmo,
kako se to odraža v nepremičninskih oglasih.

## Podatki

Podatki so bili pridobljeni med marcem 2019 in majem 2021. Tako kot v
[prejšnjem
prispevku](https://tomazweiss.github.io/blog/oglasevane_cene_stanovanj/){:target="_blank"},
je bila tudi tukaj uporabljena avtomatizirana skripta, ki je enkrat na
dan s portala [nepremicnine.net](https://www.nepremicnine.net/){:target="_blank"} pobirala
nove oglase.

Poudarimo pomanjkljivosti, ki so v tako pridobljenih podatkih prisotne:

-   Gre za **oglaševane** in ne za realizirane cene.
-   Nekatera stanovanja se v oglasih pojavijo večkrat, npr. v primerih,
    ko jih oglašuje več agencij hkrati. Dogaja se tudi, da oglaševalci v
    želji po večji vidnosti po nekem času oglas izbrišejo in nato znova
    vnesejo.
-   V majhnem številu oglasov so prisotne očitne večje napake pri
    objavljeni ceni oz. površini. Zaradi tega pred analizo oglase z
    nenavadno visoko oz. nizko ceno na kvadratni meter izločimo.
-   Zajeti niso bili oglasi, ki so se na isti dan pojavili in tudi
    izbrisali in tako v času zajema več niso bili vidni.
-   Oglaševalci lahko morda kot površino stanovanja navedejo različne
    vrednosti (bruto, neto, uporabna površina, …).
-   Nekatere nepremičnine se prodajo mimo oglasnikov in tako seveda niso
    vključene.

Na grafih časovnih vrst, ki sledijo, bo prikazana 12-tedenska drseča
mediana cene kvadratnega metra stanovanja. Zakaj ravno 12 tedensko
obdobje? Če izberemo krajše obdobje, krivulje iz dneva v dan precej
nihajo in s tem nekoliko popačijo dejanske trende, v kolikor izberemo
daljše obdobje, so pa krivulje sicer bolj zglajene, vendar je lahko
dogajanje v kakšnem krajšem zanimivem obdobju preveč zakrito.

Z namenom, da stanovanja, ki po ceni preveč izstopajo, oz. napake v
podatkih ne bi preveč zakrivili slike, prikazujemo mediane in ne
povprečij objavljenih cen. Vrednost, ki je pri nekem datumu torej
prikazana, predstavlja mediano cen v vseh oglasih, ki so bili prvič
objavljeni v 12-tedenskem obdobju, ki se konča na ta dan.

Kot rečeno, pred analizo izločimo oglase z zelo visoko oz. zelo nizko
ceno na kvadratni meter in oglase, v katerih podatek o ceni ali površini
manjka. Konkretno to pomeni izbris 45 oglasov s ceno pod 400
EUR/m<sup>2</sup>, izbris 11 oglasov s ceno nad 10.000 EUR/m<sup>2</sup>
in izbris 18 oglasov z manjkajočima podatkoma.

Na koncu ostane v vzorcu 25.124 oglasov, od tega 18.169 za ljubljanska
stanovanja.

## Rezultati

Opozorilo: na grafih, ki sledijo, se skala na osi y namenoma ne začne
pri 0.

#### Ljubljana, okolica Ljubljane in Maribor

![]({{site.url}}/assets/images/lj_okolica_mb-1.png)<!-- -->


#### Ljubljana glede na tip stanovanja

![]({{site.url}}/assets/images/lj_tip-1.png)<!-- -->

#### Ljubljana glede na del mesta

![]({{site.url}}/assets/images/lj_del_mesta-1.png)<!-- -->

#### Ljubljana glede na tip stanovanja in del mesta

![]({{site.url}}/assets/images/lj_tip_del_v1-1.png)<!-- -->

![]({{site.url}}/assets/images/lj_tip_del_v2-1.png)<!-- -->

#### Ljubljana glede na letnico gradnje

Tukaj so oglasi razdeljeni v pet približno enako velikih skupin.

![]({{site.url}}/assets/images/lj_letnica-1.png)<!-- -->

#### Ljubljana glede na vrsto ponudbe (zasebna/agencija)

Oglasov iz zasebne ponudbe je 18%. Razlike med skupinama niso nujno
neposredno povezane z vrsto ponudbe oz. z vrsto oglaševalca; lahko gre
za posredne vplive drugih lastnosti oglaševanih stanovanj.

![]({{site.url}}/assets/images/lj_vrsta_ponudbe-1.png)<!-- -->

## Komentarja

-   Pri interpretaciji teh grafov je potrebno biti previden. Sprememba
    mediane oglašavanih cen pri neki kombinaciji atributov ne pomeni
    spremembe cen vseh takšnih stanovanj. Možno je namreč, da so se v
    tistem odbobju v večji meri oglaševala stanovanja, pri katerih nek
    tretji atribut govori v prid tej spremembi. Konkreten primer bi npr.
    bil dvig in padec cen v Mostah poleti 2020, ki je posledica večjega
    oglaševanja novogradenj v tem delu mesta.
-   Nekatere krivulje lahko v celoti oz. na določenem odseku temeljijo
    na manjšem vzorcu in zato dogajanja na trgu ne prikazujejo najbolje.
    To je še posebej izrazito na grafih, ki hkrati prikazujejo cene
    glede na tip stanovanja in del mesta.

## Koda

<https://github.com/tomazweiss/apartment_prices_over_time>{:target="_blank"}
