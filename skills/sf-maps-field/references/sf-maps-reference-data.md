# Salesforce Maps Reference Data

Reference guide for the built-in third-party data sources available in Salesforce Maps Data Layers: Business Data (USA, Canada, UK) and Property Data (USA).

---

## Frequently Asked Questions

**Q. What countries are included in business data?**  
USA, Canada, and the UK.

**Q. How often is business data updated?**  
Monthly for USA and Canada. Quarterly for the UK.

**Q. How many records are in each data set?**  
- USA Business Data: 14M+ businesses  
- Canadian Business Data: 1M+ businesses  
- USA Property Data: 147,760,955 records

**Q. What is a SIC code?**  
The Standard Industrial Classification — a four-digit code system for classifying industries by primary activity. SIC is the older standard; NAICS is now preferred.

**Q. What is a NAICS code?**  
The North American Industry Classification System — replaced SIC in North America. Classifies businesses by type of economic activity.

**Q. What is a parent and ultimate parent ID?**  
A three-tier linkage: the Business ID of the headquarters (ultimate parent) appears on all linked records, along with the sub-parent ID (immediate parent). For example, Yum Brands owns Taco Bell — every linked record carries both the Yum Brands HQ ID and the Taco Bell sub-HQ ID, allowing roll-ups to either level.

**Q. How is Total Sales calculated?**  
Total Sales is modeled using employee size, years in business, local economic conditions, and SIC code.

**Q. What is the difference between mailing address and property location?**  
Mailing address is where the owner can be contacted; location is where the property is situated.

**Q. How does Property Data relate to MLS?**  
Property Data is sourced from county tax assessors and recorders — it covers the whole building (not individual units). Third-party data (HOA fees, satellite dishes, etc.) is not included. Loan information and interest rates may lag due to county office update schedules.

---

## USA Business Data

**14M+ businesses | 102 fields | Updated monthly**

| # | Label | Description | Format | Filter |
|---|---|---|---|---|
| 1 | 7-digit SIC | Full length SIC code | string | string |
| 2 | Company: Employer Identification Number (EIN) | Federal Tax ID (9-digit IRS number) | string | string |
| 3 | Company: Fortune 1000 Branches | — | decimal | decimal |
| 4 | Company: Fortune 1000 Rank | Numeric rank in the Fortune 1000 | decimal | decimal |
| 5 | Company: Immediate Parent Company City | City of the immediate parent company | string | string |
| 6 | Company: Immediate Parent Company Name | Name of the immediate parent company | string | string |
| 7 | Company: Immediate Parent Company State | State of the immediate parent company | string | string |
| 8 | Company: Location Type | Headquarters or branch | string | string |
| 9 | Company: Name | Company name | string | string |
| 10 | Company: Number of Linked Locations | Number of linked locations | decimal | decimal |
| 11 | Company: Stock Exchange | Stock exchange where the company trades | string | string |
| 12 | Company: Ticker Symbol | Stock ticker symbol | string | string |
| 13 | Company: Total Employees (Corporate) | Total employees across all branches; appears only on HQ and single-location records | decimal | decimal |
| 14 | Company: Total Employees (Location) | Employees at this location (some modeled) | decimal | decimal |
| 15 | Company: Ultimate Parent Company City | City of the ultimate parent company | string | string |
| 16 | Company: Ultimate Parent Company Name | Name of the ultimate parent company | string | string |
| 17 | Company: Ultimate Parent Company State | State of the ultimate parent company | string | string |
| 18 | Company: Website | — | url | string |
| 19 | Company: Year Established | Year the company was established | string | decimal |
| 20 | Company: Years in Business (Range) | Standardized range for years in business | string | string |
| 21 | Contact: Area Code | Area code of phone number | string | string |
| 22 | Contact: Email | Email address of the listed contact | string | string |
| 23 | Contact: Fax Number | Fax number | string | string |
| 24 | Contact: First Name | First name of listed contact | string | string |
| 25 | Contact: Full Name | Full name of listed contact | string | string |
| 26 | Contact: Gender | Gender of listed contact | string | string |
| 27 | Contact: Last Name | Last name of listed contact | string | string |
| 28 | Contact: Middle Initial | Middle initial of listed contact | string | string |
| 29 | Contact: Phone Number | Phone number | string | string |
| 30 | Contact: Prefix | Prefix (Mr, Ms, Dr, etc.) | string | string |
| 31 | Contact: Reported Job Title | Job title as reported by the company | string | string |
| 32 | Contact: Standardized Job Title | Standardized job title | string | string |
| 33 | Contact: Suffix | Suffix of listed contact | string | string |
| 34 | Credit: Capacity | Max recommended credit (proprietary model; not credit bureau data) | currency | decimal |
| 35 | Credit: Code | Credit rating grade letter (proprietary model) | string | string |
| 36 | Credit: Description | Description of credit rating | string | string |
| 37 | Credit: Score | Credit score (proprietary model) | decimal | decimal |
| 38 | Expenses: Accounting | Estimated accounting expenditure | string | string |
| 39 | Expenses: Advertising | Estimated advertising expenditure | string | string |
| 40 | Expenses: Business Insurance | Estimated insurance expenditure | string | string |
| 41 | Expenses: Legal | Estimated legal expenditure | string | string |
| 42 | Expenses: Office Equipment | Estimated office equipment expenditure | string | string |
| 43 | Expenses: Rent | Estimated rent expenditure | string | string |
| 44 | Expenses: Technology | Estimated technology expenditure | string | string |
| 45 | Expenses: Telecom | Estimated telecom expenditure | string | string |
| 46 | Expenses: Utilities | Estimated utilities expenditure | string | string |
| 47 | Flag: Email Availability | Whether an email address is associated with the contact | string | boolean |
| 48 | Flag: Female Owned | Business is owned by a woman | string | boolean |
| 49 | Flag: Fortune 1000 | Listed in the Fortune 1000 | string | boolean |
| 50 | Flag: Franchise | Part of a franchise | string | boolean |
| 51 | Flag: Home Based Business | Home-based business | string | boolean |
| 52 | Flag: Manufacturing Location | Manufacturing performed at this location | string | boolean |
| 53 | Flag: Non Profit | Not-for-profit organization | string | boolean |
| 54 | Flag: Public Ownership | Company issues stock or is part of a publicly traded company | string | boolean |
| 55 | Flag: Small Business | Classified as a small business | string | boolean |
| 56 | Location: 4-Digit ZIP | ZIP+4 extension | string | string |
| 57 | Location: Carrier Route | Mail carrier route code | string | string |
| 58 | Location: City | City of company location | string | string |
| 59 | Location: Core Based Statistical Area (CBSA) | Metropolitan area code (OMB definition; formerly MSA) | string | string |
| 60 | Location: County Code (FIPS) | FIPS county code | string | string |
| 61 | Location: County Name | County of company location | string | string |
| 62 | Location: Delivery Point | USPS delivery point digit | string | string |
| 63 | Location: Delivery Point Check Digit | USPS delivery point check digit | string | string |
| 64 | Location: Geo Match Level | Accuracy of address geocoding down to ZIP level | string | string |
| 65 | Location: Population of City (2010) | 2010 population estimate for the metro area | string | string |
| 66 | Location: Square Footage | Workspace area in square feet | decimal | decimal |
| 67 | Location: State | Two-digit FIPS state code | string | string |
| 68 | Location: Street Address | Street address | string | string |
| 69 | Location: Time Zone | — | string | string |
| 70 | Location: Total Personal Computers (PC) | Range of PCs at this location | string | string |
| 71 | Location: ZIP Code | 5-digit ZIP code | string | string |
| 72 | Mailing: 4-Digit ZIP | ZIP+4 extension for mailing address | string | string |
| 73 | Mailing: Carrier Route | Mail carrier route for mailing address | string | string |
| 74 | Mailing: City | City for mailing address | string | string |
| 75 | Mailing: Delivery Point | USPS delivery point for mailing address | string | string |
| 76 | Mailing: Delivery Point Check Digit | USPS check digit for mailing address | string | string |
| 77 | Mailing: Mail Deliverability | Risk that mail will be missorted or undelivered | string | string |
| 78 | Mailing: Sectional Center Facility (SCF) | USPS Processing & Distribution Center for mailing address | string | string |
| 79 | Mailing: State | State for mailing address | string | string |
| 80 | Mailing: Street Address | Street address, PO box, or rural route for mailing | string | string |
| 81 | Mailing: ZIP Code | 5-digit ZIP for mailing address | string | string |
| 82 | NAICS: Level 1 - Sector | 2-digit NAICS sector description | string | string |
| 83 | NAICS: Level 1 - Sector Code | 2-digit NAICS sector code | string | string |
| 84 | NAICS: Level 2 - Sub-Sector | 3-digit NAICS sub-sector description | string | string |
| 85 | NAICS: Level 2 - Sub-Sector Code | 3-digit NAICS sub-sector code | string | string |
| 86 | NAICS: Level 3 - Industry Group | 4-digit NAICS industry group description | string | string |
| 87 | NAICS: Level 3 - Industry Group Code | 4-digit NAICS industry group code | string | string |
| 88 | NAICS: Level 4 - North American Industry | 5-digit NAICS North American industry description | string | string |
| 89 | NAICS: Level 4 - North American Industry Code | 5-digit NAICS code | string | string |
| 90 | NAICS: Level 5 - US Industry | 6-digit NAICS US industry description | string | string |
| 91 | NAICS: Level 5 - US Industry Code | 6-digit NAICS code | string | string |
| 92 | SIC: Level 1 - Division | SIC lettered division description | string | string |
| 93 | SIC: Level 1 - Division Code | SIC lettered division code | string | string |
| 94 | SIC: Level 2 - Major Group | 2-digit SIC major group description | string | string |
| 95 | SIC: Level 2 - Major Group Code | 2-digit SIC major group code | string | string |
| 96 | SIC: Level 3 - Industry Group | 3-digit SIC industry group description | string | string |
| 97 | SIC: Level 3 - Industry Group Code | 3-digit SIC industry group code | string | string |
| 98 | SIC: Level 4 - Industry | 4-digit SIC industry description | string | string |
| 99 | SIC: Level 4 - Industry Code | 4-digit SIC industry code | string | string |
| 100 | Sales: Range (USD) | Annual sales range (modeled for most records) | string | string |
| 101 | Sales: Total (USD) | Total annual sales (modeled for most records) | currency | decimal |

---

## Canadian Business Data

**1M+ businesses | 72 fields | Updated monthly**

| # | Label | Description | Format | Filter |
|---|---|---|---|---|
| 1 | Address | Location address for the business | string | string |
| 2 | Address: City | City of business address | string | string |
| 3 | Address: City 2 | Secondary city name | string | string |
| 4 | Address: Number | Address number | string | string |
| 5 | Address: Post Direction | Postdirectional for address | string | string |
| 6 | Address: Postal Code | 7-digit postal code (format: A1A 1A1) | string | string |
| 7 | Address: Pre Direction | Predirectional for address | string | string |
| 8 | Address: Province | Province of business address | string | string |
| 9 | Address: Street Name | Street name | string | string |
| 10 | Address: Street Suffix | Street suffix | string | string |
| 11 | Address: Unit Designator | Suite or apartment designator (e.g., Ste, Apt) | string | string |
| 12 | Address: Unit Designator Number | Suite or apartment number | string | string |
| 13 | Business Status Description | Description of business status code | string | string |
| 14 | Company: Linkage Description | Linked company name | string | string |
| 15 | Company: Name | Primary name of business | string | string |
| 16 | Company: Stock Exchange | Stock exchange code | string | string |
| 17 | Company: Ticker Symbol | Stock ticker symbol | string | string |
| 18 | Company: Website | Primary URL | url | string |
| 19 | Contact: Area Code | 3-digit area code | string | string |
| 20 | Expenses: Accounting (Exact) | Modeled accounting expense | currency | decimal |
| 21 | Expenses: Accounting (Range) | Range of accounting expense code | string | string |
| 22 | Expenses: Advertising (Exact) | Modeled advertising expense | currency | decimal |
| 23 | Expenses: Advertising (Range) | Range of advertising expense code | string | string |
| 24 | Expenses: Business Insurance (Exact) | Modeled business insurance expense | currency | decimal |
| 25 | Expenses: Business Insurance (Range) | Range of business insurance expense code | string | string |
| 26 | Expenses: Legal (Exact) | Modeled legal expense | currency | decimal |
| 27 | Expenses: Legal (Range) | Range of legal expense code | string | string |
| 28 | Expenses: Office Equipment (Exact) | Modeled office equipment expense | currency | decimal |
| 29 | Expenses: Office Equipment (Range) | Range of office equipment expense code | string | string |
| 30 | Expenses: Rent (Exact) | Modeled rent expense | currency | decimal |
| 31 | Expenses: Rent (Range) | Range of rent expense code | string | string |
| 32 | Expenses: Technology (Exact) | Modeled technology expense | currency | decimal |
| 33 | Expenses: Technology (Range) | Range of technology expense code | string | string |
| 34 | Expenses: Telecom (Exact) | Modeled telecom expense | currency | decimal |
| 35 | Expenses: Telecom (Range) | Range of telecom expense code | string | string |
| 36 | Expenses: Utilities (Exact) | Modeled utilities expense | currency | decimal |
| 37 | Expenses: Utilities (Range) | Range of utilities expense code | string | string |
| 38 | Flag: Phone | Presence of phone number | boolean | boolean |
| 39 | Flag: Public Ownership | Public status of a company | boolean | boolean |
| 40 | Flag: Toll Free | Presence of toll-free number | boolean | boolean |
| 41 | Flag: URL Exists | Record has a company, Facebook, or Google+ URL | boolean | boolean |
| 42 | Location: Square Footage (Exact) | Modeled square footage | integer | decimal |
| 43 | Location: Square Footage Description (Range) | Description of square footage code | string | string |
| 44 | Location: Time Zone | Description of time zone code | string | string |
| 45 | Location: Total Employees (Exact) | Number of employees at business location | integer | decimal |
| 46 | Location: Total Employees (Range) | Description of employees location code | string | string |
| 47 | Location: Total Personal Computers (Exact) | Modeled number of PCs | integer | decimal |
| 48 | Location: Total Personal Computers (Range) | Description of PC count code | string | string |
| 49 | Mail Deliverability Score | Level of mail deliverability | string | string |
| 50 | NAICS: Level 1 - Sector | 2-digit NAICS sector description | string | string |
| 51 | NAICS: Level 1 - Sector Code | 2-digit NAICS sector code | string | string |
| 52 | NAICS: Level 2 - Sub-Sector | 3-digit NAICS sub-sector description | string | string |
| 53 | NAICS: Level 2 - Sub-Sector Code | 3-digit NAICS sub-sector code | string | string |
| 54 | NAICS: Level 3 - Industry Group | 4-digit NAICS industry group description | string | string |
| 55 | NAICS: Level 3 - Industry Group Code | 4-digit NAICS code | string | string |
| 56 | NAICS: Level 4 - North American Industry | 5-digit NAICS description | string | string |
| 57 | NAICS: Level 4 - North American Industry Code | 5-digit NAICS code | string | string |
| 58 | NAICS: Level 5 - US Industry | 6-digit NAICS description | string | string |
| 59 | NAICS: Level 5 - US Industry Code | 6-digit NAICS code | string | string |
| 60 | Phone | 10-digit primary business phone number | string | string |
| 61 | SIC: Level 1 - Division | SIC lettered division description | string | string |
| 62 | SIC: Level 1 - Division Code | SIC lettered division code | string | string |
| 63 | SIC: Level 2 - Major Group | 2-digit SIC major group description | string | string |
| 64 | SIC: Level 2 - Major Group Code | 2-digit SIC major group code | string | string |
| 65 | SIC: Level 3 - Industry Group | 3-digit SIC industry group description | string | string |
| 66 | SIC: Level 3 - Industry Group Code | 3-digit SIC code | string | string |
| 67 | SIC: Level 4 - Industry | 4-digit SIC industry description | string | string |
| 68 | SIC: Level 4 - Industry Code | 4-digit SIC code | string | string |
| 69 | Sales: Range | Description of modeled sales code | string | string |
| 70 | Sales: Total | Modeled sales value | currency | decimal |
| 71 | Toll Free Number | 10-digit toll-free business phone number | string | string |

---

## UK Business Data

**Companies House data | 41 fields | Updated quarterly**

| # | Label | Format | Filter |
|---|---|---|---|
| 1 | Accounts: Account Category | string | string |
| 2 | Accounts: Account Ref Day | integer | decimal |
| 3 | Accounts: Account Ref Month | integer | decimal |
| 4 | Accounts: Last Made Update | date | date |
| 5 | Accounts: Next Due Date | date | date |
| 6 | Company Category | string | string |
| 7 | Company Name | string | string |
| 8 | Company Number | string | string |
| 9 | Company Status | string | string |
| 10 | Confirmation Statement: Last Made Update | date | date |
| 11 | Confirmation Statement: Next Due Date | date | date |
| 12 | Country of Origin | string | string |
| 13 | Incorporation Date | date | date |
| 14 | Limited Partnerships: Number of General Partners | integer | decimal |
| 15 | Limited Partnerships: Number of Limited Partners | integer | decimal |
| 16 | Mortgages: Number of Charges | integer | decimal |
| 17 | Mortgages: Number of Charges Outstanding | integer | decimal |
| 18 | Mortgages: Number of Charges Part Satisfied | integer | decimal |
| 19 | Mortgages: Number of Charges Satisfied | integer | decimal |
| 20 | Previous Name 1: Change of Name Date | date | date |
| 21 | Previous Name 1: Company Name | string | string |
| 22 | Previous Name 2: Change of Name Date | date | date |
| 23 | Previous Name 2: Company Name | string | string |
| 24 | Previous Name 3: Change of Name Date | date | date |
| 25 | Previous Name 3: Company Name | string | string |
| 26 | Registered Office Address: Address Line 1 | string | string |
| 27 | Registered Office Address: Address Line 2 | string | string |
| 28 | Registered Office Address: Care Of | string | string |
| 29 | Registered Office Address: Country | string | string |
| 30 | Registered Office Address: County | string | string |
| 31 | Registered Office Address: PO Box | string | string |
| 32 | Registered Office Address: Post Code | string | string |
| 33 | Registered Office Address: Post Town | string | string |
| 34 | Returns: Last Made Update | date | date |
| 35 | Returns: Next Due Date | date | date |
| 36 | SIC: 1st SIC Code/Text | string | string |
| 37 | SIC: 2nd SIC Code/Text | string | string |
| 38 | SIC: 3rd SIC Code/Text | string | string |
| 39 | SIC: 4th SIC Code/Text | string | string |
| 40 | URI | url | string |

---

## USA Property Data

**147,760,955 records | 124 fields | Updated monthly | Source: county tax assessors**

| # | Label | Description | Format | Filter |
|---|---|---|---|---|
| 1 | Area Building Definition Code | Details the area described by the AreaBuilding value | string | string |
| 2 | Area of Total Living Space (Square Feet) | Living sq ft of all structures on the property | integer | decimal |
| 3 | Assessor Last Sale Amount | Amount paid by primary owner (assessor data) | currency | decimal |
| 4 | Assessor Last Sale Date | Date primary owner acquired the property (YYYY-MM-DD) | date | date |
| 5 | Assessor Prior Sale Amount | Amount paid by previous owner (assessor data) | currency | decimal |
| 6 | Available Equity | AVM value minus sum of current outstanding loan amounts | currency | decimal |
| 7 | Census Block Group | US Census Block Group for subject property | integer | decimal |
| 8 | Census Tract | US Census Tract Code for subject property | integer | decimal |
| 9 | Combined Statistical Area | CSA name as defined by OMB | string | string |
| 10 | Construction Type | Construction type | string | string |
| 11 | Contact: Owner Mailing Address Carrier Route | Mailing carrier route | string | string |
| 12 | Contact: Owner Mailing Address City | Mailing city | string | string |
| 13 | Contact: Owner Mailing Address Full | Full mailing address | string | string |
| 14 | Contact: Owner Mailing Address State | Mailing state | string | string |
| 15 | Contact: Owner Mailing Address Type | Mailing address type (Standard, PO Box, Rural Route, etc.) | string | string |
| 16 | Contact: Owner Mailing Address Zip | Mailing zip code | string | string |
| 17 | Contact: Owner Mailing Address Zip 4 | Mailing zip+4 | string | string |
| 18 | Contact: Owner Mailing County | Mailing county | string | string |
| 19 | Contact: Owner Mailing FIPS | Mailing FIPS county code | string | string |
| 20 | Core Based Statistical Area (CBSA) Name | CBSA name as defined by OMB | string | string |
| 21 | Count: Bathrooms | Total bathroom count (includes partial) | decimal | decimal |
| 22 | Count: Bedrooms | Total bedroom count | integer | decimal |
| 23 | Count: Buildings | Number of buildings on the property | integer | decimal |
| 24 | Count: Fireplace | Number of fireplaces | integer | decimal |
| 25 | Count: Rooms | Total room count across all buildings | integer | decimal |
| 26 | Count: Solar Permits | Total solar permit count | integer | decimal |
| 27 | Count: Stories | Number of stories across all buildings | integer | decimal |
| 28 | Count: Units | Number of units on the property | integer | decimal |
| 29 | Current First Position Mortgage Type | Loan type for first lien position (conventional, FHA, HELoC, etc.) | string | string |
| 30 | Current First Position Open Loan Amount | Original amount of first lien loan (modeled) | currency | decimal |
| 31 | Current First Position Open Loan Document Number Formatted | Document number for first lien loan | string | string |
| 32 | Current First Position Open Loan Interest Rate | Interest rate for first lien loan | decimal | decimal |
| 33 | Current First Position Open Loan Interest Rate Type | Rate terms for first lien loan (blank if unknown) | string | string |
| 34 | Current First Position Open Loan Lender Info Entity Classification | Lender type for first lien loan | string | string |
| 35 | Current First Position Open Loan Lender Name First | Lender company name or individual first name | string | string |
| 36 | Current First Position Open Loan Lender Name Last | Lender individual last name | string | string |
| 37 | Current First Position Open Loan Recording Date | Recording date for first lien document | date | date |
| 38 | Current First Position Open Loan Type | Loan type: P=Purchase, R=Refinance, E=Equity | string | string |
| 39 | Current Open Loan Amount | Sum of first + second + third position loan amounts | currency | decimal |
| 40 | Current Second Position Mortgage Type | Loan type for second lien position | string | string |
| 41 | Current Second Position Open Loan Amount | Original amount of second lien loan | currency | decimal |
| 42 | Current Second Position Open Loan Document Number Formatted | Document number for second lien loan | string | string |
| 43 | Current Second Position Open Loan Interest Rate | Interest rate for second lien loan | decimal | decimal |
| 44 | Current Second Position Open Loan Interest Rate Type | Rate terms for second lien loan | string | string |
| 45 | Current Second Position Open Loan Lender Info Entity Classification | Lender type for second lien loan | string | string |
| 46 | Current Second Position Open Loan Lender Name First | Lender company name or individual first name | string | string |
| 47 | Current Second Position Open Loan Lender Name Last | Lender individual last name | string | string |
| 48 | Current Second Position Open Loan Recording Date | Recording date for second lien document | date | date |
| 49 | Current Second Position Open Loan Type | Loan type: P/R/E | string | string |
| 50 | Current Third Position Mortgage Type | Loan type for third lien position | string | string |
| 51 | Current Third Position Open Loan Amount | Original amount of third lien loan | currency | decimal |
| 52 | Current Third Position Open Loan Document Number Formatted | Document number for third lien loan | string | string |
| 53 | Current Third Position Open Loan Interest Rate | Interest rate for third lien loan | decimal | decimal |
| 54 | Current Third Position Open Loan Interest Rate Type | Rate terms for third lien loan | string | string |
| 55 | Current Third Position Open Loan Lender Info Entity Classification | Lender type for third lien loan | string | string |
| 56 | Current Third Position Open Loan Lender Name First | Lender company name or individual first name | string | string |
| 57 | Current Third Position Open Loan Lender Name Last | Lender individual last name | string | string |
| 58 | Current Third Position Open Loan Recording Date | Recording date for third lien document | date | date |
| 59 | Current Third Position Open Loan Type | Loan type: P/R/E | string | string |
| 60 | Deed Last Sale Date | Latest sale date (YYYY-MM-DD) | date | date |
| 61 | Deed Last Sale Price | Latest sale price | currency | decimal |
| 62 | Fireplace Type | Fireplace presence and type | string | string |
| 63 | First Owner Type | Company, individual, government, or unknown | string | string |
| 64 | Flag: Loan | True if loan data exists | boolean | boolean |
| 65 | Flag: Owner Occupied | Owner status — Absentee or Occupied (logic-based) | boolean | boolean |
| 66 | Flag: Parking Carport on Property | Carport exists on the property | boolean | boolean |
| 67 | Flag: Solar | Solar data exists | boolean | boolean |
| 68 | Foundation Type | Type of foundation for primary structure | string | string |
| 69 | Gross Area of All Structures (Square Footage) | Gross sq ft of all structures | integer | decimal |
| 70 | HVAC Cooling Type | Cooling method or system | string | string |
| 71 | HVAC Heating Fuel Type | Primary heating fuel | string | string |
| 72 | HVAC Heating Type | Heating method or system | string | string |
| 73 | Last Ownership Transfer Date | Most recent ownership transfer date (YYYY-MM-DD) | date | date |
| 74 | Lendable Equity | 80% of (AVM value minus outstanding loan amounts) | currency | decimal |
| 75 | Loan-to-Value Ratio (LTV) | Sum of open loans divided by AVM value | integer | decimal |
| 76 | Lot Area (Square Footage) | Lot size in square feet | decimal | decimal |
| 77 | Lot Depth (Feet) | Lot depth in feet | decimal | decimal |
| 78 | Lot Size (Acres) | Lot size in acres | decimal | decimal |
| 79 | Lot Width (Feet) | Lot width in feet | decimal | decimal |
| 80 | Metropolitan Statistical Area Name | MSA name as defined by OMB | string | string |
| 81 | Owner 1 Name Full | Full unparsed name of first owner | string | string |
| 82 | Owner 2 Name Full | Full unparsed name of second owner | string | string |
| 83 | Ownership Vesting Type | Ownership vesting held by the owner(s) | string | string |
| 84 | Parcel Number Formatted | Legacy Parcel — deprecated | string | string |
| 85 | Parcel Number Raw | Primary parcel number and unique identifier within the county | string | string |
| 86 | Parking Garage Area (Square Footage) | Garage square footage | integer | decimal |
| 87 | Parking Garage Type | Garage presence, type (attached/detached, etc.) | string | string |
| 88 | Pool Type | Pool presence and type | integer | decimal |
| 89 | Porch Area (Square Footage) | Total porch square footage | integer | decimal |
| 90 | Porch Type | Porch presence and type | string | string |
| 91 | Previous Assessed Value | Previous total assessed value | currency | decimal |
| 92 | Primary Exterior Wall Covering Material | Exterior wall finish material | string | string |
| 93 | Property Address County | County where the property is situated | string | string |
| 94 | Property Address State Code | State where the property is situated | string | string |
| 95 | Property: Address City | Site address city | string | string |
| 96 | Property: Address Full | Full site address line | string | string |
| 97 | Property: Address State | Site address state | string | string |
| 98 | Property: Address Zip | Site address zip code | string | string |
| 99 | Property: Address Zip 4 | Site address zip+4 | string | string |
| 100 | Property: Jurisdiction Name | Tax jurisdiction name (typically the county) | string | string |
| 101 | Property: Latitude | Latitude in degrees | decimal | decimal |
| 102 | Property: Longitude | Longitude in degrees | decimal | decimal |
| 103 | Property: Use (Detail) | Standardized property use description (from assessor zoning data) | string | string |
| 104 | Property: Use Group (Main) | General property type: residential, commercial, other, etc. | string | string |
| 105 | Record Year Added | Year current parcel number was introduced into the database | string | decimal |
| 106 | Roof Material | Primary roof finish material | string | string |
| 107 | Second Owner Type | Company, individual, government, or unknown | string | string |
| 108 | Solar Permit Date (Most Recent) | Date of most recent solar permit | date | date |
| 109 | Solar Permit Job Cost (Total) | Valuation amount of solar permit | currency | decimal |
| 110 | Structure Style Type | Structural style or style elements | integer | decimal |
| 111 | Subdivision Name | Subdivision name | string | string |
| 112 | Tax: Assessed Value Improvements | Assessed value of improvements | currency | decimal |
| 113 | Tax: Assessed Value Land | Assessed value of land | currency | decimal |
| 114 | Tax: Assessed Value Total | Total assessed value | currency | decimal |
| 115 | Tax: Billed Amount | Tax amount billed for the tax year | currency | decimal |
| 116 | Tax: Fiscal Year | Year of property taxes provided | string | decimal |
| 117 | Tax: Market Value Improvements | Market value of improvements | currency | decimal |
| 118 | Tax: Market Value Land | Market value of land | currency | decimal |
| 119 | Tax: Market Value Total | Total market value | currency | decimal |
| 120 | Tax: Year Assessed | Year of assessed values | string | decimal |
| 121 | Type of View From Property | Ocean, mountain, or other notable view | string | string |
| 122 | Year Built (Adjusted for Structural Changes) | Adjusted year built based on condition/major changes | string | decimal |
| 123 | Year Built (Primary Structure) | Year built of the primary structure | string | decimal |
