# Customer

> [!NOTE]
> This common entity model is not a message contract; message-specific schemas must define the rules of each interaction.

## Property documentation, validation rules.

### `id`
We use a UUID since the ownership of the customer entity will change in future, and in the transition phase multiple systems will provide customers (namely BC and HsM), therfore a UUID is favored over a simple sequential integer IDs. Additionally BC already has a UUID for each customer (and for each tenant).

### crmCustomerNumber

### `erpCustomerNumber`
* The customer number is optional
* Customer number is unique per reseller only
* Customer number might be defined by the reseller (i.e. not only by Hagleitner Tenants)

### `erpTenantId`
Given to make the life of the EPR easier, i.e. an erp attribute stored in the BDIM.

The CRM must implement a logic that determines the correct tenat. Could be the 1st level reseller on the reseller relationship path.

### `name1`, `name2`, `name3`
Name of the customer, split in up to 3 parts for printing postal addresses.

#### TODO
* (CRM) Decide if the owner shall really not provide a concatenated name.

### `phoneNumber1`, `phoneNumber2`
Phone numbers of the customer
CRM must enforce valid phone numbers (google libphonenumber).

#### TODOS
* (CRM) Reevaluate this model with CRM-experts. The current implementation is jsut 2 semantic-less numbers, alternative: Array of phone numbers objects, with type/tag.

### `emailAddresses{}`
We use an object containing multiple email address properties, each of them having a defined semantic.

(CRM) There is a specific logic in the ERP system when to use what email address and also what to fallback to, in case a needed address is not filled.

### `lifeCycleStatus`

#### TODO
* (CRM) Some validation rules depend on the lifeCycleStatus, e.g. a unique customerNumber is only unique within active customers
* (CRM) Changing from deleted to active is not always possible, e.g. uniqueness constraints must be checked
* (CRM) What life-cycle status do we realy have
* (CRM) What happens to customer relations if the ancestor is marked as deleted

### `languageTag`
While this is the BCP47 language tag, it is also used to derive the locale or culture with formatting conventions. 

### `timezone`

Only canonical IANA timeZones are allowed

### `avatar`
A URL that points to the avatar image resource.

#### TODO
* Probably skip for version 0.1, but discuss with Orbis if this is something they can theortically provide, or if we anyway have to implement this in another sytem
* How to protect? SAS token, Rest Endpoint?
* Specify image format, auotmatic resizing, etc.

### `taxIdentificationNumber`
Rationale:
* VAT is Europe only

#### TODO
* (CRM) Validation must be done in code based on country code. 

### `roles[]`
Instead of using individual flags like `isReseller`, roles provide a more abstract and extensible model for associating customers with one or more roles. Roles might also have role-spcific data models, which—if necessary—are modelled as separate entities.

Candidates:

* Reseller

##### Role changes
* Reseller role cannot be removed the customer has linked sub-customers

#### TODOS
* (CRM) Define the actual roles
* (CRM) Discuss using roles as the necessary flag to allow adding descendants for specific customer relationship types

### `customerRelations`

#### TODOS
Customer relations between customers under different resellers need a more deatilled discussion.

#### `reseller`
* Can only point to customers, that have the `reseller` role
* Must not be null except for one customer (the root customer)
* Must not form loops
* Must not be modified after creation

#### `createdAt`
This is the max  date and time when any of the properties of this entity was created

#### `updatedAt`
This is the max  date and time when any of the properties of this entity was last changed

#### `contentHash`
Hash of the customer content for change detection.


## Properties not yet in the schema
### `updatedBy`
Would be string only, therfore troublesome with GPDR

### `customerCode`
E.g. HHAT. A code uniquely identifying a customer. 

#### TODO
* (CRM) Is this needed
* May also be limited to resellers only, but then would be part of the customer/reseller profile
* May be a uniqueCustomerAlias, theoretically available for all customers
* May be a problem with delete/undelete

### `block` (not yet part of model)
* ERP: status = Freigabestatus: Is this needed any where?
* ERP: blockedInERP: 0: not blocked, 1: blocked for delivery but invoicing still possible, 2: blocked for Invoicing (), 3: completely blocked
* ERP:BlockreasonCode
* ERP: requestForBlockERP (block pending)
* Who defines "CanUseDigitalServices", "CanOrder", ...
#### TODOS:
* Exact definition of block 
* What happens if an ancestor is blocked (each realtionship)
* What happens if the side-effects of blocking affects multiple customers, and if these side-effects are calculated in ERP

* `customerDisplayName` Preferred human-readable customer name for display in user interfaces; may be derived from name1, name2, and name3 if not explicitly maintained.

* `IsLead` or some kind of customer classification would make sense for other tools that only want to work with "active" customers. If any system other than the CRM, wants to work with leads, then this classification is necessary.

## General questions and todos

* Bi-directional synchronization of customer data in Phase 1
* The `customer` in our model is a very generic entity. Spezializations of a customer might need additional properties. In that case we use role profiles e.g. `customerResellerProfile` to communicate additional properties for that role.
* Email addresses as specific email properties: pro: explicit, con: new types need contract change (but as they convey meaning, they anyway have to.)
* `geolocationQuality`: Probably not relevant in the CRM, keep it in the FLS if possible (like name4 in hsm)
* `responsibilityCenter` could be modelled as a customer with a responsibilityCenter role, plus a customer relationship.




## Sequence
* Customer creation shall be done without any relations other than the reseller relationship, all other relationships are created using a separate entity `customerRelation`
* Traverse customers recursively with a "preorder depth-first traversal" strategy. 
* On each depth-level sort customers by [Name1, Name2, Name3, id]

# Aditional fields
* homepage: URI
* currencyCode: ISO 4217, default = EUR
* shippingInstruction: Volllieferung Teillieferung (noch unklar) enum
* 