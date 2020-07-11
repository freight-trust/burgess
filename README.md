# Burgess

Jurisdictional Regimes and Agreement Configurator

## Configuration

### Tariff Control Events

partyB && partyC

```json
["partyB_tarrif_event_upon_crossing", "partyC_tarrif_event_upon_crossing"]
```

### General Terms

relationship_with_the_control_agreement: should always be the `carrier`
execution_language: should pertain the `carrier` langauge that they use
date_of_tradedocs_master_agreement: joining agreement date execution_date: date
upon they have entered into

delivery_in_lieu_right: - right: applicable - volumetric: embeded

umbrella_agreement_and_principal_identification -
urn:freight-trust:rulebook:authority //
`refers to the omnibus/rulebook, where authority = active version`

resolution_time: - date:time upon which delivery must be completed, otherwise it
enters `detention` period

conveyance_asset:

```json
	valuation_of_appropriated_collateral: {
        "specified": "false"
      }
```

## Examples

```json
  "partyB": {
    "entity": {
      "id": "967600BJ1XXIQR6G3948",
      "name": "ABC LOGISTICS",
      "lei": "967600BJ1XXIQR6G3948"
    }
```
