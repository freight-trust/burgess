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

Jurisdictional Regimes and Agreement Configuraton

formally `jEDI` - a de facto protocol for business transactions (yes we had to change the name, if you see jEDI refrenced, its talking about `burgess`.

- [Burgess](#burgess)
- [Burgess](#burgess-1)
  * [Abstract](#abstract)
  * [TL;DR](#tl-dr)
  * [Primitives](#primitives)
    + [ExecutionPrimitive](#executionprimitive)
    + [ContractFormationPrimitive](#contractformationprimitive)
    + [AllocationPrimitive](#allocationprimitive)
    + [Primitive Example](#primitive-example)
    + [ExercisePrimitive](#exerciseprimitive)
    + [Inception Primitive](#inception-primitive)
    + [Obervation Primitive](#obervation-primitive)
    + [Quantity Change Primitive](#quantity-change-primitive)
    + [Reset  Primitive](#reset--primitive)
    + [Terms Change Primitive](#terms-change-primitive)
    + [Transfer Primitive](#transfer-primitive)
  * [Workflow for Contractual Product (e.g. Forward Contracts)](#workflow-for-contractual-product--eg-forward-contracts-)
  * [Solidity Contract Specification](#solidity-contract-specification)
  * [Configuration](#configuration)
    + [Tariff Control Events](#tariff-control-events)
    + [General Terms](#general-terms)
  * [Examples](#examples)
  * [License](#license)

## Abstract
We introduce a new specification for smart contract integration that requires only a certificate to enable transactions with a certificate issued by an authorized operator. We provide a list of primitives so that smart contract mechanics can be formalized (to an extent) in relations to legal mechanics in order to replicate a "de facto" contracting system (i.e. jursidictional regimes) within the context of blockchain based smart contracts. 

## TL;DR
We want to make smart contracts legal in the sense that the replicate all the exact workings of existing legal contracts (i.e. paper based legal contracts). We call these "primitives". 

You can read this article here detailing the concept
[Applications of traditional contract principles to Smart Contracts](https://medium.com/freighttrust/application-of-traditional-contract-principles-to-smart-contracts-64cad84091bf)

We then enable existing EDI transactions the ability to interact (i.e. create, move, delegate, transfer, etc) with these smart contracts, removing the issues of "legality and legal bindingness" of using a smart contract protocol versus traditional paper based documents, removing the need for "hash stored" protocol implematation [see protocolv1](https://github.com/freight-chain/protocol)




## Primitives

Defined ordered list 

`PrimitiveEvent`
`ExercisePrimitive`
`AllocationPrimitive`
`ContractFormationPrimitive`
`ExecutionPrimitive`
`InceptionPrimitive`
`ObservationPrimitive`
`QuantityChangePrimitive`
`ResetPrimitive`
`TermsChangePrimitive`
`TransferPrimitive`




### ExecutionPrimitive

Specification of the primitive event for an execution, with 'after' state being an ExecutionState and the 'before' state being Null.

The 'before' ExecutionState (0..0) The 0 cardinality reflects the fact that there is no execution in the before state of an execution primitive. Think of this as the "genesis" point.

after `ExecutionState`

The after state corresponds to the execution between the parties. In the case of an execution on a contractual product, some additional characteristics may need to be specified to get a fully-formed contract, for instance when the executing party acts as an agent, as is the case in an allocation scenario. This is the purpose of the `ContractFormation` primitive event.


### ContractFormationPrimitive

> **@dev** this design pattern is differnet in that it does not bundle together execution of the contract and formation of the contract, similar to the way a `proxy` contract works. The design pattern for this is to better provide for atomic primitites. Our pruposes do not need such atomiticy, as they are contractual products.


`ExecutionPrimitive` + `ContractFormationPrimitive` = atomic financial primitive

`InceptionPrimitive` = contractual product primitive [see jEDI contract below]


Specification of the primitive event for the formation of a contract, with 'before' state being an 'ExecutionState' and 'after' state being a 'PostInceptionState'.


### AllocationPrimitive
The primitive event to represent a split/allocation of a trade. As part of this primitive event the type of trade, either an execution or a contract, does not get altered. In the case of an execution, the further transformation of each split execution into a contract will be the purpose of the ContractFormation primitive.

### Primitive Example

```markdown
if AllocationPrimitive exists and before -> execution exists
	then after -> originalTrade -> execution exists
	and after -> allocatedTrade -> execution exists
	and after -> allocatedTrade -> contract is absent
	
condition ContractType: 
if AllocationPrimitive exists and before -> contract exists
	then after -> originalTrade -> contract exists
	and after -> allocatedTrade -> contract exists
	and after -> allocatedTrade -> execution is absent
```

### ExercisePrimitive

`exerciseTiming` which is deemed as associated to a request for exercise that is meant to take place, as opposed to the actual exercise event. Similar to how in FpML an OptionExercise is constructed. [FpML 5.5](https://www.fpml.org/spec/fpml-5-6-5-rec-1/html/confirmation/fpml-5-6-intro-8.html)


### Inception Primitive
The primitive event for the inception of a new contract between parties. 

### Obervation Primitive 
A class to specify the primitive object to specify market observation events, which is applicable across all asset classes.

### Quantity Change Primitive 
The primitive event to represent a change in quantity or notional.

### Reset  Primitive
The primitive event to represent a reset.

### Terms Change Primitive
The primitive event to represent change(s) to the contractual terms and the clearing submission and acceptance process.

### Transfer Primitive 
A class to specify the transfer of assets between parties, those assets being either cash, securities or physical assets (such as freight or an asset held by a bailee relationship such as warehouse receipts). This class combines components that are cross-assets (settlement date, settlement type, status, ...) and some other which are specialized by asset class (e.g. the payer/receiver amount and cashflow type for a cash transfer, the transferor/transferee, security indication, quantity, and asset transfer type qualification for the case of a security).


## Workflow for Contractual Product (e.g. Forward Contracts)

```javascript
if WorkflowStep -> businessEvent -> primitives -> inception -> after -> contract only exists
	then WorkflowStep -> businessEvent -> primitives -> inception -> after -> contract

else if WorkflowStep -> businessEvent -> primitives -> quantityChange -> after -> contract  exists
		
	then WorkflowStep -> businessEvent -> primitives -> quantityChange -> after -> contract
	else WorkflowStep -> businessEvent -> primitives -> inception -> after -> contract
 	as "Contract"
```


## Solidity Contract Specification
[version 1.4, 2020.05.01](https://github.com/freight-trust/jedi)


> This has not gone through auditing but is working

> Instruments generally are capable with ERC, but not exactly
> Primitves resemble partitioned semi-fungible token implementations 

```javascript=

/// @title EDI based Transactions using Solidity
/// @license this is BSD-3 Clause
/// @author FreightTrust and Clearing Corporation All Rights Reserved
// @version 1.4.0
/// @dev jEDI stands for "jointed" edi transactions, through our parser we are able to do this, code is for public ref. 

interface jEDI { 


function getEDI(bytes32 _name) external view returns (string, bytes32, uint256);

function setEDI(bytes32 _name, string _uri, bytes32 _EDIHash) external;

function amendEDI(bytes32 _name) external;

function getAllEDITransactions() view returns (bytes32[]);

/// emit events based upon EDI transactions 

event EDITransaction(bytes32 indexed _name, string _uri, bytes32 _ediHash);
event EDIUpdated(bytes32 indexed _name, string _uri, bytes32 _ediHash);
event EDIAmended(bytes32 indexed _name, string _uri, bytes32 _ediHash);

/// control contracts using EDI spec. 

function transferWithEDI(address _to, uint256 _value, bytes _data) external;
function transferFromWithEDI(address _from, address _to, uint256 _value, bytes _data) external;


/// EDI Events

event EDITransaction(bytes32 indexed _name, string _uri, bytes32 _ediHash);
event EDIUpdated(bytes32 indexed _name, string _uri, bytes32 _ediHash);
event EDIAmended(bytes32 indexed _name, string _uri, bytes32 _ediHash);

function transferWithEDI(address _to, uint256 _value, bytes _data) external;
function transferFromWithEDI(address _from, address _to, uint256 _value, bytes _data) external;

/// Instrument for the most part is interchangeable with ERC-20 fungability in concept
/// function transferByInstrument(bytes32 _instrument, address _to, uint256 _value, bytes _data) external returns (bytes32);

function transferWithEDI(address _to, uint256 _value, bytes _data) external;
function transferFromWithEDI(address _from, address _to, uint256 _value, bytes _data) external;

}
```


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
