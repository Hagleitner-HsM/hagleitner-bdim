# Customer

## AMQP Message types
### Events
* customer.created 
* customer.updated

### Commands
* customer.update
* customer.setstatus

## Property documentation, validation rules.

### `id`
We use a UUID since the ownership of the customer entity will change in future, and in the transition phase multiple systems will provide customers (namely BC and HsM), therfore a UUID is favored over a simple sequential integer IDs. Additionally BC already has a UUID for each customer (and for each tenant).

### `erpId`
This is an ID that is only useful in the context of the ERP, but be still store it in the CRM,
since it easens the communication. E.g. if we have multiple customer with similar name and address, this ID is used in internal communications.

#### TODOS
* Define exact regex for the erpId
* Introduce erpId as basetype schema

### `name1`, `name2`, `name3`

### `phoneNumber1`, `phoneNumber2`
#### TODOS
* Reevaluate this model with CRM-experts

### `emailAddresses{}`
We use an object containing multiple email address properties, each of them having a defined semantic.

#### TODO
* Isn't this more ERP, should we not just have the primary-email address in CRM?

### `roles[]`
Instead of using ìndividual flags like `isReseller`, roles provide a more abstract and extensible model for
associating customers with one or more roles. Roles might also have role-spcific data models, which—if necessary—are modelled as separate entities.

#### TODOS
* Define the actual roles
* Discuss using roles as the necessary flag to allow adding descendants for specific customer relationship types

### block
* ERP: status = Freigabestatus
* ERP: blockedInERP: 0: not blocked, 1: blocked for delivery but invoiding still possible, 2: blocked for Invoicing (), 3: completely blocked
* ERP:BlockreasonCode
* ERP: requestForBlockERP (Sperranfrage pending)
#### TODOS:
* Exact definition of block 


### Customer relations

#### Reseller relationship
* Can only point to customers, that have the `reseller` role
* Must not be null except for one customer (the root customer)
* Must not form loops
* Must not be modified after creation

### Role changes
* Reseller role cannot be removed the customer has linked sub-customers

## Things to discuss
* `status`: what is an active custoemer, what is inactive, how do we handle deleted.
* `deleted`
* `blocking`: digitaleService, WebShop, ...
* `phoneNumber*`: If more than 2 are required, then we need to discuss the data exchange with the ERP system.
* `customerDisplayName` Preferred human-readable customer name for display in user interfaces; may be derived from name1, name2, and name3 if not explicitly maintained.
* `IsLead` or some kind of customer classification would make sense for other tools that only want to work with "active" customers.
* The `customer` in our model is a very generic entity. Spezializations of a customer might need additional properties. In that case we use role profiles e.g. `customerResellerProfile` to communicate additional properties for that role.
* Email addresses as specific email properties: pro: explicit, con: new types need contract change (but as they convey meaning, they anyway have to.)
* `resellerCode` - unique code identifying a customer like HHAT. Coud be part of the `customerResellerProfile` 
* `isReseller` flag, same pattern for all other relationships?
* `address.latitude`, `address.longitude`: seems widely accepted that you shoud use geographic system with lat, long not a projected.
* `geolocationQuality`: Probably not relevant in the CRM, keep it in the FLS if possible (like name4 in hsm)
* `responsibilityCenter` could be modelled as a customer with a responsibilityCenter role, plus a customer relationship
* `customer.status` separate from deleted flag (to have one common interface.)