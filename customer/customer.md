# Customer

# Message type
* customer.create (includes resellerId)
* customer.update (does not include reseller id)

## Validation rules

### resellerId
* Can only point to customers, that are marked as reseller
* Can only be null for one customer (the root customer)
* Must not form loops
* Must not be modified after creation

### isReseller
* Cannot be changed from true to false if the reseller has linked sub-customers

## Things to discuss
* Should a resellet be a separate entity (pro: specific propeties, con: ?)
* Do customer relations have properties?
* email addresses as specific email properties: pro: explicit, con: new types need contract change
* reseller code - unique code identifying a customer like HHAT: that could be just an alias to the customer name
* isReseller flag, same pattern for all other relationships?
* as isReseller can only be modified on create, a separate create customer and update customer
* address.latitude, longitude: seems widely accepted that you shoud use geographic system with lat, long not a projected.
* geolocationQuality: Probably not relevant in the CRM, keep it in the FLS if possible (like name4 in hsm)
* responsibilityCenter is a customer with additional data? isResponsibilityCenter Flag, separate message for updating.
* customer.status separate from deleted flag (to have one common interface.)

# General questions
* Error response object