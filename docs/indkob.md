# Indkøbsliste — fase 1: Plantevægten (slank version)

Alle priser er **ex moms** (butikkernes priser inkl. moms er delt med 1,25
og rundet af).

Mål: **ca. 800 kr** til plantevægten, **ca. 970 kr** med temperaturboardet
nederst (loft: 2.000 kr). Danske forhandlere, skal bruges inden
juleferien 2026. Priser er fra november 2026 og vejledende — tjek i kurven.
«ca.» = skøn. Priserne på HX711-moduler (vægtforstærkere) og DS18B20-prober
(temperaturfølere) er ikke slået op endnu.

Fase 1 er **én enhed med to vægte**: en potte med plante og en kontrolpotte
side om side, ét sæt klimasensorer. Se [`plan.md`](plan.md).

Kabinet og vægtplatforme printes på skolens 3D-printere.

Ordforklaring: modelnavne har en forklaring i parentes, hver gang de nævnes.

## Let Elektronik (let-elektronik.dk)

| Vare | Antal | Pris ex moms |
|---|---|---|
| [FireBeetle 2 ESP32-C6](https://let-elektronik.dk/firebeetle-2-esp32-c6-iot-development-board-supports-wi-fi-6.-bluetooth-5.-solar-powered) (mikrocontrollerbræt, altså «hjernen», der læser sensorerne og gemmer data) | 1 | 74 kr |
| [Vejecelle 5 kg, stangtype (SparkFun TAL220B)](https://let-elektronik.dk/load-cell-5kg.-straight-bar-tal220b) (metalbjælke, der bøjer en my under vægt og giver et lille elektrisk signal) | 2 | ca. 2 × 100 = 200 kr |
| [SHT40 temperatur/luftfugtighed](https://let-elektronik.dk/fermion-sht40-temperature-and-humidity-sensor-breakout) (lille sensor, der måler lufttemperatur og luftfugtighed) | 1 | 79 kr |
| [microSD-breakout (SparkFun, 3,3 V)](https://let-elektronik.dk/shop/1000-hukommelse/544--microsd-transflash-breakout) (lille print, der lader boardet skrive til et SD-kort) | 1 | 48 kr |
| | **Delsum** | **ca. 400 kr** |

Findes 5 kg-cellen ikke på lager: SparkFun 10 kg TAL220 (en tilsvarende
vejecelle til 10 kg, ca. 99 kr ex moms hos Let Elektronik) virker også —
halv opløsning, stadig under 1 g.

## Ardustore (ardustore.dk)

| Vare | Antal | Pris ex moms |
|---|---|---|
| HX711-modul (vægtforstærker: forstærker vejecellens svage signal og omsætter det til et tal, som boardet kan læse) | 2 | ca. 2 × 32 = 64 kr (skøn) |
| [Kapacitiv jordfugtsensor](https://ardustore.dk/produkt/capacitive-jord-fugtighedsmaaler-module) (pind, der måler, hvor våd jorden er, uden at komme i kontakt med vandet) | 2 | 40 kr |
| [DS3231 realtidsur](https://ardustore.dk/produkt/ds3231-rtc-realtidsur-module) (lille ur med batteri, så tidsstemplerne stemmer, også efter strømafbrydelse) | 1 | ca. 32 kr |
| [OLED 0,96" I²C](https://ardustore.dk/produkt/display-oled-0-96-i2c-128x64-module-blaa) (lille skærm, der viser aktuelle målinger) | 1 | 36 kr |
| [SD-kort 16 GB](https://ardustore.dk/produkt/sd-memory-card-16gb-class-10-adapter) | 1 | ca. 40 kr |
| Breadboard (stikbræt uden lodning), jumperkabler, stiftlister | – | ca. 80 kr |
| | **Delsum** | **ca. 290 kr** |

## Byggemarked / elektronikbutik

| Vare | Pris ex moms |
|---|---|
| Loddetin 0,6–0,8 mm m. flusmiddelkerne og afloddefletning (fletning, der suger tin op, hvis man fejllodder) | ca. 80 kr |
| Bolte og skiver til vejecellerne (M4/M5 — se cellens datablad) | ca. 24 kr |
| | **ca. 105 kr** |

Har allerede: bidetang, multimeter, loddestation (Parkside PLSD 48 B2 —
mejselspidsen ved 330–350 °C), 3D-printere, USB-lader.

Droppet fra den første liste: Wago 221-samlemuffer (klemmer til at samle
ledninger), messinguld-spidsrenser og ekstra flusmiddel. Kan købes, hvis
der viser sig behov.

## Plantevægten i alt: ca. 800 kr ex moms

Strøm fra USB-lader — intet batteri nødvendigt, da enheden står indendørs.

## Tillæg: temperaturboard med 3 prober (ca. 175 kr ex moms)

Et ekstra, selvstændigt board med tre vandtætte temperaturprober. Det
bruges til at måle temperatur flere steder ad gangen, fx i jorden i
planteforsøgene (potte A, potte B og luften ved siden af) eller i forskellige
dybder i jord eller vand.

| Vare | Antal | Pris ex moms |
|---|---|---|
| FireBeetle 2 ESP32-C6 (endnu et mikrocontrollerbræt, som ovenfor) — Let Elektronik | 1 | 74 kr |
| DS18B20-probe (vandtæt temperaturføler på ca. 1 m ledning, kan stikkes i jord eller vand; præcision ca. ±0,5 °C) — tjek Ardustore eller Let Elektronik | 3 | ca. 3 × 32 = 96 kr (skøn) |
| Modstand 4,7 kΩ (lille komponent, der gør, at prober og board kan «tale sammen» på samme ledning) | 1 | ca. 4 kr |
| | **Delsum** | **ca. 175 kr** |

* **Kun ét ben på boardet:** DS18B20 (temperaturføler) bruger et såkaldt
  1-Wire-system (en fælles ledning, som alle tre prober deles om, hver med
  sit unikke nummer). Derfor er tre prober ikke sværere end én.
* **Uden SD-kort og ur:** boardet kan til at starte med sende data live til
  smbi.dk via USB (Web Serial), mens computeren står tændt. Skal det logge
  selv, lægges SD-breakout (ca. 48 kr), SD-kort (ca. 40 kr) og DS3231
  (ur, ca. 32 kr) til, altså ca. 120 kr ekstra.
* **Brug til andet end planter:** det samme board kan i fase 2 bruges til
  jordens varmebølge (prober i 0–40 cm dybde) eller lagdeling i en sø.

**I alt med temperaturboard: ca. 970 kr ex moms.**

## Hvad er sparet væk — og hvornår skal det med?

| Udeladt | Pris ex moms | Hvornår |
|---|---|---|
| AS7341 (lyssensor, der måler flere farver hver for sig, så man kan regne ud, hvor meget lys planten kan bruge, kaldet PAR) | ca. 176 kr | Til trin 5 i planen. Kræver desuden en lånt rigtig lysmåler (Vernier/Apogee) til kalibrering. [Fermion AS7341 hos Let Elektronik](https://let-elektronik.dk/fermion-as7341-11-channel-visible-light-sensor-breakout). |
| Reservebræt FireBeetle 2 ESP32-C6 (ekstra mikrocontrollerbræt) | 74 kr | Hvis et board går i stykker. Midlertidigt kan temperaturboardet fungere som reserve. |
| Opgradering til NAU7802 (nyere, mere støjsvag vægtforstærker, her som [Qwiic Scale](https://let-elektronik.dk/qwiic-scale-nau7802) med stik uden lodning) | netto ca. 280–400 kr ekstra | Hvis afdriftstesten viser for meget støj eller drift med HX711. Kræver også TCA9548A (omskifter, der lader to ens sensorer dele ledninger, ca. 24 kr hos [Ardustore](https://ardustore.dk/produkt/tca9548a-i2c-multiplexer-expansion-module)) og Qwiic-kabler (ca. 40 kr), fordi de to NAU7802 har samme adresse på I²C (to-leder-forbindelsen, sensorerne taler over). |
| 1 kg hvid PETG (3D-printmateriale, der tåler sol og vejr) til geostationernes kabinetter | – | Fase 2. |

## Husk

* **HX711 (vægtforstærker) frem for NAU7802 (vægtforstærker) — men med
  forbehold:** HX711 (den billige vægtforstærker) kører ifølge databladet på
  2,6–5,5 V, så den fungerer normalt fint på boardets 3,3 V. NAU7802 (den
  dyrere) har mindre støj og læser hurtigere (80 målinger i sekundet mod 10),
  men det er en opgradering og ikke et krav. Afdriftstesten afgør, om HX711
  (vægtforstærker) er god nok. Tjek at det indkøbte modul kan køre på 3,3 V.
* **Ben på boardet:** HX711 (vægtforstærker) bruger to almindelige ben (GPIO)
  hver — i alt 4 til de to vægte, plus ben til SD-kortet og
  jordfugtsensorerne. Tjek boardets benplacering, før der loddes.
* **Vejecellens retning:** pilen på cellen skal pege i belastningens retning,
  og cellen monteres som en udkraget bjælke (fast i den ene ende, platform på
  den anden). Overbelast den ikke — 5 kg-cellen tåler ca. 7,5 kg før skade.
* **Afdrift:** lad begge vægte veje en sten i et døgn, før det første forsøg.
* **DS18B20 (temperaturføler) mod luft:** proben sidder i en metalspids og
  reagerer langsomt (et minut eller to). Til hurtige luftændringer er SHT40
  (fugtsensor) bedre.
