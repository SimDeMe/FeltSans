# Udviklingsplan og designbeslutninger

## Beslutninger (november 2026)

| Emne | Valg | Begrundelse |
|---|---|---|
| Arkitektur | Én ESP32 pr. målested + central | Mikroklima handler om at sammenligne steder samtidig. Billigere pr. målested; robust hvis én node fejler. |
| Board | FireBeetle 2 ESP32-C6 (central og noder) | Fås i Danmark (78 kr ved 6 stk.), batteristik + lader, solcelleindgang, batterispændingsmåling. C3 ville også kunne, men boardet koster det samme. |
| Radio | ESP-NOW | Ingen router/internet, rækkevidde 50–200 m, lavt strømforbrug. |
| Til smbi.dk | Web Serial (USB) og Web Bluetooth fra en side på smbi.dk | Live-graf uden app. Virker i Chrome/Edge på Windows, Mac, Chromebook og Android — **ikke iPhone**. USB er den sikre vej i klassen. |
| Tid | Centralen tidsstempler (DS3231); urets tid sættes fra browseren | Alle målinger får samme ur. Gem i UTC. |
| Datasikkerhed | Noder bufrer i egen flash og eftersender | Intet tabes, hvis centralen er uden for rækkevidde. |
| Firmware | Arduino-rammen via PlatformIO | Modne biblioteker til alle sensorerne. |

## Sensorer i fase 1

| Node | Sensorer | Bus |
|---|---|---|
| Klima (×2) | SHT40 (T, RF), VEML7700 (lys), kapacitiv jordfugt, DS18B20 (jordtemp.) | I²C, analog, 1-Wire |
| Temperatur (×2) | 3 × DS18B20 på samme ledning (luft/vand/jord) | 1-Wire |
| Central | DS3231 (ur), OLED, microSD | I²C, SPI |

DS18B20: hver probe har sit eget 64-bit ID → nummerér proberne fysisk og
gem ID→nummer i en konfigurationsfil. Kalibrér i isvand (0 °C).

Jordfugt: de kapacitive sensorer giver relative værdier og skal kalibreres
mod jordprøver (vejning før/efter tørring) — god elevøvelse.

## Faser

1. **Prototype** — én node med SHT40 på breadboard, værdier på OLED. Derefter
   ESP-NOW mellem node og central.
2. **Logger** — dyb søvn mellem målinger, buffer i flash, SD-logning, ur,
   batterimåling. Strålingsskjold. **Felttest med alle 4 noder** (skov /
   græs / by).
3. **smbi.dk-side** — forbind via USB/Bluetooth, live-graf, hent log,
   eksport (også dansk Excel-venlig CSV: semikolon og decimalkomma).
4. **Robust udgave** — faste stik, dåser, solcelle på langtidsnoder.
5. **Netværk** — LoRa og evt. Raspberry Pi-gateway til stationer langt væk.
6. **Undervisningsmateriale og byggesæt.**

## Dataformat (udkast)

CSV, én række pr. tidspunkt og node, enheder i kolonnenavne:

```
tid_utc,node,luft_temp_C,rf_pct,lys_lux,jordfugt_rel,jord_temp_C,batteri_V
2026-12-10T09:00:00Z,skov,4.21,88.3,1520,0.62,3.9,3.98
```

Metadata (enheder, sensorer, nøjagtighed, interval, stationsnavne,
kalibrering) i en lille fil ved siden af.

## Åbne spørgsmål

* Kabinet: 3D-printer tilgængelig? Ellers IP65-dåser.
* Licenser (forslag i README).
* Inspiration: senseBox (Münster) og DFRobot SCI DAQ (færdig logger med
  automatisk sensorgenkendelse).
