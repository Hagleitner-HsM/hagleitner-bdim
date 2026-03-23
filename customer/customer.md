# Customer

## AMQP Message types
### Events
* customer.created 
* customer.updated

### Commands
* customer.update
* customer.setstatus




## Validation rules & notes

### customerId
We use a uuid since the ownership of the customer entity will change, and in the transition phase multiple systems will provide customers (namely BC and HsM), therfore a uuid is favored over a simple sequential integer ID. Additionally BC already has a uuid for each customer (and for each tenant).

### erpCustomerId
This is an ID that is only useful in the context of the ERP, but be still store it in the CRM,
since it easens the communication. E.g. if we have multiple customer with similar name and address, this ID is used in internal communications.

### roles
Instead of using flags like isReseller, roles provide a more abstract and extensible model for
designating customers to one or more roles, this also relates to role-spcific models.

### block
* ERP: status = Freigabestatus
* ERP: blockedInERP: 0: not blocked, 1: blocked for delivery but invoiding still possible, 2: blocked for Invoicing (), 3: completely blocked
* ERP:BlockreasonCode
* ERP: requestForBlockERP (Sperranfrage pending)



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