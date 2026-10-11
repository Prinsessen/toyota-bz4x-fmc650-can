# FMX65 — alle parametre fra Teltonika Configurator 1.8.26

Udtrukket fra den indlejrede parameter-model (ren-tekst XML) i
`Teltonika.Configurator_1.8.26_B.FMX6_R.194.exe`.
Modellen ligger som ét sammenhængende XML-område (ca. byte 28.3M–31.9M) i .exe'en.
Firmwaren (`*.e.xim`) er krypteret og indgår ikke — alt herunder kommer fra configuratoren.

## Filer

| Fil | Indhold |
|-----|---------|
| `parametre_alle.tsv` | **3.553 unikke konfigurations-parametre** (ParameterId). Én række pr. ID. |
| `parametre_variants_raw.tsv` | Alle **6.552** definitioner rå — samme ID optræder flere gange, fordi modellen har versions-/hardware-diffs. |
| `io_elementer_alle.tsv` | **2.448 unikke I/O-elementer** (IoId) — AVL-signaler, inkl. hele CAN-adapter-listen. |
| `FMX65_CAN_adapter_parametre.md` | Fokuseret udtræk af CAN-adapter-delen (LV-CAN200 / ALL-CAN300). |
| `_verification.json` | Rå verifikations-output (se nedenfor). |

Kolonnen **Varianter** i `parametre_alle.tsv` = hvor mange forskellige definitioner
samme ID har på tværs af firmware-versioner/hardware. 1 = entydig; >1 = ændret
mellem versioner (fx range eller default). 890 ID'er har flere varianter.

## Verifikation (at alle er korrekte)

Metoden: tæl rå forekomster i bytes og afstem mod det parsede, uden huller.

```
6590 rå "ParameterId="  =  6552 ægte definitioner
                         +    19 <If …>   (valideringsregler, referencer – ikke parametre)
                         +    19 ForParameterId=  (regel-reference, delstreng af "ParameterId=")
reconciles: true   (6552 + 19 + 19 = 6590)

IoId:  3500 rå  =  3500 parsede  →  2448 unikke
Integritetsproblemer: 0   (ingen ID uden for interval, ingen ødelagt label/markup)
```

Stikprøver bekræfter at "varianter" er ægte versions-forskelle, ikke parse-fejl, fx:

- **45002 "Program number"** (CAN-adapterens bil-kode): Max = 9999 / 59999 / 65535 / 99999
  og default 0 / 11120 afhængigt af firmware-version.
- **100 Movement source**, **101 Ignition source**, **1000 Open link timeout**:
  samme parameter, små forskelle i default/label mellem versioner.

## Forbehold

`io_elementer_alle.tsv` beholder ét navn pr. IoId (3 500 rå -> 2 448 unikke): et navn kan høre til en anden
firmware-familie end FMx6. Kolonnen *Section* er upålidelig (CAN-elementer står under "Auto Geofence" m.m.).
Det, der er efterprøvet på en FMC650, står øverst i `FMX65_CAN_adapter_parametre.md` - dér afviger
454xx/460xx fra modellen.

## Fordeling

- Kontroltyper: NumberInput 1375, Collection 770, DropDownList 635, TextBox 533,
  TextBoxMasked 130, m.fl.
- Største sektioner (unikke params): Network Settings 713, Manual CAN Requests 365,
  MODBUS 230, dataacquisition 173, CAN Adapter 64, SMS Events 64 …
