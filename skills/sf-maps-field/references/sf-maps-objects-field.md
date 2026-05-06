# Salesforce Maps — Field Sales Objects Reference

## SFMaps__Route__c

| Field | API Name | Type | Notes |
|---|---|---|---|
| Name | `Name` | Text | Auto-generated or rep-named |
| Date | `SFMaps__Date__c` | Date | The day this route is planned for |
| User | `SFMaps__User__c` | Lookup(User) | The field rep |
| Status | `SFMaps__Status__c` | Picklist | `Draft`, `In Progress`, `Completed`, `Cancelled` |
| Start Address | `SFMaps__StartAddress__c` | Text | Route start point |
| End Address | `SFMaps__EndAddress__c` | Text | Route end point (can match start) |
| Total Distance | `SFMaps__TotalDistance__c` | Number | Miles or km (unit set in org prefs) |
| Total Duration | `SFMaps__TotalDuration__c` | Number | Estimated minutes for entire route |
| Optimized | `SFMaps__Optimized__c` | Checkbox | Whether Maps has optimized stop order |

## SFMaps__RouteStop__c

| Field | API Name | Type | Notes |
|---|---|---|---|
| Route | `SFMaps__Route__c` | Lookup | Parent route |
| Stop Order | `SFMaps__StopOrder__c` | Number | Sequence position |
| Record | `SFMaps__Stop__c` | Lookup (polymorphic) | The Salesforce record being visited |
| Status | `SFMaps__Status__c` | Picklist | `Planned`, `Completed`, `Skipped`, `Cancelled` |
| Scheduled Arrival | `SFMaps__ScheduledArrival__c` | DateTime | Calculated ETA |
| Actual Arrival | `SFMaps__ActualArrival__c` | DateTime | Set on check-in |
| Notes | `SFMaps__Notes__c` | TextArea | Rep notes for the stop |
| Duration | `SFMaps__Duration__c` | Number | Planned visit duration (minutes) |

## SFMaps__CheckIn__c

| Field | API Name | Type | Notes |
|---|---|---|---|
| User | `SFMaps__User__c` | Lookup(User) | Who checked in |
| Route Stop | `SFMaps__RouteStop__c` | Lookup | Parent route stop (if from a route) |
| Record | `SFMaps__Record__c` | Lookup (polymorphic) | The Salesforce record visited |
| Check-In Time | `SFMaps__CheckInTime__c` | DateTime | GPS-verified arrival timestamp |
| Latitude | `SFMaps__Latitude__c` | Number | GPS latitude at check-in |
| Longitude | `SFMaps__Longitude__c` | Number | GPS longitude at check-in |
| Distance from Record | `SFMaps__DistanceFromRecord__c` | Number | Feet between rep GPS and record address |
| Activity Created | `SFMaps__ActivityCreated__c` | Checkbox | Whether a Task/Event was auto-created |

## SFMaps__DataLayer__c

| Field | API Name | Type | Notes |
|---|---|---|---|
| Name | `Name` | Text | Layer display name in the Maps panel |
| Object API Name | `SFMaps__ObjectApiName__c` | Text | e.g., `Account`, `Lead` |
| Label Field | `SFMaps__LabelField__c` | Text | Field API name for the pin tooltip |
| Color Field | `SFMaps__ColorField__c` | Text | Field used for color-coding pins |
| Max Records | `SFMaps__MaxRecords__c` | Number | Default 5,000 |
| Active | `SFMaps__Active__c` | Checkbox | |
| Shared | `SFMaps__Shared__c` | Checkbox | If true, all Maps users see this layer |

## Marker vs. Route Stop vs. Check-In

| Concept | Object | When to Use |
|---|---|---|
| Custom map pin | `SFMaps__Marker__c` | Annotate the map with a POI not in Salesforce |
| Planned visit | `SFMaps__RouteStop__c` | A stop on an optimized route |
| Logged visit | `SFMaps__CheckIn__c` | GPS-verified record of a completed visit |

Do not conflate these: a check-in without a route stop is valid (walk-in or unplanned visit). A route stop without a check-in means the rep planned but didn't log arrival.
