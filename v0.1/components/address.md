# Address

## Properties

### `address1`, `address2`

### `postalCode`

#### Decisions
* We use postal code instead of zipCode since zipCode is US-specific, most international schemas (OASIS, schema.ort etc.) use postal code
* The postal code is not required since there are countries that have no postal code system.

### `city`
#### TODO
* Length of 30 is not enough for international names, 50+ is recommended
* If CRM allows for longer city names we need a strategy for the ERP


#### TOODS
* Length: The current limit of 30 characters comes from the ERP system, but is not sufficient e.g. "San Fernando del Valle de Catamarca"
* If CRM allows for longer names we need a strategy how to shorten the names starting in Phase2, or ERP needs to change

## `stateorProvince`
Many countries require a state or province. 

### `countryCode`

### `Latitude`, `Longitude`
* Who is owner of the coordinates FLS/ERP or CRM.
* It seems adviseable to use lat/long because: precision (no prjection needed), commonly used (at least chatGpt says so), CRM seems to use it (at least this was their suggestion).
* FLS is currently _the_ souce for geo-location FLS seems to use a projected coordinate system (but not sure which)
* On-the fly conversion when exchanging data (project the FLS coordinates to a lat/long pair - might be trorublesome due to rounding)
* CRM creates coordinates from the given address

