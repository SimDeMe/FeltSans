# FeltSans

Et åbent, modulært sensorsystem til naturvidenskab og feltarbejde — billigt,
fleksibelt og lavet til undervisning i biologi og naturgeografi.

**Status:** Fase 1 — komponenter købes ind (november 2026).

## Idéen i én figur

```
 Node "Skov"      Node "Græs"     Node "Sø"
 (SHT40, lys,     (SHT40, lys,    (3 × DS18B20
  jordfugt)        jordfugt)       i dybder)
      \               |              /
       \---------- ESP-NOW ---------/
                      |
              Central (hub)
        skærm · SD-kort · ur
                      |
         Bluetooth/USB → smbi.dk (live-graf)
```

* **Én ESP32 pr. målested** (ikke pr. sensor). Noderne måler, sender og sover.
* **Centralen** sætter tidsstempler, logger alt på SD-kort, viser oversigt på
  skærmen og sender videre til en side på smbi.dk.
* **ESP-NOW** kræver hverken router eller internet og rækker typisk 50–200 m.
* Senere kan LoRa erstatte ESP-NOW og en Raspberry Pi blive central, uden at
  noderne skal laves om.

## Dokumenter

* [`docs/indkob.md`](docs/indkob.md) — indkøbsliste til fase 1 (budget 2.000 kr)
* [`docs/plan.md`](docs/plan.md) — udviklingsplan og designbeslutninger
* [`docs/vision.md`](docs/vision.md) — den lange vision for FeltSans

## Platform

DFRobot **FireBeetle 2 ESP32-C6** til både central og noder: batteristik
(PH2.0) med lader, solcelleindgang, batterispændingsmåling og ca. 36 µA i
dyb søvn.

## Licens

Endnu ikke valgt. Forslag: CERN-OHL-S (hardware), MIT (firmware),
CC BY 4.0 (dokumentation og undervisningsmateriale).
