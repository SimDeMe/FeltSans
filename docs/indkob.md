# Indkøbsliste — fase 1: Plantevægten

Budget: **2.000 kr**, danske forhandlere, skal bruges inden juleferien 2026.
Priser er fra november 2026 og vejledende — tjek i kurven. «ca.» = skøn.

Fase 1 er **én enhed med to vægte**: en potte med plante og en kontrolpotte
side om side, ét sæt klimasensorer. Se [`plan.md`](plan.md).

Kabinet og vægtplatforme printes på skolens 3D-printere.

## Let Elektronik (let-elektronik.dk)

| Vare | Antal | Pris |
|---|---|---|
| [FireBeetle 2 ESP32-C6](https://let-elektronik.dk/firebeetle-2-esp32-c6-iot-development-board-supports-wi-fi-6.-bluetooth-5.-solar-powered) | 2 (1 + reserve/prøvebræt) | 2 × 93 = 186 kr |
| [Vejecelle 5 kg, stangtype (SparkFun TAL220B)](https://let-elektronik.dk/load-cell-5kg.-straight-bar-tal220b) | 2 | ca. 2 × 125 = 250 kr |
| [Qwiic Scale NAU7802](https://let-elektronik.dk/qwiic-scale-nau7802) (vægtforstærker, 3,3 V, I²C) | 2 | 350–500 kr |
| [SHT40 temperatur/luftfugtighed](https://let-elektronik.dk/fermion-sht40-temperature-and-humidity-sensor-breakout) | 1 | 99 kr |
| [AS7341 spektralsensor](https://let-elektronik.dk/fermion-as7341-11-channel-visible-light-sensor-breakout) (PAR + spektrum) | 1 | ca. 220 kr |
| [microSD-breakout (SparkFun, 3,3 V)](https://let-elektronik.dk/shop/1000-hukommelse/544--microsd-transflash-breakout) | 1 | 60 kr |
| Qwiic-kabler | 3 | ca. 50 kr |
| | **Delsum** | **ca. 1.215–1.365 kr** |

Findes 5 kg-cellen ikke på lager: SparkFun 10 kg TAL220 (123,75 kr hos Let
Elektronik) virker også — halv opløsning, stadig under 1 g.

## Ardustore (ardustore.dk)

| Vare | Antal | Pris |
|---|---|---|
| [TCA9548A I²C-multiplexer](https://ardustore.dk/produkt/tca9548a-i2c-multiplexer-expansion-module) (de to NAU7802 har samme adresse) | 1 | ca. 30 kr |
| [Kapacitiv jordfugtsensor](https://ardustore.dk/produkt/capacitive-jord-fugtighedsmaaler-module) | 2 | 50 kr |
| [DS3231 realtidsur](https://ardustore.dk/produkt/ds3231-rtc-realtidsur-module) | 1 | ca. 40 kr |
| [OLED 0,96" I²C](https://ardustore.dk/produkt/display-oled-0-96-i2c-128x64-module-blaa) | 1 | 45 kr |
| [SD-kort 16 GB](https://ardustore.dk/produkt/sd-memory-card-16gb-class-10-adapter) | 1 | ca. 50 kr |
| Breadboard, jumperkabler, stiftlister | – | ca. 100 kr |
| | **Delsum** | **ca. 315 kr** |

## Byggemarked / elektronikbutik

| Vare | Pris |
|---|---|
| Loddetin 0,6–0,8 mm m. flusmiddelkerne, flusmiddel, messinguld-spidsrenser, afloddefletning | ca. 200 kr |
| Wago 221-samlemuffer | ca. 60 kr |
| Bolte og skiver til vejecellerne (M4/M5 — se cellens datablad) | ca. 30 kr |
| | **ca. 290 kr** |

Har allerede: bidetang, multimeter, loddestation (Parkside PLSD 48 B2 —
mejselspidsen ved 330–350 °C), 3D-printere, USB-lader.

## I alt: ca. 1.820–1.970 kr

Strøm fra USB-lader — intet batteri nødvendigt, da enheden står indendørs.

Bliver der penge tilovers: 1 kg hvid PETG til geostationernes kabinetter
(fase 2) eller en DS18B20-probe til jordtemperatur.

## Husk

* **NAU7802 frem for HX711:** de billige HX711-moduler er bygget til 5 V;
  NAU7802 kører direkte på 3,3 V og har Qwiic-stik.
* **Vejecellens retning:** pilen på cellen skal pege i belastningens retning,
  og cellen monteres som en udkraget bjælke (fast i den ene ende, platform på
  den anden). Overbelast den ikke — 5 kg-cellen tåler ca. 7,5 kg før skade.
* **Afdrift:** lad begge vægte veje en sten i et døgn, før det første forsøg.
