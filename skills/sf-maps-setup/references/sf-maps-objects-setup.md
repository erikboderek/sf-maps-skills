# Salesforce Maps — Setup & Configuration Objects Reference

All custom objects use the `SFMaps` managed package namespace.

## Core Configuration Objects

### SFMaps__Setting__c
Global Maps configuration record. One per org. Controls geocoding behavior, default map center, and feature flags.

Key fields:
- `SFMaps__DefaultLatitude__c` / `SFMaps__DefaultLongitude__c` — default map center
- `SFMaps__GeocodingEnabled__c` — master geocoding on/off switch
- `SFMaps__MobileEnabled__c` — enables the Salesforce Maps mobile app

### SFMaps__GeocodingConfig__c
One record per Salesforce object that has geocoding enabled.

Key fields:
- `SFMaps__SObjectType__c` — API name of the object (e.g., `Account`, `Lead`)
- `SFMaps__StreetField__c`, `SFMaps__CityField__c`, `SFMaps__StateField__c`, `SFMaps__ZipField__c`, `SFMaps__CountryField__c` — field mappings
- `SFMaps__Enabled__c` — whether geocoding is active for this object

### SFMaps__UserSetting__c
Per-user preferences: default filters, favorite layers, saved map views.

## Permission & License Objects

### SFMaps__License__c
Tracks Maps license assignment per user. Managed by the package — do not edit directly.

### SFMaps__UserPermission__c
Maps feature permissions per user, derived from assigned permission sets.

## Geocoding Status Values

| Status | Meaning |
|---|---|
| `Geocoded` | Successfully geocoded — has lat/lng |
| `Pending` | Queued for geocoding |
| `Failed` | Address could not be geocoded |
| `Not Geocoded` | Geocoding not yet attempted |
| `Excluded` | Record explicitly excluded from geocoding |

## SOQL Patterns

Find ungeocoded accounts:
```soql
SELECT Id, Name, BillingStreet, SFMaps__GeocodingStatus__c
FROM Account
WHERE SFMaps__GeocodingStatus__c IN ('Failed', 'Not Geocoded', 'Pending')
AND SFMaps__Geocodable__c = true
ORDER BY LastModifiedDate DESC
```

Find all geocoding configs:
```soql
SELECT SFMaps__SObjectType__c, SFMaps__Enabled__c, SFMaps__StreetField__c
FROM SFMaps__GeocodingConfig__c
ORDER BY SFMaps__SObjectType__c
```
