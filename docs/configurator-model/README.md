# FMx6 — every parameter in Teltonika Configurator 1.8.26

Extracted from the embedded parameter model (plain-text XML) in
`Teltonika.Configurator_1.8.26_B.FMX6_R.194.exe`. The model is one contiguous XML region
(about bytes 28.3M–31.9M) of the executable. The firmware (`*.e.xim`) is encrypted and is not
part of this — everything here comes from the Configurator.

## Files

| File | Contents |
|------|----------|
| `parameters_all.tsv` | **3,553 unique configuration parameters** (ParameterId), one row per id. |
| `parameters_variants_raw.tsv` | All **6,552** definitions raw — the same id appears several times because the model carries version / hardware differences. |
| `io_elements_all.tsv` | **2,448 unique I/O elements** (IoId) — AVL signals, including the whole CAN adapter list. |
| `FMX6_CAN_adapter_parameters.md` | The CAN adapter part (LV-CAN200 / ALL-CAN300), with a verification table against an FMC650. |
| `FMX6_parameters.xlsx` | The same tables as a workbook. |
| `_verification.json` | Raw verification output (below). |

The **Variants** column in `parameters_all.tsv` = how many different definitions the same id has
across firmware versions / hardware. 1 = unambiguous; >1 = changed between versions (e.g. range or
default). 890 ids have several variants.

## Verification (that everything was captured)

Method: count raw occurrences in the bytes and reconcile against what was parsed, with no gaps.

```
6590 raw "ParameterId="  =  6552 genuine definitions
                          +   19 <If …>   (validation rules, references – not parameters)
                          +   19 ForParameterId=  (rule reference, a substring of "ParameterId=")
reconciles: true   (6552 + 19 + 19 = 6590)

IoId:  3500 raw  =  3500 parsed  →  2448 unique
Integrity problems: 0   (no id out of range, no broken label / markup)
```

Spot checks confirm that "variants" are real version differences, not parse errors, e.g.:

- **45002 "Program number"** (the adapter's vehicle code): Max = 9999 / 59999 / 65535 / 99999 and
  default 0 / 11120 depending on the firmware version.
- **100 Movement source**, **101 Ignition source**, **1000 Open link timeout**: the same parameter,
  small differences in default / label between versions.

## Caveats

`io_elements_all.tsv` keeps one name per IoId (3,500 raw → 2,448 unique): a name can belong to a
firmware family other than FMx6. The *Section* column is unreliable (CAN elements appear under
"Auto Geofence" and the like). What has been checked on an FMC650 is at the top of
`FMX6_CAN_adapter_parameters.md` — there the 454xx / 460xx ids differ from the model.

## Distribution

- Control types: NumberInput 1375, Collection 770, DropDownList 635, TextBox 533, TextBoxMasked 130, …
- Largest sections (unique parameters): Network Settings 713, Manual CAN Requests 365, MODBUS 230,
  dataacquisition 173, CAN Adapter 64, SMS Events 64 …
