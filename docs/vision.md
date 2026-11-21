# FeltSans — vision

*Et åbent, modulært sensorsystem til naturvidenskab og feltarbejde.*

Dette er den oprindelige, langsigtede plan. Den konkrete udvikling og de
beslutninger, der er truffet undervejs, står i [`plan.md`](plan.md).

FeltSans skal gøre det nemt, billigt og fleksibelt at indsamle
naturvidenskabelige måledata — i felten, i laboratoriet og i undervisningen.
Systemet bygger på små mikrocontrollere, udskiftelige sensorer og open
source-software. Målet er et sammenhængende økosystem, hvor brugeren kan
sammensætte sit eget måleinstrument, registrere data og analysere
resultaterne.

FeltSans udvikles med særligt fokus på undervisning i biologi, naturgeografi
og andre naturvidenskabelige fag, men skal også kunne bruges af hobbyfolk og
naturinteresserede.

## 1. Grundidé

Systemet skal være:

* **Modulært:** Sensorer kan tilføjes, fjernes og udskiftes.
* **Brugervenligt:** Målinger uden programmering.
* **Prisvenligt:** Billige, almindeligt tilgængelige komponenter.
* **Åbent:** Hardware, firmware, dataformater og dokumentation er åbne.
* **Udvideligt:** Nye sensorer og funktioner uden at ændre hele systemet.
* **Undervisningsegnet:** Robust, transportabelt og nemt for elever.

## 2. Måleenheden

En kompakt enhed baseret på ESP32 med display, knapper, microSD-logning,
realtidsur, genopladeligt batteri og 3D-printet kabinet. Den skal fungere
selvstændigt uden computer eller internet. Sensorer forbindes via kabler, så
de kan placeres i forskellige omgivelser.

## 3. Sensormoduler

Første ønsker: klima (temperatur, luftfugtighed), lufttryk, lys, jord
(fugtighed og temperatur), vand (temperatur) og position (GPS).

Senere: CO₂, pH og ledningsevne, UV, vind, nedbør, partikler,
solindstråling, afstand og bevægelse.

**Automatisk genkendelse:** Systemet skal så vidt muligt selv registrere,
hvilken sensor der er tilsluttet. Det kræver en fælles standard for stik og
identifikation (fx en lille EEPROM med modul-ID og kalibrering på hvert
modul).

## 4. Dataopsamling

Selvstændig datalogning med valgfrit måleinterval, lokalt på microSD i et
åbent format (CSV). Et fælles dataformat, så målinger fra forskellige
sensorer og enheder kan kombineres. Fungerer offline.

## 5. FeltSans Web på smbi.dk

Browserbaseret analyse uden login: import af CSV, tidsserier, sammenligning
af datasæt, valg af akser, middel/min/maks, eksport, kort over
GPS-registrerede målinger. Direkte overførsel via USB eller trådløst til
livegrafer. På sigt deling af datasæt mellem elever og lærere.

## 6. Trådløs kommunikation og fjernmåling

Fjernplacerede stationer, fx i en skov, der sender til skolen. Teknologier:
ESP-NOW (radio) og Wi-Fi (korte afstande), LoRa (langdistanceradio, flere km uden abonnement),
mobilnet, Raspberry Pi som gateway.

```
FeltSans-station → LoRa → Raspberry Pi-gateway → internet → FeltSans Web
```

## 7. Undervisning

* **Mikroklima:** skov, græsplæne og bymiljø sammenlignet.
* **Vejr og klima:** døgnvariation og vejrskift over flere døgn.
* **Jordbund og planter:** jordfugtighed, lys og temperatur.
* **Vandmiljø:** temperatur i søer, vandløb og kyst.
* **Energi og stråling:** lys, indstråling og opvarmning af overflader.
* **Vertikale målinger:** evt. sensorpakke på ballon (kræver vurdering af
  vægt, sikkerhed og luftfartsregler).

Eleverne skal kunne koncentrere sig om naturvidenskaben, men også kunne
undersøge, hvordan instrumenterne virker.

## 8. Åben hardware og byggesæt

Repoet skal på sigt rumme firmware, diagrammer, 3D-modeller, dokumentation
af moduler, samlevejledninger, eksempeldata, undervisningsmateriale og kode
til analyseplatformen. Mulige byggesæt, helst uden lodning — salg kræver
vurdering af produktsikkerhed, CE og regler for radioudstyr.

## 9. Langsigtet vision

En elev tager en FeltSans-enhed med i skoven, måler mikroklimaet, overfører
data til smbi.dk og analyserer resultaterne. En lærer opstiller flere
målestationer omkring skolen og sammenligner i realtid. En hobbybruger
bygger sin egen vejrstation. En udvikler bidrager med software, hardware
eller nye anvendelser.

*FeltSans — mærk naturen. Mål verden. Forstå sammenhængene.*
