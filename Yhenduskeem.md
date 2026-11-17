NB! Hetkel vist pole seal kastis kolme eraldi klemmliistu 5V, 3.3V ja GND jaoks. 5V oma pole esialgu oluline hetkel pole vaja veel toidet ühendada. 

Ma ise ei saa tulla, aga allpool on juhis, kuidas ühendada. Kui  midagi on arusaamatu, siis sate mulle kirjutada ipeets@gmail.com või 5200496



# Suitsuahju kontrolleri ühendamine

See juhend on meie juba juhtmetega ühendatud ESP32 ja **30-klemmilise liistu** jaoks. Olemasolevaid ESP32 ja liistu vahelisi juhtmeid ümber ei tõsta.

**Juhendis olev „klemm nr ...” tähendab alati 30-klemmilise liistu numbrit.** GPIO on ESP32 viigu nimi, mille lisame ainult kontrollimiseks.

## Mida ühendame?

- Neli metallsondiga temperatuuriandurit: **T1 kamber ülemine**, **T2 kamber alumine**, **T3 liha 1**, **T4 liha 2**.
- Üks **AM2302 / DHT22** välisõhu temperatuuri ja niiskuse jaoks.
- Kaks servot: **S1 kambri siiber**, **S2 möödaviigusiiber**.
- Olemasolev punane **2,4-tolline SPI-puuteekraan**.
- Ventilaator koos juhtimiseks vajaliku draiveriga.
- Summer koos selle tüübile sobiva juhtimisega.

Eraldi nuppe ja SD-kaarti praegu ei kasuta. Seadistamine hakkab käima telefonist, ekraanil näitame näite ja olekuid.

## 1. Tee kolm toitejaotust

**Siin ehk jaotusliist** tähendab ühenduskohti, mis on omavahel elektriliselt ühendatud. Ühe siini kõik klemmid on sama elektrilise ühenduse all.

Paigalda kolm eraldi ja selgelt märgistatud kruviliistu:

| Silt | Mis seal on? | Milleks kasutame? |
|---|---|---|
| **GND / 0 V** | Ühine toite miinus | Kõigi seadmete miinusühendused |
| **+5 V** | Toiteploki pluss | ESP32 toide ja sobivad 5 V tarbijad |
| **+3,3 V** | ESP32 plaadilt tulev 3,3 V | Andurite toide ja andmeliinide tõmbetakistid |

**+5 V ja +3,3 V peavad olema eraldi.** +5 V ei ühendata kunagi klemmile 1.

Leia klemmliistu nr 1 ning loe numbreid kokkulepitud ühenduspoolelt 1–30. Ära otsusta lugemissuunda ainult karbi hetkeasendi või juhtmevärvide järgi. Kontrolli pingestamata seadmel multimeetri pidevustestiga vähemalt **1 → 3V3**, **16 → VIN** ja **17 → GND**.

## 2. Ühenda reguleeritav toiteplokk ESP32-ga

Meil on reguleeritav toiteplokk, mille juhtmete otstes on krokodillklemmid. Sellest tuleb seadmele toide.

**Ühendamise ajal peab toiteploki väljund olema välja lülitatud. ESP32 USB-kaabel peab olema eemaldatud.**

1. Seadista toiteploki väljundpingeks **5,0 V**. Kui seadistamiseks peab väljundi sisse lülitama, tee seda enne seadmega ühendamist ning lülita väljund seejärel uuesti välja.
2. Ühenda **punane plusskrokodill +5 V kruviliistule**.
3. Ühenda **must miinuskrokodill GND-kruviliistule**.
4. Vii juhe **+5 V kruviliistult 30-klemmilise liistu klemmile nr 16**. Sealt läheb olemasolev juhe ESP32 **VIN**-sisendisse.
5. Vii juhe **GND-kruviliistult klemmile nr 17**. Sealt läheb olemasolev juhe ESP32 **GND**-le.
6. Vii juhe **klemmilt nr 1 eraldi +3,3 V kruviliistule**. Klemm nr 1 on juba ühendatud ESP32 **3V3**-väljundiga.

**Klemm nr 2 on samuti GND**, kuid selles juhendis kasutame toite miinuse jaoks nr 17, et kõigil oleks sama tööjärjekord.

### Kontrolli esialgu ainult toiteosa

Andureid, servosid ja ekraani veel toitega ei ühenda.

1. Vaata ühendused üle ja lülita toiteploki väljund sisse.
2. Pane multimeeter alalispinge mõõtmisele. Must mõõteots jääb GND-liistule.
3. Punase mõõteotsaga +5 V liistult mõõtes peab näit olema umbes **5 V**.
4. Punase mõõteotsaga +3,3 V liistult mõõtes peab näit olema umbes **3,3 V**.
5. Lülita toiteploki väljund uuesti välja.

Kui 3,3 V liistul on 5 V, on ühendus vale. Seda parandatakse enne järgmisi ühendusi. Iga järgmise etapi eel on toide välja lülitatud. Välise VIN-toite kasutamise ajal USB-d ei ühenda; programmeerimiseks võtame esmalt välise toite lahti, kuni meie plaadi toiteeraldus on kontrollitud.

## 3. Valmista temperatuuriandurite DATA-jaotus

Lisaks kolmele toitesiinile tee **väike eraldi ühenduskoht**, kuhu lähevad sondide kollased andmejuhtmed. Märgista see **TEMP DATA**.

1. Vii juhe **TEMP DATA ühenduskohast klemmile nr 11**. See on ESP32 **GPIO21**.
2. Jäta samasse elektrilisse ühendusse kohad neljale kollasele andurijuhtmele.
3. See ei ole uus toitesiin: **TEMP DATA ühendust ei sillata otse GND, +5 V ega +3,3 V liistuga**.

> Järgmine peatükk kehtib **DS18B20 sondide** korral ja eeldab värve **punane = VCC**, **must = GND**, **kollane = DATA**. Foto näitab neid värve, kuid ei tõenda anduri mudelit ega sisemist ühendust. Enne sondide toitega katsetamist tuleb pakendi või müüja andmetest see üks kord kinnitada. Kui pole kinnitatud, märgista sondid ja valmista juhtmed ette.

## 4. Ühenda neli sondi ükshaaval

Märgista iga sond nii mõõteotsa kui ka juhtmeotsa juurest. Ühise DATA-liini puhul ei määra juhtmete ühendamise järjekord, millisest sondist saab T1.

### Sond T1 — suitsukamber ülemine

1. **Punane juhe → +3,3 V kruviliist.**
2. **Must juhe → GND-kruviliist.**
3. **Kollane juhe → TEMP DATA ühenduskoht**, mis läheb **klemmile nr 11**.

### Sond T2 — suitsukamber alumine

1. **Punane juhe → +3,3 V kruviliist.**
2. **Must juhe → GND-kruviliist.**
3. **Kollane juhe → TEMP DATA ühenduskoht → klemm nr 11.**

### Sond T3 — lihasond 1

1. **Punane juhe → +3,3 V kruviliist.**
2. **Must juhe → GND-kruviliist.**
3. **Kollane juhe → TEMP DATA ühenduskoht → klemm nr 11.**

### Sond T4 — lihasond 2

1. **Punane juhe → +3,3 V kruviliist.**
2. **Must juhe → GND-kruviliist.**
3. **Kollane juhe → TEMP DATA ühenduskoht → klemm nr 11.**

### Lisa üks 4,7 kΩ takisti

Takisti tähis võib olla **4,7 kΩ**, **4k7** või **4700 Ω**. Need tähendavad sama väärtust.

- Takisti **üks ots → TEMP DATA ühenduskoht**, kus on sondide kollased juhtmed ja ühendus klemmiga nr 11.
- Takisti **teine ots → +3,3 V kruviliist**.
- Takisti kummalgi otsal pole kindlat suunda: mõlemat pidi sobib.
- Takistit ei panda kollase juhtme sisse järjestikku. Kollane juhe peab jääma otse andmeühendusse.
- **Kogu nelja sondi ühisele liinile tuleb üks selline takisti.**

Programmis seome sondide unikaalsed aadressid T1–T4 nimetustega. Seda katsetame hiljem ühe sondi soojendamise kaupa.

## 5. Ühenda väline AM2302 temperatuuri- ja niiskusandur

See andur jääb suitsukambrist väljapoole. Tema DATA ei lähe sondide TEMP DATA ühendusse.

1. Anduri **VCC / + → +3,3 V kruviliist**.
2. Anduri **GND / − → GND-kruviliist**.
3. Anduri **DATA / OUT / S → klemm nr 14**, mis on ESP32 **GPIO22**.
4. Kui on kontakt **NC**, jääb see ühendamata.

AM2302 konkreetse eksemplari juhtmevärve pole fotoga kinnitatud. Lähtu tähistest; kui neid ei ole, jäta juhtmed ootele.

**Kui kasutatakse nelja jalaga paljast DHT22 andurit:** võre enda poole ja jalad all, vasakult paremale on VCC, DATA, NC ja GND. Seda järjestust ei kanta automaatselt üle kolme kontaktiga moodulile või kaabliga andurile.

### AM2302 eraldi 10 kΩ takisti

Kui anduril või moodulil on tõmbetakisti juba olemas, uut pole vaja. Kui puudub:

- **10 kΩ takisti üks ots → AM2302 DATA-juhtme ühendus / klemm nr 14**.
- **Teine ots → +3,3 V kruviliist**.

10 kΩ = 10k = 10 000 Ω. See takisti on AM2302 jaoks; sondide 4,7 kΩ takisti seda ei asenda.

## 6. Ühenda kaks servot

Servodel on pluss, miinus ja signaal. Tavaliselt on pluss punane, miinus must või pruun ning signaal kollane, oranž või valge. **Kontrolli seda oma servo mudeli järgi.**

Järgmine toiteühendus kehtib servodele, mille lubatud toitepinge sisaldab **5 V**. Kui nende pinge pole teada, valmista juhtmed ette, kuid ära ühenda servo toiteplussi.

### Servo S1 — kambri siiber

1. **Servo pluss → +5 V kruviliist.**
2. **Servo miinus → GND-kruviliist.**
3. **Servo signaal → klemm nr 6 / GPIO16.**

### Servo S2 — möödaviigusiiber

1. **Servo pluss → +5 V kruviliist.**
2. **Servo miinus → GND-kruviliist.**
3. **Servo signaal → klemm nr 7 / GPIO17.**

Servode vool tuleb otse toiteplokist jaotusliistude kaudu. Seda ei juhita läbi ESP32 ega klemmi nr 16. Sama toiteplokk võib toita ESP32-d ja servosid, kui selle vooluvõimsusest piisab ka servode liikumise ja koormuse ajal. Pinge peab püsima 5 V juures.

Servo signaalijuhtmele selles plaanis eraldi tõmbetakistit ei lisa. Esimese liikumiskatse ajal on servo siibri hoovastikust lahti.

## 7. Ühenda olemasoleva SPI-puuteekraani signaalid

Pildil on punane plaat kirjaga **„2.4 TFT SPI 240*320”**. Kasutame plaadil olevaid tähiseid; oluline pole see, kas plaati hoitakse parajasti püsti või külili.

**Foto järgi saab määrata signaalid, kuid mitte kindlalt VCC ega LED lubatud toitepinget.** Enne selle kinnitamist valmista allolevad juhtmed ette ja jäta ekraan tervikuna elektriliselt ühendamata. Ka signaalide kaudu ei tohi toita väljalülitatud ekraani.

| Võta ekraanilt kontakt | Ühenda sinna |
|---|---|
| **GND** | **GND-kruviliist** |
| **SCK** | **Klemm nr 9 / GPIO18** |
| **SDI(MOSI)** | **Klemm nr 15 / GPIO23** |
| **SDO(MISO)** | **Klemm nr 10 / GPIO19** |
| **CS** | **Klemm nr 21 / GPIO27** |
| **DC / D/C** | **Klemm nr 22 / GPIO26** |
| **RESET** | **Klemm nr 23 / GPIO25** |
| **T_CLK** | **Sama ühendus kui SCK → klemm nr 9** |
| **T_DIN** | **Sama ühendus kui MOSI → klemm nr 15** |
| **T_DO** | **Sama ühendus kui MISO → klemm nr 10** |
| **T_CS** | **Klemm nr 24 / GPIO33** |
| **T_IRQ** | **Ühendamata** |
| **VCC** | **Ootel: mooduli toitepinge kinnitamine** |
| **LED** | **Ootel: taustvalguse pinge ja voolupiirangu kinnitamine** |

Ekraani küljel olevad **SD_CS, SD_MOSI, SD_MISO ja SD_SCK** on mälukaardi jaoks. Need jätame ühendamata.

### Kolm hargnemiskohta ekraanile ja puutele

Tee kolm väikest, omavahel eraldatud ühenduskohta:

1. **SPI KELL:** klemm nr 9 + ekraani SCK + puute T_CLK.
2. **SPI MOSI:** klemm nr 15 + ekraani SDI(MOSI) + puute T_DIN.
3. **SPI MISO:** klemm nr 10 + ekraani SDO(MISO) + puute T_DO.

Need on signaalijaotused, mitte toitesiinid. Muid ekraani kontakte omavahel kokku ei ühenda. Näiteks CS ja T_CS lähevad eri klemmidele.

Selles signaaliplaanis pole eraldi ekraani tõmbetakisteid ette nähtud. LED-kontakti võimalik voolupiirang selgub mooduli andmetest. **VCC 5 V taluvus ei tähenda automaatselt, et LED-kontakt talub 5 V.**

## 8. Ventilaator

ESP32 annab ainult juhtsignaali. Ventilaatori toitevool läheb läbi sobiva draiveri, mitte läbi ESP32 GPIO.

- **Klemm nr 25 / GPIO32 → ettevalmistatud juhe sildiga „VENT JUHT”.**
- See ühendatakse hiljem valitud draiveri juhtsisendiga, kui selle 3,3 V sobivus ja polaarsus on kinnitatud.
- Draiveri signaalimaa ühendatakse vajaduse järgi ühise GND-ga.
- Ventilaatori toitepinge tuleb ventilaatori sildilt: 12 V ventilaatorit ei saa selle 5 V ühendusplaani järgi täisvõimsusel toita.

**Praegu puuduvad ventilaatori ja draiveri mudelid, seetõttu ventilaatori pluss-, miinus- ja väljundklemme siin ei oletata.** Valmis draiverimooduli vajalikud takistid ja kaitsed sõltuvad moodulist; neid ei lisata juhuslikult. Palja MOSFET-i või transistori juhtskeemi koostame eraldi vastavalt valitud ventilaatorile.

## 9. Summer

- **Klemm nr 18 / GPIO13 → ettevalmistatud juhe sildiga „SUMMER JUHT”.**
- Kolme kontaktiga summerimoodulil saab see minna sobivale **S / IN** sisendile, kui moodul toetab 3,3 V juhtimist.
- Kahe juhtmega summeri puhul sõltuvad ühendus ning võimalik transistor, takistid ja kaitsed summeri tüübist ning voolutarbest.

**Summeri mudel pole veel teada.** Selle toitejuhtmeid ega vajalikku transistori ei määrata ainult välimuse järgi. Jätame selle osa märgistatult ootele.

## 10. Kuidas teha juhtmed ja takistid korralikult?

### Soovituslik paigutus karbis

- Paiguta kolm toitejaotusliistu kõrvuti, kuid elektriliselt eraldi: **GND**, **+5 V**, **+3,3 V**.
- Paiguta 30-klemmiline liist nende lähedale ja jäta numbrid nähtavale.
- Tee andurite ja ekraani hargnemised väikestele eraldi ühendusklemmidele.
- Pane tõmbetakistid väikesele kinnitatud kruviklemmidega vaheplaadile. Märgista selle ühendused **+3,3 V**, **TEMP DATA** ja **AM DATA**.

### Takistite paigutus vaheplaadil

| Takisti | Esimene ots | Teine ots |
|---|---|---|
| **R1: 4,7 kΩ** | **TEMP DATA / klemm nr 11** | **+3,3 V** |
| **R2: 10 kΩ**, kui AM2302-l puudub sisemine takisti | **AM DATA / klemm nr 14** | **+3,3 V** |

Vaheplaadi sama +3,3 V ühendust võivad kasutada mõlema takisti toitepoolsed otsad. TEMP DATA ja AM DATA peavad jääma teineteisest eraldi.

Kasuta tootja lubatud viisil kinnituvaid juhtmeid. Peene kiudjuhtme jaoks sobivad kruviklemmile vastavad otsahülsid; peenikest takistijalga ei pressita liiga suure hülsi sisse. Takisti võib joota vaheplaadile, millel on juhtmetele eraldi kruviklemmid. Ära jäta takistit karpi lahtiselt rippuma.

Juhtmete korrastamiseks:

- Märgista mõlemad otsad, näiteks **T1 +**, **T1 GND**, **T1 DATA**, **S1 SIG**.
- Võimalusel kasuta sisemistes toitejuhtmetes musta GND, punast +5 V ja näiteks oranži +3,3 V jaoks. Märgistus on värvist tähtsam.
- Hoia servode ja ventilaatori toitejuhtmed andmejuhtmetest eraldi ning väldi pikki kõrvuti kulgevaid kimpe.
- Jäta väike hooldusvaru ja kinnita kaablid nii, et sikutamine ei tõmbaks juhtmeid klemmidest välja.
- Ära pane mitut juhet ühe kruvi või pesa alla, kui klemm pole selleks ette nähtud. Selle asemel lisa sobiv jaotusklemm.

## 11. Tööjärjekord ja lõpetamine

1. Tee toiteosa ja mõõda 5 V ning 3,3 V üle.
2. Toide välja. Lisa üks kinnitatud sond ja R1. Katseta sobiva programmiga.
3. Toide välja. Lisa ülejäänud sondid ükshaaval ning märgista mõõtekohad.
4. Lisa kinnitatud kontaktidega AM2302 ja vajaduse korral R2.
5. Ühenda ekraan pärast VCC/LED toiteandmete kinnitamist.
6. Katseta ühte servot korraga, siis mõlemat. Hoovastik jääb alguses lahti.
7. Lisa ventilaatori draiver ja summer pärast nende mudelite kinnitamist.

Kui mõni osa pole veel kinnitatud, saab ülejäänud ühendused ja juhtmete ettevalmistuse siiski ära teha. **See juhend ei tähenda, et kogu tööprogramm on juba ESP32-s.** Ilma sobiva programmita ei pruugi andurid, ekraan ega servod midagi nähtavat teha.

Enne ära minemist tee üks ülevaatefoto ja lähifotod toitejaotustest, takistite vaheplaadist ning 30-klemmilisest liistust.

## Lühike klemmide spikker

| Nr | Ühendus | Nr | Ühendus |
|---:|---|---:|---|
| **1** | +3,3 V jaotuse väljund | **16** | +5 V sisend ESP32 VIN-i |
| **17** | GND-ühendus | **11** | Nelja sondi DATA |
| **14** | AM2302 DATA | **6** | Kambri servo signaal |
| **7** | Möödaviigu servo signaal | **25** | Ventilaatori juhtimine |
| **18** | Summeri juhtimine | **9** | SPI kell |
| **15** | SPI MOSI | **10** | SPI MISO |
| **21** | Ekraani CS | **22** | Ekraani DC |
| **23** | Ekraani RESET | **24** | Puute T_CS |

## Ühenduste alused ja allikad

- Olemasolev `ESP32_klemmide_nimekiri-1(1).pdf`: 1 = 3V3, 16 = VIN, 17 = GND; teiste klemmide seos kontrollitud samast nimekirjast.
- Ekraani signaalitähised on loetud kasutaja saadetud punase SPI-ekraani tagakülje fotolt. VCC ja LED lubatud pinged ei ole sellelt fotolt kindlalt tuvastatavad.
- [DS18B20 tootja andmeleht](https://www.analog.com/media/en/technical-documentation/data-sheets/ds18b20.pdf).
- [Adafruit: DS18B20 andmeliin ja 4,7 kΩ takisti](https://learn.adafruit.com/using-ds18b20-temperature-sensor-with-circuitpython/hardware).
- [Adafruit: DHT22/AM2302 ühendamine](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor).

Uuendatud 17.11.2026. Kooli versioon töötab lõpuks ESP32 enda Wi-Fi võrgus internetita.
