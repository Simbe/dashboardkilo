# Kilon kotinäyttö (dashboardkilo)

Perheen seinänäyttö Kilossa, Espoossa. Pääkäyttäjät ovat aikuinen ja 10-vuotias lapsi. Lapsi katsoo siitä aamulla, mikä bussi tai juna lähtee. **Yksinkertainen on parempi**: lapsen pitää ymmärtää näyttö yhdellä silmäyksellä.

- **Tiedosto:** `index_kani.html`
- **Osoite:** https://simbe.github.io/dashboardkilo/index_kani.html
- **Laite:** Samsung Galaxy Tab S2, **AMOLED-näyttö** (kiinni palamisen vaara), pystynäkymä 768×1024, Fully Kiosk Browser. Pidä JavaScript ES5-tyylisenä kuten nyt (vanha Android-laite).
- **Kieli:** suomi.
- **HSL:n API-avain** on julkisella sivulla omistajan päätöksellä.

## Mitä näytössä on (versio 3.5)

- **Ylärivi:** päivämäärä, liputuspäivän lippu, kello.
- **Sääpaneeli:**
  - lämpötila, olotila, tuntuu kuin;
  - tuuli, puuskat, kosteus;
  - sähkön hinta nyt (spot-hinta.fi; värirajat 5 / 10 / 20 snt) ja halvin tunti ("Halvin 2–3");
  - auringonnousu ja -lasku, päivän pituus ja muutos.
- **Ennuste:** tuntinauha ja päivänauha.
- **Junat Kilon asemalta (KIL),** Digitraffic: kaksi ryhmää, Helsingin suunta ja länteen. Enintään `TRAIN_ROWS = [5, 5]`.
- **Bussit Trillakadulta,** HSL Digitransit:
  - pysäkki E1827 (Leppävaaran suunta) **aina ylempänä**, E1826 (Matinkylän suunta) alempana; enintään `BUS_ROWS = [5, 5]`;
  - **`HIDE_LINES = ['227K', '229']`**: nämä linjat piilotetaan. Kaikki muut näkyvät, myös uudet numerot ja korvaavat bussit (omistajan toive: mieluummin piilotetaan kuin valitaan näytettävät);
  - pelkästään piilotettuja linjoja koskevat HSL-tiedotteet eivät näy.
- **Lähtöjen näyttö:**
  - alle 15 min (`SOON_MIN`) minuutteina, muuten kellonaikana;
  - peruttu lähtö yliviivattuna ja himmennettynä, **ilman sanaa "Peruttu"**;
  - pitkät linjatunnukset saavat luokan `dep__line--long`.
- **Rivien sovitus:** `fitBoard()` poistaa rivejä pidemmän ryhmän alapäästä, kun tiedotteet vievät tilaa. Ensimmäinen ryhmä pitää ylimääräisen rivin.
- **Tiedotteet:** Digitrafficin junatiedotteet ja HSL-tiedotteet yhdellä rivillä ("+1 muuta").
- **Ukkosvaroitus** (salama alle 20 km, 30 min).
- **AMOLED-suojaus:**
  - pikselisiirto 3 minuutin välein (`pixelShift`);
  - yöhimmennys klo 22–06 (`NIGHT_DIM = 0.45`).

## Kani (lapsen ilahduttaja)

- **Kanin vaatteet** seuraavat tuntuu kuin -lämpötilaa: pipo ≤ 8 °C, kaulahuivi ≤ −5 °C.
- **Juhlapäivät ja sää:**
  - juhla-asusteet (`kaniHoliday`): joulupukki, kokardi, vappu, juhannus, halloween;
  - sateenvarjo;
  - lumikasa (localStorage `kani-lumi`).
- **Pukeutumisvinkit** (`clothingTip`) aamuisin ja iltapäivisin (`TIP_TIMES`).
- **Puhekupla:** 100 repliikkiä (`KANI_LINES`), sekoitettuna niin, että kaikki tulevat kerran ennen toistoa. Kupla on kanin oikealla puolella, eikä se saa peittää kelloa tai päivämäärää.
- **`fitKani()`** pienentää kania, jos tila ei riitä.
- **Asetukset:**
  - `BIRTHDAYS` (synttärit);
  - `RINGETTE_PRACTICE_DAYS` ja `RINGETTE_GAME_DATES` (ringette).
- **Pitkä painallus kaniin** näyttää version: "Kilon kotinäyttö · versio X".
- **Jättikani** (pääsiäismuna):
  - avautuu, kun vasenta yläkulmaa (päivämäärää) napautetaan 5 kertaa;
  - koko ruudun kani pomppii ja sanoo "Mä ihan tärisen!";
  - kania voi silittää, jolloin se rauhoittuu ja sulkee silmänsä;
  - **napautus ei sulje sitä**, koska omistaja ei halua sen katoavan vahingossa.

## Tehdyt päätökset (älä muuta kysymättä)

- Sadetutka poistettu. Lähdedatarivi ja alapalkki poistettu, jotta lähtöriveille jää enemmän tilaa.
- Leppävaaran bussit aina ylempänä; aamujärjestyksen vaihto poistettu.
- Perutuissa ei sanaa "Peruttu", pelkkä yliviivaus ja himmennys.
- FMI:n säävaroitukset jätetty pois (ei CORSia). Nimipäiviä ei näytetä (lisensoitu).
- Avoin kysymys: säätekstin "Lumijyväsiä" voisi vaihtaa muotoon "Hienoa lumisadetta". Ei vielä päätetty.
- Palaute 23.11.2026: lapsi ei saanut aamulla selvää oikeasta bussista. Ratkaisuksi piilotettiin 227K ja 229, muuten kaikki pidettiin ennallaan.

## Työskentelysäännöt (koskevat kaikkia muutoksia)

- **Kieli:** vastaa suomeksi ja selkokielellä. Omistaja ei ole ohjelmoija, joten selitä mitä muutos tarkoittaa käytännössä, älä koodia.
- **Kerro suunnitelma ennen kuin teet mitään:** mitä teet ja mitä jätät tekemättä. Jos jokin on epäselvää, kysy ensin. Jos omistaja pyytää vain vastausta tai selvitystä, älä muuta koodia.
- **Versio:** nosta jokaisessa muutoksessa `VERSION`-muuttujaa (pieni muutos 1.0 → 1.1, iso muutos → 2.0).
- **Commit-viesti** aina tässä muodossa:
  ```
  Version X.Y
  - muutos 1
  - muutos 2
  ```
- **Kuvat:** anna kuvakaappaukset ennen ja jälkeen aina, kun ulkoasu muuttuu. Tee kuvat laitteen oikealla näyttökoolla.
- **Testaa ennen julkaisua:** Playwright, Chromium ja WebKit. Testaa valekellolla (`page.clock.install`) ja korvaa kaikki rajapinnat mock-vastauksilla (`page.route`), jotta eri tilanteet saadaan näkyviin (yö, varoitukset, virheet, perutut lähdöt). Tarkista, ettei mikään mene ruudun yli tai katkea. Open-Meteo antaa 429-virheen, jos sitä kutsutaan testeissä liikaa, joten käytä mockeja.
- **Pidä sivu yhtenä tiedostona:** HTML, CSS ja JS samassa tiedostossa, ei build-vaihetta eikä riippuvuuksia (paitsi Leaflet CDN:stä, jos karttaa käytetään).
- **Sivu on julkinen** (GitHub Pages): ei henkilötietoja, terveystietoja eikä tarkkoja kotiosoitteita/koordinaatteja.
- **Julkaisu:** muutos menee haaraan ja pull requestiin. Kun omistaja hyväksyy (merge) päähaaraan, GitHub Pages julkaisee sen. Pages välimuistittaa noin 10 minuuttia. Livenä olevan version voi tarkistaa hakemalla sivun ja katsomalla `VERSION`-arvon.
- **Säännöt kaikkien kolmen näytön kesken:** sama koodipohja on käytössä myös repoissa `weatherlassila`, `dashboardkilo` ja `dashboardketunkorpi`. Jos korjaat yleisen bugin (esim. sää, tutka, liputuspäivät), mainitse, että sama korjaus voi koskea muitakin näyttöjä.

## Tekniset huomiot

- **Koko:** kaikki mitoitetaan muuttujasta `--u` (ks. CSS:n alku), jotta näyttö täyttyy tarkasti. Fonttikoot tulevat muuttujista `--fs-xs` … `--fs-2xl`; älä lisää irrallisia px-kokoja.
- **Aikavyöhyke:** kaikki kellonajat lasketaan Europe/Helsinki-ajassa apufunktioilla (`helsinkiDateKey`, `helsinkiHour` jne.), ei laitteen paikallisajassa.
- **Rajapinnat, jotka toimivat suoraan selaimesta (CORS):** Open-Meteo, FMI WFS (ennuste, salamat), FMI WMS (sadetutka), Digitraffic (junat ja tiedotteet), HSL Digitransit, spot-hinta.fi.
- **Eivät toimi selaimesta (ei CORSia):** FMI:n CAP-varoitukset, MeteoAlarm, porssisahko.net. Nämä vaatisivat välipalvelun.
- **Sadetutka:** uusin kuva-aika luetaan GetCapabilities-vastauksesta (muuten CORB-ongelmia).
- **Nimipäiväluettelo on lisensoitu** (Almanakkatoimisto), joten sitä ei käytetä. Liputuspäivät tulevat virallisesta listasta (`FLAG_DAYS` + sääntöpohjaiset päivät).
