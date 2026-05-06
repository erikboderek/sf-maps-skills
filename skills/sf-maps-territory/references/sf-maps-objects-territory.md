# Salesforce Maps — Territory Objects Reference

## SFMaps__Territory__c

The core territory record.

| Field | API Name | Type | Notes |
|---|---|---|---|
| Name | `Name` | Text | Territory display name |
| Territory Type | `SFMaps__TerritoryType__c` | Picklist | `Geographic`, `Attribute`, `Manual`, `Overlay` |
| Hierarchy Level | `SFMaps__TerritoryHierarchy__c` | Lookup | Parent hierarchy level config |
| Parent Territory | `SFMaps__ParentTerritory__c` | Lookup | Lookup to `SFMaps__Territory__c` |
| Active | `SFMaps__Active__c` | Checkbox | Inactive territories are excluded from routing/assignment |
| Boundary Polygon | `SFMaps__BoundaryPolygon__c` | TextArea | GeoJSON polygon definition |
| Coverage Complete | `SFMaps__CoverageComplete__c` | Checkbox | Whether all coverage units are assigned |

## SFMaps__AssignmentRule__c

| Field | API Name | Type | Notes |
|---|---|---|---|
| Rule Type | `SFMaps__RuleType__c` | Picklist | `Boundary`, `Attribute`, `Combined` |
| Territory | `SFMaps__Territory__c` | Lookup | Parent territory |
| Priority | `SFMaps__Priority__c` | Number | Lower = higher priority |
| Active | `SFMaps__Active__c` | Checkbox | |
| Attribute Field | `SFMaps__AttributeField__c` | Text | API field name for attribute rules |
| Attribute Value | `SFMaps__AttributeValue__c` | Text | Value to match |
| Assign All Matching | `SFMaps__AssignAllMatching__c` | Checkbox | If true, assign record to all matching territories (needed for overlays) |

## SFMaps__CoverageUnit__c

Pre-built geographic boundaries from Reference Data.

| Field | API Name | Type | Notes |
|---|---|---|---|
| Territory | `SFMaps__Territory__c` | Lookup | Which territory owns this unit |
| Unit Type | `SFMaps__UnitType__c` | Picklist | `ZipCode`, `County`, `State`, `CensusTract`, etc. |
| Unit Code | `SFMaps__UnitCode__c` | Text | The boundary code (e.g., `94105` for zip, `San Francisco` for county) |
| Country | `SFMaps__Country__c` | Text | ISO country code |

## SFMaps__TerritoryRecordAssignment__c

Links Salesforce records to territories.

| Field | API Name | Type | Notes |
|---|---|---|---|
| Record | `SFMaps__Record__c` | Lookup (polymorphic) | The assigned Salesforce record |
| Territory | `SFMaps__Territory__c` | Lookup | The owning territory |
| Assignment Type | `SFMaps__AssignmentType__c` | Picklist | `Auto` (rule-based) or `Manual` |
| Effective Date | `SFMaps__EffectiveDate__c` | Date | When assignment takes effect |

## Key Design Constraints

1. **Polygon limits**: Each territory polygon can contain a maximum of 1,000 vertices. For complex boundaries, use Coverage Units (zip/county stacking) instead of hand-drawn polygons.
2. **Assignment rule limit**: 500 active assignment rules per org.
3. **Re-alignment jobs**: Running a full territory re-alignment (re-assigning all records) is a batch job — schedule during off-peak hours for orgs with >100k account records.
4. **Hierarchy immutability**: Territory hierarchy levels cannot be renamed after records are assigned to territories at that level. Plan the hierarchy before creating territories.
