## Latitude & Longitude
* Who is owner of the coordinates FLS/ERP or CRM.
* It seems adviseable to use lat/long because: precision (no prjection needed), commonly used (at least chatGpt says so), CRM seems to use it (at least this was their suggestion).
* FLS is currently _the_ souce for geo-location FLS seems to use a projected coordinate system (but not sure which)
* On-the fly conversion when exchanging data (project the FLS coordinates to a lat/long pair - might be trorublesome due to rounding)
* CRM creates coordinates from the given address

## Geolocation quality
* Currently only used in FLS and ERP