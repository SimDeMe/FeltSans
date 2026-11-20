# Udviklingsplan og designbeslutninger

## Fase 1: Plantevægten (vinter 2026–27)

En lille **lysimeter** til potteplanter: to vægte måler, hvor meget vand der
forlader potterne time for time, og sensorer viser hvorfor.

```
   ┌──────────── FireBeetle 2 ESP32-C6 ────────────┐
   │  OLED · SD-kort · DS3231-ur · USB-strøm         │
   └──┬──────────┬──────────┬──────────┬──────────┘
      │ I²C      │          │ analog   │
   TCA9548A    SHT40     2 × jordfugt
   ├─ NAU7802 → vejecelle A   (potte MED plante)
   └─ NAU7802 → vejecelle B   (kontrol: bar jord / anden plante)
            AS7341 (PAR + spektrum)
```

Data: logges på SD-kort (CSV) og vises live på smbi.dk via USB (Web Serial)
eller Bluetooth (Web Bluetooth). Senere evt. Wi-Fi til skyen, så målingen
kan følges hjemmefra.

### Måler

| Størrelse | Sensor | Bruges til |
|---|---|---|
| Masse, 2 potter | 2 × TAL220B 5 kg + NAU7802 | vandtab (g/h) = fordampning + transpiration |
| Lufttemperatur, luftfugtighed | SHT40 | damptryksunderskud (VPD) |
| Lys | AS7341 | PAR (µmol/m²/s) og spektrum |
| Jordfugtighed, 2 potter | kapacitiv | kalibreres mod vægten |

**Hvorfor to vægte på ét board:** potterne står i samme lys og luft, så
forskellen mellem dem skyldes planten — samme dag, samme vejr.

**Forventet præcision:** opløsning ca. 0,1–0,5 g; en stueplante taber typisk
20–100 g/døgn. Billige vejeceller kan drive nogle gram over døgnet (krybning,
temperatur) → mål afdriften med en sten i et døgn først.

**PAR med AS7341:** sensoren er ikke en kvantesensor; PAR beregnes fra 8
farvekanaler (415–685 nm) og skal kalibreres mod en rigtig PAR-måler
(Vernier/Apogee — lån evt.). Kalibreringen afhænger af lyskilden (sol
gennem rude ≠ LED-plantelys).

### Forsøg

1. **Døgnrytme i transpiration** — vandtab pr. time følger lyset
   (spalteåbninger).
2. **CAM-planter** — kalanchoe eller pengetræ (*Crassula*) taber vand om
   natten.
3. **Fordampning vs. transpiration** — plante mod bar jord, eller jorden
   dækket med plastfolie.
4. **VPD** — plastpose over planten → højere luftfugtighed → mindre
   transpiration.
5. **Jordens vandkapacitet** — mætning → markkapacitet → visnegrænse;
   plantetilgængeligt vand.
6. **Kalibrering af jordfugtsensoren** mod vægten (vandindhold i g).

Beregninger eleverne laver: vandtab (g/h), transpiration pr. bladareal,
VPD ud fra temperatur og RF (Magnus-formlen), vandindhold (vægt-%).

### Trin

1. Breadboard: SHT40 + OLED virker.
2. Én vægt (NAU7802 + celle) kalibreret med kendte lodder; afdriftstest.
3. Begge vægte via TCA9548A; SD-logning med DS3231-tid.
4. AS7341 + jordfugt; printet kabinet og vægtplatforme.
5. smbi.dk-side: forbind via USB/Bluetooth, live-graf, hent log, eksport
   (også dansk Excel-venlig CSV: semikolon og decimalkomma).
6. Første rigtige forsøg + vejledning til `smbi/Øvelser`.

## Fase 2: Geostationer

To ens udendørs stationer til naturgeografi:
SHT40 (under printet strålingsskjold), BMP280 (tryk), 3 × DS18B20,
2 solceller + ADS1115 (indstråling og albedo), SD, ur, OLED, LiPo.

Forsøg: jordens varmebølge (prober i 0–40 cm), albedo, energibalance,
byens varmeø, mikroklima, lagdeling i sø, vejrfronter, højde og tryk,
dugpunkt og tåge.

**Indstråling med solcelle:** mål kortslutningsstrømmen (lille shunt,
1–5 Ω) — ikke tomgangsspændingen, som er logaritmisk. Kalibrér mod skolens
W/m²-måler. Spektralt begrænset (Si: 400–1100 nm), ±5–10 %.

## Fase 3 og videre

* Noder med ESP-NOW til en fælles central (én ESP32 pr. målested).
* LoRa og Raspberry Pi-gateway til fjerne stationer.
* Modulstandard (stik, EEPROM-ID), robuste kabinetter, solcelle.
* Undervisningsmateriale og byggesæt.

## Beslutninger

| Emne | Valg | Begrundelse |
|---|---|---|
| Board | FireBeetle 2 ESP32-C6 | Fås i Danmark (93 kr, 78 kr ved 6 stk.), batteristik + lader, solcelleindgang, batterimåling. C3 ville også kunne — boardet koster det samme. |
| Indkøb | Danske butikker | Ægte dele, 1–3 dages levering, faktura. AliExpress er 3–5× billigere på moduler, men med told (3 €/varetype fra 1/7-2026) og risiko for kopier er besparelsen lille ved små ordrer. Relevant senere til klassesæt. |
| Vægtforstærker | NAU7802 | 3,3 V-native, I²C/Qwiic, mindre støj end billige HX711-moduler. |
| Lys | PAR (AS7341) og W/m² (solcelle) — ikke lux | Lux er vægtet efter øjet; omregning til W/m² (≈ lux/110) er usikker, især under trækroner. |
| Til smbi.dk | Web Serial (USB) og Web Bluetooth | Live-graf uden app. Chrome/Edge på Windows, Mac, Chromebook, Android. iPhone kun via Bluefy. |
| Tid | DS3231, gemt i UTC | Undgår sommertidsfejl. |
| Kabinetter | 3D-print på skolen | PETG/ASA udendørs (PLA bliver blød ~55 °C); hvidt strålingsskjold. |
| Firmware | Arduino-rammen via PlatformIO | Modne biblioteker til alle sensorer. |

## Dataformat (udkast)

CSV, én række pr. tidspunkt, enheder i kolonnenavne:

```
tid_utc,masse_A_g,masse_B_g,luft_temp_C,rf_pct,par_umol,jordfugt_A,jordfugt_B
2026-12-10T09:00:00Z,1834.6,1702.1,21.4,38.2,145,0.62,0.58
```

Metadata (sensorer, kalibrering, interval, hvad der står i potte A og B) i
en lille fil ved siden af.

## Inspiration

senseBox (Münster), DFRobot SCI DAQ, Bäumker m.fl. 2021 (PAR fra AS7341,
*Sensors* 21:3390).
