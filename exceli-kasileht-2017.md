# Arvuti kasutamine keeleuurimisel. Andmete esmane töötlus Excelis
## Kristel Uiboaed
### *19/10/2017*
---

# Näiteandmestik
Oletame, et soovime uurida sõnade *tarvis* ja *vaja* varieerumist eesti murretes ning konstrkuktsioone, mis neid sõnavorme sisaldavad. Täpsemalt vt selle kohta [Lindström jt 2014](http://kjk.eki.ee/pdf/LindstromUiboaedVihman609_630.pdf). Andmestiku oleme kogunud [murdekorpusest](http://www.murre.ut.ee/mkweb/) ja see on failis `tarvis-vaja-n2iteandmestik-alg.xlsx`.

# Filtrid ja ebasoovitavate andmete eemaldamine
Alustuseks tutvume kõigi veergude sisuga (mis seal on, kas kõik on korrektne, mida peaks lisama/kustutama/parandama). Seda on lihtne teha filtritega.

![enter image description here](https://i.imgur.com/Fto5njY.png)

Vaatame näiteks veergu `Murre`. Näeme, et andmestikus on ka *läänevadja*. Kuna meid huvitavad ainult eesti murded, siis peaksime andmestiku puhastamiseks ja lihtsamaks analüüsimiseks selle rea kustutama. Selleks filtreerime vastava väärtuse välja, et veenduda, kas tegemist on tõesti sobimatu lause või veaga. Kustutame selle rea ära.

Kustutame ära vormiinfo ja tähenduse veeru, kuna see pole uuritava tunnuse seisukohalt hetkel oluline.

Igasuguste kustutamistega andmestikus tuleb olla ettevaatlik. Enamasti on mõistlik soovimatud read eraldi märgistada. Kui hilisema analüüsi käigus selgub, et need read on olulised, on kustutatut keeruline taastada. Eraldi märgitud read on aga lihtne lihtsalt välja filtreerida. Seega **igasuguste kustutamistega algandmestikust ole alati väga ettevaatlik!**

Lisaks saame andmeid eri viisidel sorteerida (näiteks küla nime järgi tähestikuliselt, või sageduste järgi kanahevalt, kui meil oleks seline veerg).

# Puuduvad andmed
Järgmiseks soovime täita kõik puuduva väärtusega lahtrid veergudes *Isik, Vanus, Sugu*. Selleks märgistame veerud, kus soovime lahtreid täita, valime `Home` menüüst `Find and Select`, `Go to Special`, `Blanks`, `OK`. Nüüd liigub kursor esimesse lahtrisse, mis on tühi ning märgistatud on ka kõik ülejäänud tühjad lahtrid. Sisestame esimesse lahtrisse soovitud väärtuse (näiteks *NA*) ja vajutame `Ctrl + Enter`. Nüüd saavad kõik märgistatud lahtrid sama väärtuse. Sageli tähistatakse puuduvad väärtused lühendiga *NA* (ingl *not available*), kuid loomulikult pole see kohustuslik.

# Uute andmete sisestamine olemasoleva info põhjal
Seejärel lisame ühe lahtri, mis tähistaks põhja- ja lõunaeesti murderühma. Lisame uue veeru nimega *Murderühm*. Kuna murded on meil andmestikus juba märgitud ning me teame, millised murded eri rühmadesse kuuluvad, siis selle info põhjal võime täita kõik lahtrid automaatselt. Selleks kasutame valemeid ning **if** ja **or** konstruktsioone:

**\=IF(OR(F2="Mulgi";F2="Setu";F2="Tartu";F2="Võru");"eL";"eP")**

Peale uue veeru loomist, on *Murre* F-veerus. Kasutame murde infot, et jagada murded rühmadesse, selleks ütleme valemi abil, et kui murre on Mulgi, Setu, Tartu või Võru: **OR(F2="Mulgi";F2="Setu";F2="Tartu";F2="Võru")**, siis on lahtri väärtus **eL** ehk lõunaeesti ja kui see tingimus pole tõene (st ükskõik mis muu murre), siis on lahtri väärtus **eP** (põhjaeesti).

**IF(**OR(F2="Mulgi";F2="Setu";F2="Tartu";F2="Võru");**"eL";"eP")**

# Vaatlustele unikaalsete tunnuste lisamine
Lisame andmestiku lausetele ka id-d ehk unikaalsed tunnused, mille järgi oleks hiljem võimalik neid tuvastada ja sorteerida. Töö käigus on tõenäoliselt vajadus andmeid mitut moodi töödelda ja sorteerida, seega oleks hea, kui meil oleks võimalik algne järjekord millegi põhjal taastada. Lisaks seame eesmärgiks, et *tarvis* ja *vaja* read oleksid eraldi kodeeritud, st et saaksime tuvastada, kas lause üks on *tarvis* või *vaja* portsu esimene jne. Kõigepealt nummerdame eraldi kummagi sõnaga read (`=ROW(A1)`). Seejärel lisame uue veeru (Id-sona) ning liidame numbri ja märksõna (`=CONCATENATE(A2;"_";D2)`). Sõnade liitmise vastastehe on nende tükeldamine. Seda on sageli vaja juhul, kui saame andmed tekstifailina *csv*-formaadis (*comma separated value*) ning soovime andmed Excelis veergudeks tükeldada. Näiteks võime praegu lauseid veerus *Kontekst* tükeldada tühiku kohalt (`Data, Text to Columns, Delimiter (valime _), OK`).

# Risttabelid
Kui andmed on korrastatud, saame hakata tegema põhjalikumaid ülevaateid, näiteks vaadata sagedusi. Selleks kasutame risttabeleid (`Insert, PivotTable`). Enne risttabeli lisamist tuleb ära märgistada kogu andmestik (või andmestiku osa), mille põhjal risttabeli infot näha soovime. Näiteks vaatame *tarvis* ja *vaja* jagunemist murrete ja murderühmade kaupa.

![enter image description here](https://i.imgur.com/8qiyjL5.png)

Lisaks on võimalik tabeli väljadel kuvatavat infot muuta, näiteks vaadata andmeid protsentidena. Selleks tuleb muuta välja seadmeid (`Value Field Settings`).

Kui algandmetes midagi muutub, ei pea tegema uut risttabelit, vaid piisab risttabeli andmete värskendamisest (`Data, Refresh`).

# Andmete kodeerimine
Sageli ei piisa ammendavaks lingvistiliseks analüüsiks infost, mida pakub korpus. Selleks tuleb meil uurimisküsimusest lähtuvalt ise andmeid kodeerida, st andmestikku uuritavaid tunnuseid lisada. Praegu võiksime näiteks tekitada ühe uue muutuja ja vaadata, milline on konstruktsioonis esinev peaverb (*olema*, *minema*, *tulema*, *0*). Kui me hakkame lauseid läbi lugema ja käsitsi kodeerima, siis niimoodi tekivad kergesti trükivead. Selliste vigade välistamiseks esitame väärtused, mis on lahtrites võimalikud ning valesti sisestamisel saame veateate.

Lisame uue muutuja jaoks uue veeru. Märgistame kõik selle veeru lahtrid, kuna soovime, et rippmenüü väärtused kehtiksid terves veerus. Üks paljudest võimalustest selleks: läheme veeru esimesse lahtrisse, kirjutame veeru tähistavasse lahtrisse faili viimase rea numbri ning vajutame `Shift + Enter`. 

![enter image description here](https://i.imgur.com/jOLsAWo.png)

Nüüd saame tekitada selle veeru kõigisse lahtritesse rippmenüü. Selleks valime `Data` menüüst alammenüü `Data Validation`, valime menüüks `List` ja sisestame (`Source` valikus) lubatud väärtused (*olema*, *minema*, *tulema*, *0*).
![enter image description here](https://i.imgur.com/3zkdOh0.png)

Niimoodi tekib kõigisse lahtritesse rippmenüü ainult lubatud väärtustega, kui proovime lahtrisse sisestada mingit muud väärtust, saame veateate. Seda kasutades väldime kodeerimisel kirjutamisvaeva ja juhuslike vigade tekkimist. Loomulikult on võimalik menüüd töö käigus muuta.


# Andmete kohandamine
Kodeerimisel võime peita selle ülesande jaoks ebaolulised veerud, sest liigne informatsioon raskendab lugemist ja keskendumist. Märgistame veerud, parem klõps ja `Hide` (taas saab teha need nähtavaks valikuga `Unhide`). 

Samuti on mugavam lugeda lauseid, kui need jääksid lahtri piiridesse, ilma et peaksime lahtreid väga pikaks "venitama". Selleks võime lahtri ridu n-ö murda (`Home, Wrap Text`).

Mugav on näha päist kogu andmestikus, ka siis kui jõuame tabeli lõppu. Selleks võime päise rea "külmutada" (`View, Freeze Panes, Freeze Top Row`).

![enter image description here](https://i.imgur.com/rz9n6C1.png)

# Soovituslik lugemine
Gries, Stefan Th (2013). [Statistics for linguistics with R: a practical introduction](https://www.amazon.com/Statistics-Linguistics-R-Mouton-Textbook/dp/3110307286). Berlin/Boston: Walter de Gruyter, lk 1-56.

# Täpsemalt näiteandmestiku kohta
Lindström, Liina, Kristel Uiboaed, Virve-Anneli Vihman (2014). [Varieerumine *tarvis/vaja*-konstruktsioonides keelekontaktide valguses](http://kjk.eki.ee/pdf/LindstromUiboaedVihman609_630.pdf). Keel ja Kirjandus 8-9, lk 609-630.




