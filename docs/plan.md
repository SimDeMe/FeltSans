# Udviklingsplan og designbeslutninger

Modelnavne har en forklaring i parentes, hver gang de nævnes.

## Fase 1: Plantevægten (vinter 2026–27)

En lille **lysimeter** (vægt, der måler en plantes vandforbrug) til
potteplanter: to vægte måler, hvor meget vand der forlader potterne time for
time, og sensorer viser hvorfor.

```
   ┌──────── FireBeetle 2 ESP32-C6 (mikrocontrollerbræt) ────────┐
   │  OLED (skærm) · SD-kort · DS3231 (ur) · USB-strøm            │
   └──┬───────────┬─────────────┬─────────────────┬──────────────┘
      │ I²C       │ 2 ben hver  │ analog          │ senere
    SHT40       HX711 A       2 × jordfugt        AS7341
  (luft)       HX711 B                            (PAR + spektrum)
                  │ │
         vejecelle A  vejecelle B
        (potte MED plante)  (kontrol: bar jord / anden plante)

   Selvstændigt tillæg: FireBeetle + 3 × DS18B20 (temperaturprober)
```

* **I²C** er en to-leder-forbindelse, som skærmen, uret og SHT40
  (temperatur- og fugtsensor) deler. HX711 (vægtforstærker, der omsætter
  vejecellens signal til et tal) bruger i stedet to almindelige ben
  (GPIO) hver, så ingen af dem skal deles via en I²C-omskifter.
* **AS7341** (lyssensor, der måler flere farver hver for sig) er ikke med i
  første version. Den kommer til i trin 5.

Data: logges på SD-kort (CSV) og vises live på smbi.dk via USB (Web Serial)
eller Bluetooth (Web Bluetooth). Senere evt. Wi-Fi til skyen, så målingen
kan følges hjemmefra.

### Måler

| Størrelse | Sensor | Bruges til |
|---|---|---|
| Masse, 2 potter | 2 × TAL220B 5 kg (vejecelle, metalbjælke der bøjer en my under vægt) + 2 × HX711 (vægtforstærker) | vandtab (g/h) = fordampning + transpiration |
| Lufttemperatur, luftfugtighed | SHT40 (lille temperatur- og fugtsensor) | damptryksunderskud (VPD; hvor tør luften er for planten) |
| Jordfugtighed, 2 potter | kapacitiv sensor (pind, der måler fugt uden kontakt med vandet) | kalibreres mod vægten |
| Temperatur flere steder | 3 × DS18B20 (vandtæt temperaturprobe) på eget board | jordtemperatur i begge potter og luften ved siden af |
| Lys (senere) | AS7341 (lyssensor med flere farvekanaler) | PAR (µmol/m²/s; det lys planten kan bruge) og spektrum |

**Hvorfor to vægte på ét board:** potterne står i samme lys og luft, så
forskellen mellem dem skyldes planten — samme dag, samme vejr.

**Forventet præcision:** opløsning ca. 0,1–0,5 g; en stueplante taber typisk
20–100 g/døgn. Billige vejeceller kan drive nogle gram over døgnet (krybning,
temperatur) → mål afdriften med en sten i et døgn først. Afdriftstesten
afgør også, om HX711 (den billige vægtforstærker) er støjsvag nok, eller om
man skal opgradere til NAU7802 (nyere, mere støjsvag vægtforstærker).

**DS18B20 (temperaturføler) mod luft:** proben sidder i en metalspids og reagerer langsomt
(et minut eller to) — god til jord og vand, mindre god til hurtige
luftændringer. Dem måler SHT40 (fugtsensor) bedre.

**PAR med AS7341 (lyssensor):** sensoren er ikke en kvantesensor (en rigtig PAR-måler);
PAR beregnes fra 8 farvekanaler (415–685 nm) og skal kalibreres mod en rigtig
PAR-måler (Vernier/Apogee — lån evt.). Kalibreringen afhænger af lyskilden
(sol gennem rude ≠ LED-plantelys).

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
7. **Jordtemperatur** (med temperaturboardet) — hvordan potternes
   temperatur følger døgnet, og om den påvirker vandtabet.

Beregninger eleverne laver: vandtab (g/h), transpiration pr. bladareal,
VPD ud fra temperatur og RF (Magnus-formlen), vandindhold (vægt-%).

### Trin

1. Breadboard (stikbræt uden lodning): SHT40 (fugtsensor) + OLED (skærm) virker.
2. Én vægt (HX711 (vægtforstærker) + celle) kalibreret med kendte lodder; afdriftstest.
3. Begge vægte (HX711 (vægtforstærker) på hver sine to ben); SD-logning med tid fra DS3231 (ur).
4. Jordfugt; temperaturboard med 3 × DS18B20 (temperaturføler); printet kabinet og
   vægtplatforme.
5. AS7341 (lyssensor) og kalibrering mod en lånt PAR-måler — først når 1–4 virker.
6. smbi.dk-side: forbind via USB/Bluetooth, live-graf, hent log, eksport
   (også dansk Excel-venlig CSV: semikolon og decimalkomma).
7. Første rigtige forsøg + vejledning til `smbi/Øvelser`.

Hvis afdriftstesten i trin 2 viser for meget støj med HX711 (vægtforstærker), skiftes til
NAU7802 (vægtforstærker) med TCA9548A (omskifter, der lader to ens sensorer dele I²C-ledninger).
Se opgraderingen i [`indkob.md`](indkob.md).

## Fase 2: Geostationer

To ens udendørs stationer til naturgeografi:
SHT40 (fugtsensor; under printet strålingsskjold, så solen ikke varmer sensoren op),
BMP280 (lufttrykssensor), 3 × DS18B20 (temperaturprober; kan genbruge
temperaturboardet fra fase 1), 2 solceller + ADS1115 (præcis
analog-til-digital-omsætter, der måler solcellernes strøm: indstråling og
albedo), SD, ur, OLED (skærm), LiPo (genopladeligt batteri).

Forsøg: jordens varmebølge (prober i 0–40 cm), albedo, energibalance,
byens varmeø, mikroklima, lagdeling i sø, vejrfronter, højde og tryk,
dugpunkt og tåge.

**Indstråling med solcelle:** mål kortslutningsstrømmen (lille shunt,
1–5 Ω) — ikke tomgangsspændingen, som er logaritmisk. Kalibrér mod skolens
W/m²-måler. Spektralt begrænset (Si: 400–1100 nm), ±5–10 %.

## Fase 3 og videre

* Noder med ESP-NOW (trådløs kommunikation mellem ESP32-bræt uden router)
  til en fælles central (én ESP32 pr. målested).
* LoRa (langtrækkende radio med lavt strømforbrug) og Raspberry Pi-gateway
  (lille computer, der samler data) til fjerne stationer.
* Modulstandard (stik, EEPROM-ID), robuste kabinetter, solcelle.
* Undervisningsmateriale og byggesæt.

## Beslutninger

| Emne | Valg | Begrundelse |
|---|---|---|
| Board | FireBeetle 2 ESP32-C6 (mikrocontrollerbræt) | Fås i Danmark (74 kr ex moms, 62 kr ved 6 stk.), batteristik + lader, solcelleindgang, batterimåling. C3 (en lidt enklere variant af samme chipfamilie) ville også kunne — boardet koster det samme. Fase 1 bruger to: plantevægten og temperaturboardet. |
| Indkøb | Danske butikker | Ægte dele, 1–3 dages levering, faktura. AliExpress er 3–5× billigere på moduler, men med told (3 €/varetype fra 1/7-2026) og risiko for kopier er besparelsen lille ved små ordrer. Relevant senere til klassesæt. |
| Budget | Slank liste først, ca. 800 kr plus ca. 175 kr til temperaturboardet (ex moms) | Det dyre springes over, til det er nødvendigt: AS7341 (lyssensor) venter til trin 5, og NAU7802 (vægtforstærker) kommer kun med, hvis afdriftstesten kræver det. Se [`indkob.md`](indkob.md). |
| Vægtforstærker | HX711 (billig vægtforstærker); NAU7802 (dyrere, mere støjsvag) som opgradering | HX711-chippen (vægtforstærker) kører på 2,6–5,5 V og virker normalt på 3,3 V, koster ca. en tiendedel og kræver ingen omskifter. NAU7802 (vægtforstærker) har mindre støj og læser hurtigere (80 mod 10 målinger i sekundet) — men kun afdriftstesten viser, om det er nødvendigt. Tjek, at det indkøbte HX711-modul kan køre på 3,3 V. |
| Lys | PAR (lyssensor AS7341) og W/m² (solcelle) — ikke lux | Lux er vægtet efter øjet; omregning til W/m² (≈ lux/110) er usikker, især under trækroner. AS7341 (lyssensor) udsættes til trin 5. |
| Til smbi.dk | Web Serial (USB) og Web Bluetooth | Live-graf uden app. Chrome/Edge på Windows, Mac, Chromebook, Android. iPhone kun via Bluefy. |
| Tid | DS3231 (ur med batteri), gemt i UTC | Undgår sommertidsfejl. |
| Kabinetter | 3D-print på skolen | PETG/ASA udendørs (PLA bliver blød ~55 °C); hvidt strålingsskjold. |
| Firmware | Arduino-rammen via PlatformIO | Modne biblioteker til alle sensorer. |

## Dataformat (udkast)

CSV, én række pr. tidspunkt, enheder i kolonnenavne:

```
tid_utc,masse_A_g,masse_B_g,luft_temp_C,rf_pct,par_umol,jordfugt_A,jordfugt_B
2026-12-10T09:00:00Z,1834.6,1702.1,21.4,38.2,,0.62,0.58
```

`par_umol` er tom, indtil AS7341 (lyssensor) er monteret (trin 5).

Temperaturboardet logger i egen fil med samme tidsformat:

```
tid_utc,temp_1_C,temp_2_C,temp_3_C
2026-12-10T09:00:00Z,20.6,20.9,21.3
```

Metadata (sensorer, kalibrering, interval, hvad der står i potte A og B, og
hvor de tre prober sidder) i en lille fil ved siden af.

## Inspiration

senseBox (Münster), DFRobot SCI DAQ, Bäumker m.fl. 2021 (PAR fra AS7341 (lyssensor),
*Sensors* 21:3390).
