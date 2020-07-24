<p align="center">
<img src="https://raw.githubusercontent.com/freight-trust/branding/90665e6efb31c1e22638937d083befeb9fd7fcc2/images/bundle/github_repo_card.svg">
</p>
<br>
<!-- Badges Start -->
<p align="center">
<img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/freighttrustnet?label=%40FreightTrustNet&style=social">
<img alt="Open Source License" src="https://img.shields.io/github/license/freight-trust/releases?style=social">
<!-- Badges End -->

# Burgess

Jurisdictional Regimes and Agreement Configurator

## Configuration

### Tariff Control Events

partyB && partyC

```json
["partyB_tarrif_event_upon_crossing", "partyC_tarrif_event_upon_crossing"]
```

### General Terms
relationship_with_the_control_agreement: should always be the `carrier` <br />
execution_language: should pertain the `carrier` langauge that they use<br />
date_of_tradedocs_master_agreement: joining agreement date<br />
execution_date: date upon they have entered into <br />

delivery_in_lieu_right: <br />
	- right: applicable<br />
	- volumetric: embeded<br />

umbrella_agreement_and_principal_identification<br />
	- urn:freight-trust:rulebook:authority // `refers to the omnibus/rulebook, where authority = active version`<br />

resolution_time:<br />
	- date:time upon which delivery must be completed, otherwise it enters `detention` period<br />

conveyance_asset:<br />

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

## License

Mozilla Public License 2.0
