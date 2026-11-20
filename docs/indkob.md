# Indkøbsliste — fase 1

Budget: **2.000 kr**, danske forhandlere. Priser er fra november 2026 og
vejledende — tjek i kurven. Priser markeret «ca.» er skøn.

Resultat: **1 central** (skærm, ur, SD-kort) + **4 noder** (2 klima, 2
temperatur) + 1 reserveboard til breadboard-forsøg.

## Let Elektronik (let-elektronik.dk)

| Vare | Antal | Stk.-pris | I alt |
|---|---|---|---|
| [FireBeetle 2 ESP32-C6](https://let-elektronik.dk/firebeetle-2-esp32-c6-iot-development-board-supports-wi-fi-6.-bluetooth-5.-solar-powered) | 6 | 78 kr (mængderabat) | 468 kr |
| [SHT40 temperatur/fugt](https://let-elektronik.dk/fermion-sht40-temperature-and-humidity-sensor-breakout) (±0,2 °C, ±1,8 % RF) | 2 | 99 kr | 198 kr |
| [VEML7700 lys, 0–120 klx](https://let-elektronik.dk/gravity-i2c-veml7700-ambient-light-sensor-0120klx) | 2 | 132 kr | 264 kr |
| [LiPo 3,7 V 2000 mAh, PH2.0 (Hadex)](https://let-elektronik.dk/rechargeable-battery-lipo-3.7v-2000mah-634169-hadex) | 3 | 95 kr | 285 kr |
| | | **Delsum** | **ca. 1.215 kr** |

6 boards koster det samme som 5 pga. mængderabatten.

## Ardustore (ardustore.dk)

| Vare | Antal | I alt |
|---|---|---|
| [OLED 0,96" I²C 128×64](https://ardustore.dk/produkt/display-oled-0-96-i2c-128x64-module-blaa) | 1 | 45 kr |
| [microSD-modul (SPI, 5 V)](https://ardustore.dk/produkt/micro-sd-kort-hukommelse-spi-module) | 1 | 20 kr |
| SD-kort 16 GB | 1 | ca. 50 kr |
| [DS3231 realtidsur](https://ardustore.dk/produkt/ds3231-rtc-realtidsur-module) | 1 | ca. 40 kr |
| [Kapacitiv jordfugtsensor](https://ardustore.dk/produkt/capacitive-jord-fugtighedsmaaler-module) | 2 | 50 kr |
| [DS18B20 vandtæt, 1–2 m](https://ardustore.dk/produkt/ds18b20-temperatur-sensor-1-meter) | 6 | ca. 225 kr |
| Breadboard, jumperkabler, stiftlister, 4,7 kΩ-modstande | – | ca. 100 kr |
| | **Delsum** | **ca. 530 kr** |

## Byggemarked / elektronikbutik

| Vare | I alt |
|---|---|
| Loddetin 0,6–0,8 mm m. flusmiddelkerne, flusmiddel (no-clean), messinguld-spidsrenser, afloddefletning | ca. 200 kr |
| Afisoleringstang | ca. 100 kr |
| Wago 221-samlemuffer (10 stk.) | ca. 60 kr |
| 3–4 IP65-samledåser + PG7-kabelforskruninger | ca. 150 kr |
| | **ca. 510 kr** |

Har allerede: bidetang, multimeter, loddestation (Parkside PLSD 48 B2 —
brug mejselspidsen ved 330–350 °C).

## I alt: ca. 2.250 kr — sådan kommer vi ned på 2.000

* BH1750 ([61 kr](https://let-elektronik.dk/fermion-bh1750-light-sensor-breakout))
  i stedet for VEML7700: −140 kr. Ulempe: mætter ved ca. 65 klx (direkte sol
  er ~100 klx), fint i skygge og skov.
* Kun 2 LiPo nu, kør én node på powerbank: −95 kr.
* Spar evt. på dåserne (madkasser med kabelgennemføring kan bruges til at starte).

## Husk

* **Mål batteriets polaritet med multimeteret**, før det sættes i boardet.
  LiPo-stik er ikke standardiseret, og forkert polaritet ødelægger laderen.
* SD-modulet kræver 5 V → centralen kører på USB (powerbank/lader).
  Billige powerbanks kan slukke ved lavt forbrug — prøv den, du har.
* Strålingsskjold til lufttemperatur: 5–6 hvide underskåle til urtepotter
  stablet med afstandsstykker. Sensoren skal sidde **under skjoldet**, ikke i
  dåsen.
