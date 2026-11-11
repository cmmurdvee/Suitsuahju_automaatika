# Suitsuahju automaatika

Teen koolitööna ESP32-põhist suitsuahju juhtseadet. Mõte on selles, et ma ei peaks suitsutamise ajal kogu aeg ahju juures temperatuuri kontrollima ja siibreid käsitsi sättima. Tahan näha, mis toimub suitsukambris ja liha sees, määrata soovitud temperatuurid ning saada teavituse, kui mõni lihatükk hakkab valmis saama või ahi vajab tähelepanu.

Esialgu katsetan lahendust simulaatoris ja maketil. Eesmärk on ehitada seade, mida saaks pärast katsetamist kasutada ka päris suitsuahjul.

## Mida seade tegema hakkab?

Süsteemi juhib üks ESP32-WROOM-32. Selle külge tulevad kaks kambri temperatuuriandurit, kaks lihasondi, välisõhu temperatuuri- ja niiskusandur, kaks siibriservot, ringlusventilaatori juhtimine ning väike puuteekraan.

See teeb kokku **viis temperatuurinäitu ja ühe õhuniiskuse näidu**. Välisandur mõõdab korraga nii temperatuuri kui ka niiskust.

Seadistada ja jälgida tahan järgmisi asju:

- suitsukambri soovitud temperatuur;
- kambri ülemine ja alumine temperatuur ning nende keskmine ja erinevus;
- kummagi lihatüki liik, lõige, kaal ja ligikaudne paksus;
- kummagi liha soovitud sisetemperatuur ja sobiva programmi puhul küpsusaste;
- mõlema liha eraldi valmimisprognoos;
- siibrite juhtasendid ja ventilaatori olek;
- temperatuuride graafikud ja hoiatused.

## Kuidas ahju temperatuuri hoida?

Ahju lahenduses on tulekolde järel kaks suitsuteed. Üks viib kuuma suitsu suitsukambrisse ja teine otse möödaviigukorstnasse. Kummalgi harul on oma siiber ja servo. Suitsukambril on ka oma suitsu väljapääs.

Kui kamber vajab rohkem soojust, on kambriharu rohkem avatud ja möödaviik rohkem suletud. Kui temperatuur läheneb sihile või läheb liiga kõrgeks, avaneb möödaviik ning kambriharu tõmmatakse koomale. Nii saab osa kuumast suitsust juhtida kambrist mööda.

Siibreid tahan liigutada sujuvalt väikeste sammudega. Pärast asendi muutmist peab juhtseade ootama ahju reaktsiooni, sest temperatuur ei muutu kohe. Arvestada tuleb ka sellega, kui kiiresti temperatuur tõuseb, et reguleerimine ei algaks alles pärast ülekuumenemist.

Servo poole käigu peal olemine ei tähenda automaatselt poole väiksemat soojuse juurdevoolu. Õige asendite ja temperatuurimuutuse seos tuleb päris ahjul katsetega leida.

Mõlemad suitsuteed ei tohi korraga sulguda. Selle jaoks tuleb teha sobiv mehaaniline lahendus ja piirata lubatud käike. Alles peab jääma ka käsitsi juhtimise võimalus.

## Milleks kaks kambriandurit ja ventilaator?

Üks temperatuuriandur läheb kambri ülemisse ja teine alumisse ossa. Nende järgi saab arvutada keskmise temperatuuri, aga ainult keskmise vaatamisest ei piisa.

Näiteks võib üleval olla 100 °C ja all 60 °C. Keskmine on 80 °C, kuid eri kõrgusel olev liha on väga erinevates tingimustes. Sellepärast jälgin eraldi nii keskmist, kuumimat mõõtekohta kui ka andurite vahet.

Ringlusventilaatori ülesanne on kambri sees õhku segada. See ei ole tulekolde puhur ega suitsu väljatõmbeventilaator. Kui ülemise ja alumise temperatuuri erinevus kasvab, saab ventilaatori käivitada. Käivitumise ja seiskumise piirid ning viited peavad olema seadistatavad.

Maketil saab seda katsetada tavalise madalpingelise ventilaatoriga. Päris ahju jaoks tuleb valida kuumust, suitsu ja rasvast keskkonda taluv lahendus. Mootori võib olla mõistlik paigutada kambrist väljapoole.

## Kahe erineva liha jälgimine

Mõlemal lihasondil on oma seadistused. Näiteks saan esimese sondi siduda ühe lihatükiga ja teise teistsuguse lihaga. Ühe tüki valmimisel peab tulema selle kohta eraldi teavitus, ilma et teise jälgimine lõpeks.

Mõlemad lihad on siiski samas kambris. Seetõttu ei saa neile korraga määrata kahte erinevat kambritemperatuuri. Kui valitud programmid omavahel kokku ei sobi, peab veebiliides sellest märku andma.

Valikud nagu „medium” ja „well done” tuleb siduda konkreetse lihaliigi ja lõikega. Sama valik ei sobi ühtemoodi kõigile lihadele. Programmis tuleb eristada soovitud tulemust, sihttemperatuuri ja vajadusel hoidmis- või puhkeaega. Ajaprognoos ei asenda sisetemperatuuri mõõtmist ega sobivat küpsetusprogrammi.

## Kuidas hinnata valmimisaega?

Valmimisaega tahan näidata prognoosina, mis küpsetamise jooksul täpsustub. Alguses saab kasutada liha liiki, lõiget, kaalu, paksust ja algtemperatuuri. Edasi saab prognoosi parandada liha tegeliku soojenemise ja kambri temperatuuriajaloo järgi.

Ainult kogukaalust ei piisa. Üks kahekilone tükk ja neli poolekilost tükki võivad vajada erinevat aega. Samuti ei saa eeldada, et liha temperatuur tõuseb kogu aeg ühesuguse kiirusega.

Välisõhu temperatuur ja niiskus annavad katsetele lisatausta. **Välisõhu niiskus ei näita suitsukambri niiskust** ning sellest üksi ei saa liha valmimisaega arvutada. Esialgu on prognoosi peamised alused liha enda soojenemine, tüki mõõtmed ja kambri tegelik temperatuur.

Kui mõõteandmeid on vähe või temperatuur pikalt ei tõuse, on mõistlik näidata laiemat ajavahemikku või teadet, et prognoos alles täpsustub. Prognoosi täpsust saan hinnata alles päris küpsetuste tulemusi võrreldes.

## Ekraan ja telefonist juhtimine

Kohalikult ekraanilt tahan näha põhinäite ja muuta olulisemaid seadeid ka ilma telefonita.

ESP32 sisse tuleb väike veebiserver. Telefonist kasutamiseks on plaanis kaks võimalust:

1. ESP32 loob oma Wi-Fi võrgu ja telefon ühendub otse sellega.
2. ESP32 ühendub kodusesse Wi-Fi võrku ning veebivaate saab avada samas võrgus olevast telefonist.

Kohalik veebivaade ei vaja eraldi internetiserverit ega kuutasulist teenust. Ekraan ja veeb peavad kasutama samu seadistusi: telefonis muudetud sihttemperatuur peab kajastuma ka ekraanil.

Temperatuuri hoidmine peab jätkuma siis, kui telefon ühenduse katkestab või Wi-Fi kaob. Kaugteavitused väljaspool kohalikku võrku on eraldi täiendus, mille jaoks peab seadmel olema sobiv võrguühendus ja teavituse edastamise lahendus.

## Milliseid komponente vaja on ja miks?

Allolev tabel kirjeldab põhivalikuid. Täpsemad kogused, väikekomponendid, ostunimed ja alternatiivid on [komponentide ostunimekirjas](Suitsuahju_komponentide_ostunimekiri.pdf).

| Komponent | Kogus | Milleks seda vaja on? |
|---|---:|---|
| ESP32-WROOM-32 arendusplaat | 1 | Loeb andureid, juhib siibreid ja ventilaatorit ning teenindab ekraani ja veebivaadet. |
| Kambri PT100 temperatuuriandurid | 2 | Mõõdavad kambri ülemist ja alumist temperatuuri. Valida tuleb kogu sondi ja kaabli sobiv kuumataluvus. |
| Toiduga kokkupuuteks sobivad PT100 lihasondid | 2 | Mõõdavad kahe lihatüki sisetemperatuuri. Terav ots teeb sondi paigaldamise lihtsamaks. |
| MAX31865 mõõtemoodulid | 4 | Muudavad PT100 anduri takistuse ESP32 jaoks loetavaks mõõtetulemuseks. Iga sond vajab oma moodulit. |
| AM2302/DHT22 | 1 | Mõõdab välisõhu temperatuuri ja niiskust. |
| SPI-puuteekraan | 1 | Näitab temperatuure, olekuid ja hoiatusi ning võimaldab kohapeal seadistada. |
| Siibriservod | 2 | Liigutavad kambriharu ja möödaviigu siibrit. Vajalik jõud ja liikumisulatus sõltuvad siibritest. |
| Ringlusventilaator | 1 | Aitab vähendada kambri ülemise ja alumise osa temperatuurierinevust. |
| Ventilaatori draiver | 1 | ESP32 annab juhtsignaali, kuid ventilaatori töövool läbib sobivat draiverit. |
| Summer, märgutuled ja nupp | Komplekt | Annab kohapeal hoiatuse ning võimaldab selle kinnitada. |
| Toiteplokk ja pingemuundurid | Komplekt | Annavad eri osadele sobiva pinge ja piisava voolu. Servode toide peab taluma nende voolutippe. |
| Takistid | Komplekt | Piiravad näiteks LED-ide ja transistori baasivoolu ning hoiavad andmeliinid ja nupusisendid kindlal vaiketasemel. |
| Kondensaatorid | Komplekt | Aitavad vähendada kohalikke toitehäireid ja lühiajalisi pingekõikumisi. Need ei asenda piisavat toiteplokki. |
| Transistor ja vajadusel kaitsediood | Komplekt | Võimaldavad summerit eraldi toita ja lülitada; induktiivse koormuse kaitse valitakse selle tüübi järgi. |
| Vajadusel signaalipuhver | 1 | Muudab servo juhtsignaali 3,3 V tasemelt 5 V tasemele, kui valitud servo seda vajab. |
| Kaitsmed, lüliti ja toitejaotus | Komplekt | Võimaldavad toidet välja lülitada ning kaitsta juhtmeid ja toiteharusid liigvoolu korral. |
| Pistikud, juhtmed ja ühendusplaat | Komplekt | Teevad ühendused kindlaks, märgistatavaks ja vajadusel lahtiühendatavaks. |
| Korpus, siibrid, hoovad ja kinnitused | Komplekt | Hoiavad elektroonika ja mehhanismi paigal ning võimaldavad seadme ahjule kinnitada. |

Põhivariandis on neli PT100 sondi. Kui lihasondideks valida hoopis NTC-sondid, tuleb muuta nende mõõteahelat ja kahe MAX31865 asemel kasutada vastavat lahendust. Alternatiive ei ole vaja põhikomplektile lisaks osta.

## Toide ja töökindlus

Toite lähteplaan on väline 12 V adapter, millest tehakse eraldi 5 V haru servodele ja eraldi 5 V haru juhtplokile. Ventilaatori toide ja draiver sõltuvad lõplikult valitud mudelist. Madalpingeliste juhtimisahelate maandus peab olema õigesti ühendatud.

Servosid ega ventilaatorit ei toideta ESP32 signaaliviikudest. Toiteplokk, juhtmed ja pistikud tuleb valida ka käivitus- ning seiskumisvoolu järgi, et siibrite liikumine ei põhjustaks juhtploki taaskäivitumist.

Päris ahjul tuleb läbi mõelda ka andurivead, ülekuumenemine, kinnijäänud siiber ja elektrikatkestus. Servo käsk ei tõesta, et siiber päriselt õigesse asendisse jõudis. Selle kontrollimiseks on plaanis kasutada asenditagasisidet või muud sobivat kontrolli.

Tavaline servo ei ava voolu kadumisel ise möödaviiku. Rikkeasend tuleb lahendada koos mehaanikaga. Elektroonika, servod ja 3D-prinditud korpus jäävad kuumast alast eemale.

## Projekti hetkeseis

- Süsteemi põhimõte ja soovitud funktsioonid on kirjeldatud.
- Koostatud on esialgne ühendusplaan ja komponentide ostunimekiri.
- Wokwis on käivitatud varasem näidisversioon: ekraanil on olnud näha temperatuurid ja juhtimise olekud.
- Varasem simulatsioon kasutas kolme kambriandurit ja kahte lihasondi. Praeguses plaanis on kaks kambriandurit ja kaks lihasondi; välisandur annab viienda temperatuurinäidu.
- Simulatsiooni DS18B20 andurid ei kinnita päris ahju sondide sobivust. Päris seadme põhivariandiks on praegu valitud PT100 koos MAX31865 moodulitega.
- Veebiliides, puutejuhtimine, valmimisprognoos ja päris ahju katsed on veel tegemisel või planeeritud, mitte valmis funktsioonid.

Wokwi abil saab proovida programmi ja juhtimiskäske, kuid see ei näita, kui kiiresti päris ahi soojeneb, kui palju jõudu siiber vajab või kui ühtlaseks ventilaator kambri muudab.

### Viiguplaan

1. Must - 3V                       16. Roheline - VIN                 
2. Valge - GND                     17. Kollane - GND
3. Hall - D15                      18. Oranž - D13
4. Lilla - D2                      19. Punane - D12
5. Sinine - D4                     20. Pruun - D14
6. Roheline - RX2                  21. Must - D27
7. Kollane - TX2                   22. Valge - D26
8. Oranž - D5                      23. Hall - D25
9. Punane - D18                    24. Lilla - D33
10. Pruun - D19                    25. Sinine - D32
11. Must - D21                     26. Roheline - D35
12. Valge - RX0                    27. Kollane - D34
13. Hall - TX0                     28. Oranž - VN   
14. Lilla - D22                    29. Punane - VP
15. Sinine - D23                   30. Pruun - EN



## Edasine töö

1. Kinnitan sondide, ekraani, servode ja ventilaatori täpsed mudelid.
2. Teen nende põhjal lõpliku elektriskeemi ja viiguplaani.
3. Katsetan päris andureid ning võrdlen näite kontrolltermomeetriga.
4. Katsetan toidet, servode liikumist ja ventilaatori juhtimist.
5. Lisan kambri automaatjuhtimise ja kahe liha eraldi seadistused.
6. Teen kohaliku ekraanivaate ja telefonis kasutatava veebiliidese.
7. Lisan ajaloo, prognoosi ja teavitused.
8. Katsetan süsteemi maketil ning seejärel järelevalve all päris ahjul.

Tulemuse hindamiseks tahan võrrelda, kui stabiilselt püsib kambri temperatuur, kui palju väheneb ülemise ja alumise osa temperatuuride erinevus ning kui täpseks muutub valmimisaja prognoos.

## Tehnilised lähteallikad

- [ESP32-WROOM-32 tootja dokumentatsioon](https://documentation.espressif.com/esp32-wroom-32_datasheet_en.html)
- [MAX31865 mõõtemuunduri tootja leht](https://www.analog.com/en/products/max31865.html)
- [MAX31865 mõõtemooduli ühendamine](https://learn.adafruit.com/adafruit-max31865-rtd-pt100-amplifier)
- [ESP32 Wi-Fi võimalused](https://docs.espressif.com/projects/arduino-esp32/en/latest/api/wifi.html)
