# Juhtkarbi ehitus ja sisemine paigutus

Automaatika jaoks tahan teha 3D-prinditud karbi, kuhu mahub kogu juhtimise elektroonika. Mõte on selles, et juhtmed oleksid korras ja vajadusel saaks mõne anduri või muu detaili lihtsalt ära vahetada. Ei taha lahendust, kus kõik juhtmed on omavahel kokku joodetud ja ühe komponendi vahetamiseks tuleb pool karpi lahti võtta.

Esialgu arvestan karbi välismõõduks umbes **240 × 190 × 85 mm**. Lõplikud mõõdud selguvad siis, kui komponendid on valitud ja nende paigutus üle kontrollitud. Karbi teen eraldi põhja ja eemaldatava kaanega.

## Mis karbi sisse tuleb?

Karbi põhja sisse planeerin kinnituskohad eemaldatavale paigaldusplaadile. Selle külge saab kinnitada ESP32, andurite mõõtemoodulid ja toitemuundurid. Nii saab vajadusel suurema osa elektroonikast koos plaadiga välja tõsta.

ESP32 jaoks tuleb oma kinnituskoht. Plaat kinnitub hoidikusse ning ühendused teen sobivate pesaliistude või adapterplaadi kaudu, et ESP32 oleks vahetatav. USB-pesale jätan ligipääsu programmi laadimiseks. Antenni ümber jätan vaba ruumi ja paigutan selle metallist DIN-liistust ning toitejuhtmetest eemale.

Karbi sisse tuleb ka lühike **35 mm DIN-liist ühendusklemmide jaoks**. Sellele lähevad eelkõige toite jagamise klemmid ning vajadusel kaitsmeklemmid. Klemmidele lisan tähised, et oleks aru saada, milline ühendus kuhu läheb. Kõiki mooduleid ei ole vaja DIN-liistule panna: väiksemad elektroonikaplaadid kinnitan eraldi tugedele.

Andurite mõõtemoodulid paigutan võimalikult lähedale anduripesadele. Toitemuundurid ja mootorite toiteühendused jäävad omaette alasse. Juhtmete jaoks jätan karbi servadesse liikumisruumi ning kinnituskohad, et need ei jääks kaane vahele ega ripuks üle elektroonika.

## Ühendused karbi seintes

Väljast tulevad juhtmed ühendan karbiga pistikute kaudu. Oma ühendused saavad kambriandurid, kaks lihasondi, õueandur, mõlemad servod ja ventilaator. Iga pesa juurde tuleb selge nimetus.

Pistikud valin sellised, et need püsiksid kindlalt kinni. Andurite ja mootorite ühendused peaksid olema võimalikult raskesti segiaetavad. Kaabli tõmme peab jääma pistiku kinnituse või tõmbetõkise kanda, mitte elektroonikaplaadi ühendustele.

Karbi sees kasutan võimalusel pistikühendusi ja eemaldatavaid klemmliiste. Kruviklemmidesse minevatele peenekiulistele juhtmetele panen sobivad juhtmeotsahülsid.

## Kuidas kaas kinnitub?

Kaane kinnitan **M3 kruvidega messingist kuumsisestusmutritesse**. Need on keermestatud sisud, mis surutakse jootekolviga karbi jaoks prinditud kinnituspostidesse. Nii saab kaant korduvalt avada, ilma et kruvi otse plastiku sisse tehtud keeret ära kulutaks.

Kaanele tuleb veidi kaldus ekraanikinnitus, et näitu oleks mugavam lugeda. Lisaks planeerin eemaldatava telefonihoidiku. Ekraani juhtmetele jätan piisavalt pikkust ja lahtiühendamise võimaluse, et kaane eemaldamine oleks lihtne.

## Toide ja hooldamine

230 V toiteplokk jääb karbist välja. Karpi tuleb madalpinge toitesisend ning sees jagatakse toide eraldi elektroonikale, servodele ja ventilaatorile. Telefoni laadimiseks tuleb vajadusel eraldi USB-C laadimismoodul koos oma kinnitusega.

Karbi paigutan suitsuahju kuumast pinnast eemale. Lõplikul paigutamisel arvestan ka toitemuundurite soojenemisega. Eesmärk on teha karp, mida on lihtne avada, mille ühendustest saab aru ning kuhu jääb natuke ruumi ka hilisemateks täiendusteks.
