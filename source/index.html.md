---
title: CharityBase Docs

language_tabs:
  - shell
  - javascript

toc_footers:
  - <a href='https://charitybase.uk/api-portal'>Sign Up for an API Key</a>
  - <a href='https://charitybase.uk'>CharityBase Web App</a>
  - <a href='https://charitybase.uk/about#licence'>Licence</a>
  - <a href='https://github.com/charity-base/charity-base-docs'>Edit on GitHub</a>

includes:
  - errors

search: true
---

# Introduction

The CharityBase REST API provides public information regarding the activities, locations and finances of all currently registered charities in England and Wales.  The API powers the [CharityBase Web App](https://charitybase.uk) - see the [About](https://charitybase.uk/about) page for more information.

To get started, sign up for an API key from the [API Portal](https://charitybase.uk/api-portal).

# Charities

## List Charities

> E.g. list charities with income £10k - £100k:

```shell
curl "https://charitybase.uk/api/v4.0.1/charities\
?apiKey=my-api-key\
&fields=income.latest.total\
&incomeRange=10000,100000\
&sort=income.latest.total:desc"
```

```javascript
const charityBase = require('charity-base')({
  apiKey: 'my-api-key'
})

const responsePromise = charityBase.charity.list({
  fields: ['income.latest.total'],
  incomeRange: [10000, 100000],
  sort: ['income.latest.total:desc'],
})
```

> Example Response:

```json
{
  "version": "v4.0.1",
  "query": { ... },
  "charities": [
    {
      "income": { "latest": { "total": 99997 } },
      "name": "The Feinmann Trust",
      "ids": { "GB-CHC": 1045149, "charityId": "GB-CHC-1045149" }
    },
    ...
  ]
}
```

Returns a list of registered charities.  By specifying query string parameters you can:

* Choose which fields are returned on each charity
* Filter, sort and page through the results

### Query Parameters

Parameter               |  Description
----------------------- | --------------
`apiKey`                | Key obtained from the [API Portal](https://charitybase.uk/api-portal) (required)
`fields`                | Comma-separated list of properties (`ids` & `name` are always returned)
`ids.GB-CHC`            | List of England & Wales charity number
`search`                | Query string for full-text search
`incomeRange`           | List of the form `[min, max]`
`addressWithin`         | List of coords the form `[maxLat, minLon, minLat, maxLon]`
`areasOfOperation.id`   | List of [area of operation ids](#area-of-operation-id-lookup)
`causes.id`             | List of [cause ids](#cause-id-lookup)
`beneficiaries.id`      | List of [beneficiary ids](#beneficiary-id-lookup)
`operations.id`         | List of [operation ids](#operation-id-lookup)
`funders`               | List of [funder ids](#funder-id-lookup)
`hasGrant`              | Boolean of whether charities have received public grant or not
`grantDateRange`        | List of the form `[YYYY-MM-DD, YYYY-MM-DD]`
`sort`                  | List of fields to sort by with optional `:asc` or `:desc` suffixes
`limit`                 | Number of results per request (default `10`, max `50`)
`skip`                  | Number of results to skip (default `0`)

<aside class="notice">
  See the repo <a href='https://github.com/charity-base/charity-base-schema'>charity-base-schema</a> for the full list of field options.  Nested properties are allowed.
</aside>

<aside class="notice">
  Use <code>fields=*</code> to return all properties from the schema.  Think carefully before doing this as it significantly increases the response size.
</aside>

## Count Charities

> E.g. count Big Lottery funded animal charities operating in North Yorkshire:

```shell
curl "https://charitybase.uk/api/v4.0.1/count-charities\
?apiKey=my-api-key\
&funders=360G-blf\
&causes.id=111\
&areasOfOperation.id=B-122"
```

```javascript
const charityBase = require('charity-base')({
  apiKey: 'my-api-key'
})

const responsePromise = charityBase.charity.count({
  'funders': '360G-blf',
  'causes.id': [111],
  'areasOfOperation.id': ['B-122'],
})
```

> Example Response:

```json
{
  "version": "v4.0.1",
  "query": { ... },
  "count": 11
}
```

Returns the count of all registered charities matching the specified query.

### Query Parameters

Parameter               |  Description
----------------------- | --------------
`apiKey`                | Key obtained from the [API Portal](https://charitybase.uk/api-portal) (required)
`ids.GB-CHC`            | List of England & Wales charity number
`search`                | Query string for full-text search
`incomeRange`           | List of the form `[minInclusive, maxExclusive]`
`addressWithin`         | List of coords the form `[maxLat, minLon, minLat, maxLon]`
`areasOfOperation.id`   | List of [area of operation ids](#area-of-operation-id-lookup)
`causes.id`             | List of [cause ids](#cause-id-lookup)
`beneficiaries.id`      | List of [beneficiary ids](#beneficiary-id-lookup)
`operations.id`         | List of [operation ids](#operation-id-lookup)
`funders`               | List of [funder ids](#funder-id-lookup)
`hasGrant`              | Boolean of whether charities have received public grant or not
`grantDateRange`        | List of the form `[YYYY-MM-DD, YYYY-MM-DD]`


## Aggregate Charities

This endpoint is currently private.

## Download Charities

This endpoint is currently private.


# Schema

Please see [charity-base-schema](https://github.com/charity-base/charity-base-schema)

# Filter ID Lookups

## Cause ID Lookup

> JSON Causes

```json
[
  { "id": 101, "name": "General charitable purposes" },
  { "id": 102, "name": "Education/training" },
  { "id": 103, "name": "The advancement of health or saving of lives" },
  { "id": 104, "name": "Disability" },
  { "id": 105, "name": "The prevention or relief of poverty" },
  { "id": 106, "name": "Overseas aid/famine relief" },
  { "id": 107, "name": "Accommodation/housing" },
  { "id": 108, "name": "Religious activities" },
  { "id": 109, "name": "Arts/culture/heritage/science" },
  { "id": 110, "name": "Amateur sport" },
  { "id": 111, "name": "Animals" },
  { "id": 112, "name": "Environment/conservation/heritage" },
  { "id": 113, "name": "Economic/community development/employment" },
  { "id": 114, "name": "Armed forces/emergency service efficiency" },
  { "id": 115, "name": "Human rights/religious or racial harmony/equality or diversity" },
  { "id": 116, "name": "Recreation" },
  { "id": 117, "name": "Other charitable purposes" }
]
```

Cause ID | Cause Name
--------- | -----------
101 | General charitable purposes
102 | Education/training
103 | The advancement of health or saving of lives
104 | Disability
105 | The prevention or relief of poverty
106 | Overseas aid/famine relief
107 | Accommodation/housing
108 | Religious activities
109 | Arts/culture/heritage/science
110 | Amateur sport
111 | Animals
112 | Environment/conservation/heritage
113 | Economic/community development/employment
114 | Armed forces/emergency service efficiency
115 | Human rights/religious or racial harmony/equality or diversity
116 | Recreation
117 | Other charitable purposes

## Beneficiary ID Lookup

> JSON Beneficiaries

```json
[
  { "id": 201, "name": "Children/young people" },
  { "id": 202, "name": "Elderly/old people" },
  { "id": 203, "name": "People with disabilities" },
  { "id": 204, "name": "People of a particular ethnic or racial origin" },
  { "id": 205, "name": "Other charities or voluntary bodies" },
  { "id": 206, "name": "Other defined groups" },
  { "id": 207, "name": "The general public/mankind" }
]
```

Beneficiary ID | Beneficiary Name
--------- | -----------
201 | Children/young people
202 | Elderly/old people
203 | People with disabilities
204 | People of a particular ethnic or racial origin
205 | Other charities or voluntary bodies
206 | Other defined groups
207 | The general public/mankind

## Operation ID Lookup

> JSON Operations

```json
[
  { "id": 301, "name": "Makes grants to individuals" },
  { "id": 302, "name": "Makes grants to organisations" },
  { "id": 303, "name": "Provides other finance" },
  { "id": 304, "name": "Provides human resources" },
  { "id": 305, "name": "Provides buildings/facilities/open space" },
  { "id": 306, "name": "Provides services" },
  { "id": 307, "name": "Provides advocacy/advice/information" },
  { "id": 308, "name": "Sponsors or undertakes research" },
  { "id": 309, "name": "Acts as an umbrella or resource body" },
  { "id": 310, "name": "Other charitable activities" }
]
```

Operation ID | Operation Name
--------- | -----------
301 | Makes grants to individuals
302 | Makes grants to organisations
303 | Provides other finance
304 | Provides human resources
305 | Provides buildings/facilities/open space
306 | Provides services
307 | Provides advocacy/advice/information
308 | Sponsors or undertakes research
309 | Acts as an umbrella or resource body
310 | Other charitable activities

## Funder ID Lookup

> JSON Funders

```json
[
  {
    "id": "GB-CHC-1000147",
    "name": "A B Charitable Trust"
  },
  {
    "id": "360G-ArcadiaFund",
    "name": "ARCADIA"
  },
  {
    "id": "GB-CHC-1115476",
    "name": "Barrow Cadbury Trust"
  },
  {
    "id": "GB-CHC-802052",
    "name": "BBC Children in Need"
  },
  {
    "id": "360G-blf",
    "name": "The Big Lottery Fund"
  },
  {
    "id": "GB-LAE-BIR",
    "name": "Birmingham City Council"
  },
  {
    "id": "GB-CHC-1164021",
    "name": "The Blagrave Trust"
  },
  {
    "id": "GB-CHC-1143711",
    "name": "Cheshire Community Foundation"
  },
  {
    "id": "GB-CHC-1035628",
    "name": "City Bridge Trust"
  },
  {
    "id": "GB-CHC-274100",
    "name": "The Clothworkers Foundation"
  },
  {
    "id": "GB-COH-IP00525R",
    "name": "Co-operative Group"
  },
  {
    "id": "GB-CHC-326568 ",
    "name": "Comic Relief"
  },
  {
    "id": "GB-CHC-1111600",
    "name": "Community Foundation for Surrey"
  },
  {
    "id": "GB-COH-02273708",
    "name": "Community Foundation serving Tyne & Wear and Northumberland"
  },
  {
    "id": "GB-CHC-309671",
    "name": "Culham St Gabriel\'s Trust"
  },
  {
    "id": "GB-CHC-1078217",
    "name": "The David & Elaine Potter Foundation"
  },
  {
    "id": "GB-GOR-D9",
    "name": "Department for Transport"
  },
  {
    "id": "GB-COH-07991677",
    "name": "The Dulverton Trust"
  },
  {
    "id": "GB-CHC-1140372",
    "name": "The Dunhill Medical Trust"
  },
  {
    "id": "GB-CHC-200051",
    "name": "Esmée Fairbairn Foundation"
  },
  {
    "id": "GB-CHC-1052061",
    "name": "Essex Community Foundation"
  },
  {
    "id": "GB-CHC-1123081",
    "name": "The Fore"
  },
  {
    "id": "GB-CHC-230260",
    "name": "Garfield Weston Foundation"
  },
  {
    "id": "GB-CHC-251988",
    "name": "Gatsby Charitable Foundation"
  },
  {
    "id": "GB-LAE-GLA",
    "name": "Greater London Authority"
  },
  {
    "id": "GB-CHC-1045304",
    "name": "Heart Of England Community Foundation"
  },
  {
    "id": "GB-CHC-230102",
    "name": "The Henry Smith Charity "
  },
  {
    "id": "GB-CHC-1075920",
    "name": "Indigo Trust"
  },
  {
    "id": "GB-CHC-253481",
    "name": "John Moores Foundation"
  },
  {
    "id": "GB-CHC-1093844",
    "name": "The Joseph Rank Trust"
  },
  {
    "id": "GB-CHC-210037",
    "name": "Joseph Rowntree Charitable Trust"
  },
  {
    "id": "GB-CHC-210169",
    "name": "Joseph Rowntree Foundation"
  },
  {
    "id": "GB-CHC-1111360",
    "name": "Kingston Voluntary Action"
  },
  {
    "id": "GB-CHC-295157",
    "name": "LandAid Charitable Trust"
  },
  {
    "id": "GB-CHC-1107583",
    "name": "Lankelly Chase Foundation"
  },
  {
    "id": "GB-CHC-327114",
    "name": "Lloyds Bank Foundation for England and Wales"
  },
  {
    "id": "GB-LAE-BNE",
    "name": "London Borough of Barnet"
  },
  {
    "id": "GB-LAE-SWK",
    "name": "London Borough of Southwark"
  },
  {
    "id": "GB-COH-03037449",
    "name": "London Councils"
  },
  {
    "id": "GB-CHC-1145921",
    "name": "Macc"
  },
  {
    "id": "GB-CHC-1164703",
    "name": "Masonic Charitable Foundation"
  },
  {
    "id": "GB-CHC-1158914",
    "name": "Millfield House Foundation"
  },
  {
    "id": "GB-GOR-D18",
    "name": "Ministry of Justice"
  },
  {
    "id": "GB-CHC-1065552",
    "name": "Nationwide Foundation"
  },
  {
    "id": "GB-CHC-1144091",
    "name": "Nesta"
  },
  {
    "id": "GB-COH-03416658",
    "name": "Northern Rock Foundation"
  },
  {
    "id": "GB-COH-RS007018",
    "name": "One Manchester"
  },
  {
    "id": "GB-LAE-OXO",
    "name": "Oxford City Council"
  },
  {
    "id": "GB-CHC-1151621",
    "name": "Oxfordshire Community Foundation"
  },
  {
    "id": "GB-CHC-1102927",
    "name": "Paul Hamlyn Foundation"
  },
  {
    "id": "GB-CHC-1009195",
    "name": "Pears Foundation"
  },
  {
    "id": "GB-CHC-1159982",
    "name": "Power to Change "
  },
  {
    "id": "GB-CHC-1080418",
    "name": "Quartet Community Foundation"
  },
  {
    "id": "GB-SC-SC012710",
    "name": "R S Macdonald Charitable Trust"
  },
  {
    "id": "GB-CHC-1156300",
    "name": "Road Safety Trust"
  },
  {
    "id": "GB-SC-SC002970",
    "name": "The Robertson Trust"
  },
  {
    "id": "GB-CHC-226446",
    "name": "Seafarers UK"
  },
  {
    "id": "GB-COH-04530979",
    "name": "Somerset Community Foundation"
  },
  {
    "id": "GB-COH-RC000766",
    "name": "Sport England"
  },
  {
    "id": "GB-CHC-1010656",
    "name": "Staples Trust"
  },
  {
    "id": "GB-CHC-1113226",
    "name": "Sussex Community Foundation"
  },
  {
    "id": "GB-CHC-328524",
    "name": "Tedworth Charitable Trust"
  },
  {
    "id": "GB-CHC-1059652",
    "name": "Three Guineas Trust"
  },
  {
    "id": "GB-COH-04831118",
    "name": "Trafford Housing Trust Social Investment"
  },
  {
    "id": "GB-LAE-TRF",
    "name": "Trafford Metropolitan Borough Council"
  },
  {
    "id": "GB-CHC-1089893",
    "name": "True Colours Trust"
  },
  {
    "id": "GB-CHC-1105580",
    "name": "The Tudor Trust"
  },
  {
    "id": "GB-CHC-1081124",
    "name": "Tuixen Foundation"
  },
  {
    "id": "GB-CHC-1166471",
    "name": "Two Ridings Community Foundation"
  },
  {
    "id": "GB-CHC-1103731",
    "name": "United St Saviour\'s Charity"
  },
  {
    "id": "GB-CHC-1161290",
    "name": "Virgin Money Foundation"
  },
  {
    "id": "GB-CHC-312800",
    "name": "Walcot Foundation"
  },
  {
    "id": "GB-CHC-1123126",
    "name": "Wiltshire Community Foundation"
  },
  {
    "id": "GB-CHC-1156077",
    "name": "Wolfson Foundation"
  },
  {
    "id": "GB-CHC-299963",
    "name": "Woodward Charitable Trust"
  },
  {
    "id": "GB-CHC-1133342",
    "name": "ZING"
  }
]
```

Funder ID | Funder Name
--------- | -----------
GB-CHC-1000147 | A B Charitable Trust
360G-ArcadiaFund | ARCADIA
GB-CHC-1115476 | Barrow Cadbury Trust
GB-CHC-802052 | BBC Children in Need
360G-blf | The Big Lottery Fund
GB-LAE-BIR | Birmingham City Council
GB-CHC-1164021 | The Blagrave Trust
GB-CHC-1143711 | Cheshire Community Foundation
GB-CHC-1035628 | City Bridge Trust
GB-CHC-274100 | The Clothworkers Foundation
GB-COH-IP00525R | Co-operative Group
GB-CHC-326568  | Comic Relief
GB-CHC-1111600 | Community Foundation for Surrey
GB-COH-02273708 | Community Foundation serving Tyne & Wear and Northumberland
GB-CHC-309671 | Culham St Gabriel\'s Trust
GB-CHC-1078217 | The David & Elaine Potter Foundation
GB-GOR-D9 | Department for Transport
GB-COH-07991677 | The Dulverton Trust
GB-CHC-1140372 | The Dunhill Medical Trust
GB-CHC-200051 | Esmée Fairbairn Foundation
GB-CHC-1052061 | Essex Community Foundation
GB-CHC-1123081 | The Fore
GB-CHC-230260 | Garfield Weston Foundation
GB-CHC-251988 | Gatsby Charitable Foundation
GB-LAE-GLA | Greater London Authority
GB-CHC-1045304 | Heart Of England Community Foundation
GB-CHC-230102 | The Henry Smith Charity 
GB-CHC-1075920 | Indigo Trust
GB-CHC-253481 | John Moores Foundation
GB-CHC-1093844 | The Joseph Rank Trust
GB-CHC-210037 | Joseph Rowntree Charitable Trust
GB-CHC-210169 | Joseph Rowntree Foundation
GB-CHC-1111360 | Kingston Voluntary Action
GB-CHC-295157 | LandAid Charitable Trust
GB-CHC-1107583 | Lankelly Chase Foundation
GB-CHC-327114 | Lloyds Bank Foundation for England and Wales
GB-LAE-BNE | London Borough of Barnet
GB-LAE-SWK | London Borough of Southwark
GB-COH-03037449 | London Councils
GB-CHC-1145921 | Macc
GB-CHC-1164703 | Masonic Charitable Foundation
GB-CHC-1158914 | Millfield House Foundation
GB-GOR-D18 | Ministry of Justice
GB-CHC-1065552 | Nationwide Foundation
GB-CHC-1144091 | Nesta
GB-COH-03416658 | Northern Rock Foundation
GB-COH-RS007018 | One Manchester
GB-LAE-OXO | Oxford City Council
GB-CHC-1151621 | Oxfordshire Community Foundation
GB-CHC-1102927 | Paul Hamlyn Foundation
GB-CHC-1009195 | Pears Foundation
GB-CHC-1159982 | Power to Change 
GB-CHC-1080418 | Quartet Community Foundation
GB-SC-SC012710 | R S Macdonald Charitable Trust
GB-CHC-1156300 | Road Safety Trust
GB-SC-SC002970 | The Robertson Trust
GB-CHC-226446 | Seafarers UK
GB-COH-04530979 | Somerset Community Foundation
GB-COH-RC000766 | Sport England
GB-CHC-1010656 | Staples Trust
GB-CHC-1113226 | Sussex Community Foundation
GB-CHC-328524 | Tedworth Charitable Trust
GB-CHC-1059652 | Three Guineas Trust
GB-COH-04831118 | Trafford Housing Trust Social Investment
GB-LAE-TRF | Trafford Metropolitan Borough Council
GB-CHC-1089893 | True Colours Trust
GB-CHC-1105580 | The Tudor Trust
GB-CHC-1081124 | Tuixen Foundation
GB-CHC-1166471 | Two Ridings Community Foundation
GB-CHC-1103731 | United St Saviour\'s Charity
GB-CHC-1161290 | Virgin Money Foundation
GB-CHC-312800 | Walcot Foundation
GB-CHC-1123126 | Wiltshire Community Foundation
GB-CHC-1156077 | Wolfson Foundation
GB-CHC-299963 | Woodward Charitable Trust
GB-CHC-1133342 | ZING

## Area of Operation ID Lookup

> JSON Areas of Operation

```json
[
  { "id": "A-1", "name": "Throughout England And Wales", ... },
  { "id": "A-2", "name": "Throughout England", ... },
  { "id": "A-3", "name": "Throughout Wales", ... },
  { "id": "A-4", "name": "Throughout London", ... },
  { "id": "B-1", "name": "Bracknell Forest", ... },
  { "id": "B-2", "name": "West Berkshire", ... },
  { "id": "B-3", "name": "Reading", ... },
  { "id": "B-4", "name": "Slough", ... },
  { "id": "B-5", "name": "Windsor And Maidenhead", ... },
  { "id": "B-6", "name": "Wokingham", ... },
  { "id": "B-7", "name": "Barking And Dagenham", ... },
  { "id": "B-8", "name": "Barnet", ... },
  { "id": "B-9", "name": "Bexley", ... },
  { "id": "B-10", "name": "Brent", ... },
  { "id": "B-11", "name": "Bromley", ... },
  { "id": "B-12", "name": "Camden", ... },
  { "id": "B-13", "name": "City Of London", ... },
  { "id": "B-14", "name": "City Of Westminster", ... },
  { "id": "B-15", "name": "Croydon", ... },
  { "id": "B-16", "name": "Ealing", ... },
  { "id": "B-17", "name": "Enfield", ... },
  { "id": "B-18", "name": "Greenwich", ... },
  { "id": "B-19", "name": "Hackney", ... },
  { "id": "B-20", "name": "Hammersmith And Fulham", ... },
  { "id": "B-21", "name": "Haringey", ... },
  { "id": "B-22", "name": "Harrow", ... },
  { "id": "B-23", "name": "Havering", ... },
  { "id": "B-24", "name": "Hillingdon", ... },
  { "id": "B-25", "name": "Hounslow", ... },
  { "id": "B-26", "name": "Islington", ... },
  { "id": "B-27", "name": "Kensington And Chelsea", ... },
  { "id": "B-28", "name": "Kingston Upon Thames", ... },
  { "id": "B-29", "name": "Lambeth", ... },
  { "id": "B-30", "name": "Lewisham", ... },
  { "id": "B-31", "name": "Merton", ... },
  { "id": "B-32", "name": "Newham", ... },
  { "id": "B-33", "name": "Redbridge", ... },
  { "id": "B-34", "name": "Richmond Upon Thames", ... },
  { "id": "B-35", "name": "Southwark", ... },
  { "id": "B-36", "name": "Sutton", ... },
  { "id": "B-37", "name": "Tower Hamlets", ... },
  { "id": "B-38", "name": "Waltham Forest", ... },
  { "id": "B-39", "name": "Wandsworth", ... },
  { "id": "B-40", "name": "Bolton", ... },
  { "id": "B-41", "name": "Bury", ... },
  { "id": "B-42", "name": "Manchester City", ... },
  { "id": "B-43", "name": "Oldham", ... },
  { "id": "B-44", "name": "Rochdale", ... },
  { "id": "B-45", "name": "Salford City", ... },
  { "id": "B-46", "name": "Stockport", ... },
  { "id": "B-47", "name": "Tameside", ... },
  { "id": "B-48", "name": "Trafford", ... },
  { "id": "B-49", "name": "Wigan", ... },
  { "id": "B-50", "name": "Knowsley", ... },
  { "id": "B-51", "name": "Liverpool City", ... },
  { "id": "B-52", "name": "Sefton", ... },
  { "id": "B-53", "name": "St Helens", ... },
  { "id": "B-54", "name": "Wirral", ... },
  { "id": "B-55", "name": "Barnsley", ... },
  { "id": "B-56", "name": "Doncaster", ... },
  { "id": "B-57", "name": "Rotherham", ... },
  { "id": "B-58", "name": "Sheffield City", ... },
  { "id": "B-59", "name": "Gateshead", ... },
  { "id": "B-60", "name": "Newcastle Upon Tyne City", ... },
  { "id": "B-61", "name": "North Tyneside", ... },
  { "id": "B-62", "name": "South Tyneside", ... },
  { "id": "B-63", "name": "Sunderland", ... },
  { "id": "B-64", "name": "Birmingham City", ... },
  { "id": "B-65", "name": "Coventry City", ... },
  { "id": "B-66", "name": "Dudley", ... },
  { "id": "B-67", "name": "Sandwell", ... },
  { "id": "B-68", "name": "Solihull", ... },
  { "id": "B-69", "name": "Walsall", ... },
  { "id": "B-70", "name": "Wolverhampton", ... },
  { "id": "B-71", "name": "Bradford City", ... },
  { "id": "B-72", "name": "Calderdale", ... },
  { "id": "B-73", "name": "Kirklees", ... },
  { "id": "B-74", "name": "Leeds City", ... },
  { "id": "B-75", "name": "City Of Wakefield", ... },
  { "id": "B-76", "name": "Nottingham City", ... },
  { "id": "B-101", "name": "Central Bedfordshire", ... },
  { "id": "B-102", "name": "Buckinghamshire", ... },
  { "id": "B-103", "name": "Cambridgeshire", ... },
  { "id": "B-104", "name": "Cheshire East", ... },
  { "id": "B-105", "name": "Cornwall", ... },
  { "id": "B-106", "name": "Cumbria", ... },
  { "id": "B-107", "name": "Derbyshire", ... },
  { "id": "B-108", "name": "Devon", ... },
  { "id": "B-109", "name": "Dorset", ... },
  { "id": "B-110", "name": "Durham", ... },
  { "id": "B-111", "name": "East Sussex", ... },
  { "id": "B-112", "name": "Essex", ... },
  { "id": "B-113", "name": "Gloucestershire", ... },
  { "id": "B-114", "name": "Hampshire", ... },
  { "id": "B-115", "name": "Hertfordshire", ... },
  { "id": "B-116", "name": "Isle Of Wight", ... },
  { "id": "B-117", "name": "Kent", ... },
  { "id": "B-118", "name": "Lancashire", ... },
  { "id": "B-119", "name": "Leicestershire", ... },
  { "id": "B-120", "name": "Lincolnshire", ... },
  { "id": "B-121", "name": "Norfolk", ... },
  { "id": "B-122", "name": "North Yorkshire", ... },
  { "id": "B-123", "name": "Northamptonshire", ... },
  { "id": "B-124", "name": "Northumberland", ... },
  { "id": "B-125", "name": "Nottinghamshire", ... },
  { "id": "B-126", "name": "Oxfordshire", ... },
  { "id": "B-127", "name": "Powys", ... },
  { "id": "B-128", "name": "Shropshire", ... },
  { "id": "B-129", "name": "Somerset", ... },
  { "id": "B-130", "name": "Staffordshire", ... },
  { "id": "B-131", "name": "Suffolk", ... },
  { "id": "B-132", "name": "Surrey", ... },
  { "id": "B-133", "name": "Warwickshire", ... },
  { "id": "B-134", "name": "West Sussex", ... },
  { "id": "B-135", "name": "Wiltshire", ... },
  { "id": "B-136", "name": "North Somerset", ... },
  { "id": "B-137", "name": "Bath And North East Somerset", ... },
  { "id": "B-138", "name": "South Gloucestershire", ... },
  { "id": "B-139", "name": "Bristol City", ... },
  { "id": "B-140", "name": "Hartlepool", ... },
  { "id": "B-141", "name": "Middlesbrough", ... },
  { "id": "B-142", "name": "Redcar And Cleveland", ... },
  { "id": "B-143", "name": "Stockton-On-Tees", ... },
  { "id": "B-144", "name": "Kingston Upon Hull City", ... },
  { "id": "B-145", "name": "North Lincolnshire", ... },
  { "id": "B-146", "name": "North East Lincolnshire", ... },
  { "id": "B-147", "name": "East Riding Of Yorkshire", ... },
  { "id": "B-148", "name": "Conwy", ... },
  { "id": "B-149", "name": "Isle Of Anglesey", ... },
  { "id": "B-150", "name": "Blaenau Gwent", ... },
  { "id": "B-151", "name": "Bridgend", ... },
  { "id": "B-152", "name": "Caerphilly", ... },
  { "id": "B-153", "name": "Cardiff", ... },
  { "id": "B-154", "name": "Ceredigion", ... },
  { "id": "B-155", "name": "Carmarthenshire", ... },
  { "id": "B-156", "name": "Denbighshire", ... },
  { "id": "B-157", "name": "Flintshire", ... },
  { "id": "B-158", "name": "Merthyr Tydfil", ... },
  { "id": "B-159", "name": "Monmouthshire", ... },
  { "id": "B-160", "name": "Neath Port Talbot", ... },
  { "id": "B-161", "name": "Newport City", ... },
  { "id": "B-162", "name": "Pembrokeshire", ... },
  { "id": "B-163", "name": "Plymouth City", ... },
  { "id": "B-164", "name": "City Of Swansea", ... },
  { "id": "B-165", "name": "Vale Of Glamorgan", ... },
  { "id": "B-166", "name": "Thurrock", ... },
  { "id": "B-167", "name": "Wrexham", ... },
  { "id": "B-168", "name": "Halton", ... },
  { "id": "B-169", "name": "Gwynedd", ... },
  { "id": "B-170", "name": "City Of York", ... },
  { "id": "B-171", "name": "Torfaen", ... },
  { "id": "B-172", "name": "Warrington", ... },
  { "id": "B-173", "name": "Rhondda Cynon Taff", ... },
  { "id": "B-174", "name": "Luton", ... },
  { "id": "B-175", "name": "Milton Keynes", ... },
  { "id": "B-176", "name": "Derby City", ... },
  { "id": "B-177", "name": "Bournemouth", ... },
  { "id": "B-178", "name": "Poole", ... },
  { "id": "B-179", "name": "Darlington", ... },
  { "id": "B-180", "name": "Brighton And Hove", ... },
  { "id": "B-181", "name": "Southampton City", ... },
  { "id": "B-182", "name": "Portsmouth City", ... },
  { "id": "B-183", "name": "Leicester City", ... },
  { "id": "B-184", "name": "Rutland", ... },
  { "id": "B-185", "name": "Stoke-On-Trent City", ... },
  { "id": "B-186", "name": "Swindon", ... },
  { "id": "B-187", "name": "Blackburn With Darwen", ... },
  { "id": "B-188", "name": "Blackpool", ... },
  { "id": "B-189", "name": "Southend-On-Sea", ... },
  { "id": "B-190", "name": "Telford & Wrekin", ... },
  { "id": "B-191", "name": "Peterborough City", ... },
  { "id": "B-192", "name": "Herefordshire", ... },
  { "id": "B-193", "name": "Worcestershire", ... },
  { "id": "B-194", "name": "Medway", ... },
  { "id": "B-195", "name": "Isles Of Scilly", ... },
  { "id": "B-196", "name": "Torbay", ... },
  { "id": "B-197", "name": "Cheshire West & Chester", ... },
  { "id": "B-198", "name": "Bedford", ... },
  { "id": "D-1", "name": "Afghanistan", ... },
  { "id": "D-2", "name": "Albania", ... },
  { "id": "D-3", "name": "Algeria", ... },
  { "id": "D-5", "name": "Andorra", ... },
  { "id": "D-6", "name": "Angola", ... },
  { "id": "D-9", "name": "Antigua And Barbuda", ... },
  { "id": "D-10", "name": "Argentina", ... },
  { "id": "D-11", "name": "Armenia", ... },
  { "id": "D-13", "name": "Australia", ... },
  { "id": "D-14", "name": "Austria", ... },
  { "id": "D-15", "name": "Azerbaijan", ... },
  { "id": "D-17", "name": "Bahamas", ... },
  { "id": "D-18", "name": "Bahrain", ... },
  { "id": "D-19", "name": "Bangladesh", ... },
  { "id": "D-20", "name": "Barbados", ... },
  { "id": "D-21", "name": "Belarus", ... },
  { "id": "D-22", "name": "Belgium", ... },
  { "id": "D-23", "name": "Belize", ... },
  { "id": "D-24", "name": "Benin", ... },
  { "id": "D-26", "name": "Bhutan", ... },
  { "id": "D-27", "name": "Bolivia", ... },
  { "id": "D-28", "name": "Bosnia And Herzegovina", ... },
  { "id": "D-29", "name": "Botswana", ... },
  { "id": "D-30", "name": "Brazil", ... },
  { "id": "D-31", "name": "Brunei", ... },
  { "id": "D-32", "name": "Bulgaria", ... },
  { "id": "D-33", "name": "Burkina Faso", ... },
  { "id": "D-34", "name": "Burundi", ... },
  { "id": "D-35", "name": "Cambodia", ... },
  { "id": "D-36", "name": "Cameroon", ... },
  { "id": "D-37", "name": "Canada", ... },
  { "id": "D-38", "name": "Cape Verde", ... },
  { "id": "D-40", "name": "Central African Republic", ... },
  { "id": "D-41", "name": "Chad", ... },
  { "id": "D-42", "name": "Chile", ... },
  { "id": "D-43", "name": "China", ... },
  { "id": "D-44", "name": "Colombia", ... },
  { "id": "D-45", "name": "Comoros", ... },
  { "id": "D-46", "name": "Congo", ... },
  { "id": "D-48", "name": "Costa Rica", ... },
  { "id": "D-49", "name": "Croatia", ... },
  { "id": "D-50", "name": "Cuba", ... },
  { "id": "D-51", "name": "Cyprus", ... },
  { "id": "D-52", "name": "Czech Republic", ... },
  { "id": "D-53", "name": "Denmark", ... },
  { "id": "D-54", "name": "Djibouti", ... },
  { "id": "D-55", "name": "Dominica", ... },
  { "id": "D-56", "name": "Dominican Republic", ... },
  { "id": "D-57", "name": "Ecuador", ... },
  { "id": "D-58", "name": "Egypt", ... },
  { "id": "D-59", "name": "El Salvador", ... },
  { "id": "D-60", "name": "Equatorial Guinea", ... },
  { "id": "D-61", "name": "Eritrea", ... },
  { "id": "D-62", "name": "Estonia", ... },
  { "id": "D-63", "name": "Ethiopia", ... },
  { "id": "D-66", "name": "Fiji", ... },
  { "id": "D-67", "name": "Finland", ... },
  { "id": "D-68", "name": "France", ... },
  { "id": "D-71", "name": "Gabon", ... },
  { "id": "D-72", "name": "The Gambia", ... },
  { "id": "D-74", "name": "Georgia", ... },
  { "id": "D-75", "name": "Germany", ... },
  { "id": "D-76", "name": "Ghana", ... },
  { "id": "D-78", "name": "Greece", ... },
  { "id": "D-80", "name": "Grenada", ... },
  { "id": "D-83", "name": "Guatemala", ... },
  { "id": "D-85", "name": "Guinea", ... },
  { "id": "D-86", "name": "Guinea-Bissau", ... },
  { "id": "D-87", "name": "Guyana", ... },
  { "id": "D-88", "name": "Haiti", ... },
  { "id": "D-89", "name": "Honduras", ... },
  { "id": "D-91", "name": "Hungary", ... },
  { "id": "D-92", "name": "Iceland", ... },
  { "id": "D-93", "name": "India", ... },
  { "id": "D-94", "name": "Indonesia", ... },
  { "id": "D-95", "name": "Iran", ... },
  { "id": "D-96", "name": "Iraq", ... },
  { "id": "D-97", "name": "Israel", ... },
  { "id": "D-98", "name": "Italy", ... },
  { "id": "D-99", "name": "Ivory Coast", ... },
  { "id": "D-100", "name": "Jamaica", ... },
  { "id": "D-101", "name": "Japan", ... },
  { "id": "D-103", "name": "Jordan", ... },
  { "id": "D-104", "name": "Kazakhstan", ... },
  { "id": "D-105", "name": "Kenya", ... },
  { "id": "D-106", "name": "Kiribati", ... },
  { "id": "D-107", "name": "Kuwait", ... },
  { "id": "D-108", "name": "Kyrgyzstan", ... },
  { "id": "D-109", "name": "Laos", ... },
  { "id": "D-110", "name": "Latvia", ... },
  { "id": "D-111", "name": "Lebanon", ... },
  { "id": "D-112", "name": "Lesotho", ... },
  { "id": "D-113", "name": "Liberia", ... },
  { "id": "D-114", "name": "Libya", ... },
  { "id": "D-115", "name": "Liechtenstein", ... },
  { "id": "D-116", "name": "Lithuania", ... },
  { "id": "D-117", "name": "Luxembourg", ... },
  { "id": "D-119", "name": "Macedonia", ... },
  { "id": "D-120", "name": "Madagascar", ... },
  { "id": "D-122", "name": "Malawi", ... },
  { "id": "D-123", "name": "Malaysia", ... },
  { "id": "D-124", "name": "Maldives", ... },
  { "id": "D-125", "name": "Mali", ... },
  { "id": "D-126", "name": "Malta", ... },
  { "id": "D-127", "name": "Marshall Islands", ... },
  { "id": "D-129", "name": "Mauritania", ... },
  { "id": "D-130", "name": "Mauritius", ... },
  { "id": "D-132", "name": "Mexico", ... },
  { "id": "D-133", "name": "Micronesia", ... },
  { "id": "D-134", "name": "Moldova", ... },
  { "id": "D-135", "name": "Monaco", ... },
  { "id": "D-136", "name": "Mongolia", ... },
  { "id": "D-138", "name": "Morocco", ... },
  { "id": "D-139", "name": "Mozambique", ... },
  { "id": "D-140", "name": "Burma", ... },
  { "id": "D-141", "name": "Namibia", ... },
  { "id": "D-142", "name": "Nauru", ... },
  { "id": "D-143", "name": "Nepal", ... },
  { "id": "D-144", "name": "Netherlands", ... },
  { "id": "D-147", "name": "New Zealand", ... },
  { "id": "D-148", "name": "Nicaragua", ... },
  { "id": "D-149", "name": "Niger", ... },
  { "id": "D-150", "name": "Nigeria", ... },
  { "id": "D-152", "name": "North Korea", ... },
  { "id": "D-154", "name": "Norway", ... },
  { "id": "D-155", "name": "Oman", ... },
  { "id": "D-156", "name": "Pakistan", ... },
  { "id": "D-157", "name": "Palau", ... },
  { "id": "D-159", "name": "Panama", ... },
  { "id": "D-160", "name": "Papua New Guinea", ... },
  { "id": "D-161", "name": "Paraguay", ... },
  { "id": "D-162", "name": "Peru", ... },
  { "id": "D-163", "name": "Philippines", ... },
  { "id": "D-164", "name": "Poland", ... },
  { "id": "D-165", "name": "Portugal", ... },
  { "id": "D-167", "name": "Qatar", ... },
  { "id": "D-168", "name": "Ireland", ... },
  { "id": "D-170", "name": "Romania", ... },
  { "id": "D-171", "name": "Russia", ... },
  { "id": "D-172", "name": "Rwanda", ... },
  { "id": "D-174", "name": "St Kitts-Nevis", ... },
  { "id": "D-175", "name": "St Lucia", ... },
  { "id": "D-176", "name": "St Vincent And Grenadines", ... },
  { "id": "D-177", "name": "Samoa", ... },
  { "id": "D-178", "name": "San Marino", ... },
  { "id": "D-179", "name": "Sao Tome And Principe", ... },
  { "id": "D-180", "name": "Saudi Arabia", ... },
  { "id": "D-181", "name": "Senegal", ... },
  { "id": "D-182", "name": "Seychelles", ... },
  { "id": "D-183", "name": "Sierra Leone", ... },
  { "id": "D-184", "name": "Singapore", ... },
  { "id": "D-185", "name": "Slovakia", ... },
  { "id": "D-186", "name": "Slovenia", ... },
  { "id": "D-187", "name": "Solomon Islands", ... },
  { "id": "D-188", "name": "Somalia", ... },
  { "id": "D-189", "name": "South Africa", ... },
  { "id": "D-190", "name": "South Korea", ... },
  { "id": "D-191", "name": "Spain", ... },
  { "id": "D-192", "name": "Sri Lanka", ... },
  { "id": "D-193", "name": "Sudan", ... },
  { "id": "D-194", "name": "Surinam", ... },
  { "id": "D-195", "name": "Swaziland", ... },
  { "id": "D-196", "name": "Sweden", ... },
  { "id": "D-197", "name": "Switzerland", ... },
  { "id": "D-198", "name": "Syria", ... },
  { "id": "D-200", "name": "Tajikistan", ... },
  { "id": "D-201", "name": "Tanzania", ... },
  { "id": "D-202", "name": "Thailand", ... },
  { "id": "D-204", "name": "Togo", ... },
  { "id": "D-205", "name": "Tonga", ... },
  { "id": "D-206", "name": "Trinidad And Tobago", ... },
  { "id": "D-207", "name": "Tunisia", ... },
  { "id": "D-208", "name": "Turkey", ... },
  { "id": "D-209", "name": "Turkmenistan", ... },
  { "id": "D-211", "name": "Tuvalu", ... },
  { "id": "D-212", "name": "Uganda", ... },
  { "id": "D-213", "name": "Ukraine", ... },
  { "id": "D-214", "name": "United Arab Emirates", ... },
  { "id": "D-216", "name": "United States", ... },
  { "id": "D-217", "name": "Uruguay", ... },
  { "id": "D-218", "name": "Uzbekistan", ... },
  { "id": "D-219", "name": "Vanuatu", ... },
  { "id": "D-221", "name": "Venezuela", ... },
  { "id": "D-222", "name": "Vietnam", ... },
  { "id": "D-224", "name": "Yemen", ... },
  { "id": "D-225", "name": "Serbia", ... },
  { "id": "D-226", "name": "Democratic Republic Of The Congo", ... },
  { "id": "D-227", "name": "Zambia", ... },
  { "id": "D-228", "name": "Zimbabwe", ... },
  { "id": "D-233", "name": "Montenegro", ... },
  { "id": "D-234", "name": "Scotland", ... },
  { "id": "D-235", "name": "Northern Ireland", ... },
  { "id": "D-236", "name": "Antarctica", ... },
  { "id": "D-237", "name": "Timor-Leste", ... },
  { "id": "D-238", "name": "Occupied Palestinian Territories", ... },
  { "id": "D-239", "name": "Kosovo", ... },
  { "id": "D-240", "name": "Akrotiri And Dhekelia", ... },
  { "id": "D-241", "name": "American Samoa", ... },
  { "id": "D-242", "name": "Anguilla", ... },
  { "id": "D-243", "name": "Aruba", ... },
  { "id": "D-244", "name": "Bermuda", ... },
  { "id": "D-245", "name": "British Indian Ocean Territory", ... },
  { "id": "D-246", "name": "British Virgin Islands", ... },
  { "id": "D-247", "name": "Cayman Islands", ... },
  { "id": "D-248", "name": "Christmas Island", ... },
  { "id": "D-249", "name": "Clipperton Island", ... },
  { "id": "D-250", "name": "Cocos (Keeling) Islands", ... },
  { "id": "D-251", "name": "Cook Islands", ... },
  { "id": "D-252", "name": "Easter  Island", ... },
  { "id": "D-253", "name": "Falkland Islands", ... },
  { "id": "D-254", "name": "Faroe Islands", ... },
  { "id": "D-255", "name": "French Guyana", ... },
  { "id": "D-256", "name": "French Polynesia", ... },
  { "id": "D-257", "name": "Gibraltar", ... },
  { "id": "D-258", "name": "Greenland", ... },
  { "id": "D-259", "name": "Guadeloupe", ... },
  { "id": "D-260", "name": "Guam", ... },
  { "id": "D-261", "name": "Guernsey", ... },
  { "id": "D-262", "name": "Hong Kong", ... },
  { "id": "D-263", "name": "Isle Of Man", ... },
  { "id": "D-264", "name": "Jan Mayen", ... },
  { "id": "D-265", "name": "Jersey", ... },
  { "id": "D-266", "name": "Juan Fernandez Islands", ... },
  { "id": "D-267", "name": "Macau", ... },
  { "id": "D-268", "name": "Martinique", ... },
  { "id": "D-269", "name": "Mayotte", ... },
  { "id": "D-270", "name": "Montserrat", ... },
  { "id": "D-271", "name": "Netherlands Antilles", ... },
  { "id": "D-272", "name": "New Caledonia", ... },
  { "id": "D-273", "name": "Niue", ... },
  { "id": "D-274", "name": "Norfolk Island", ... },
  { "id": "D-275", "name": "Northern Mariana Island", ... },
  { "id": "D-276", "name": "Pitcairn Islands", ... },
  { "id": "D-277", "name": "Puerto Rico", ... },
  { "id": "D-278", "name": "Reunion", ... },
  { "id": "D-279", "name": "Ross Dependency", ... },
  { "id": "D-280", "name": "St Barthelemy", ... },
  { "id": "D-281", "name": "St Helena", ... },
  { "id": "D-282", "name": "St Martin", ... },
  { "id": "D-283", "name": "St Pierre And Miquelon", ... },
  { "id": "D-284", "name": "South Georgia", ... },
  { "id": "D-285", "name": "Svalbard", ... },
  { "id": "D-286", "name": "Taiwan", ... },
  { "id": "D-287", "name": "Tokelau", ... },
  { "id": "D-288", "name": "Turks And Caicos Islands", ... },
  { "id": "D-289", "name": "U.S. Virgin Islands", ... },
  { "id": "D-290", "name": "Wallis And Futuna", ... },
  { "id": "D-293", "name": "Republic Of South Sudan" }
]
```

### UK Division

Area ID | Area Name
--------- | -----------
A-1 | Throughout England And Wales
A-2 | Throughout England
A-3 | Throughout Wales
A-4 | Throughout London

### Local Authorities

Area ID | Area Name
--------- | -----------
B-1 | Bracknell Forest
B-2 | West Berkshire
B-3 | Reading
B-4 | Slough
B-5 | Windsor And Maidenhead
B-6 | Wokingham
B-7 | Barking And Dagenham
B-8 | Barnet
B-9 | Bexley
B-10 | Brent
B-11 | Bromley
B-12 | Camden
B-13 | City Of London
B-14 | City Of Westminster
B-15 | Croydon
B-16 | Ealing
B-17 | Enfield
B-18 | Greenwich
B-19 | Hackney
B-20 | Hammersmith And Fulham
B-21 | Haringey
B-22 | Harrow
B-23 | Havering
B-24 | Hillingdon
B-25 | Hounslow
B-26 | Islington
B-27 | Kensington And Chelsea
B-28 | Kingston Upon Thames
B-29 | Lambeth
B-30 | Lewisham
B-31 | Merton
B-32 | Newham
B-33 | Redbridge
B-34 | Richmond Upon Thames
B-35 | Southwark
B-36 | Sutton
B-37 | Tower Hamlets
B-38 | Waltham Forest
B-39 | Wandsworth
B-40 | Bolton
B-41 | Bury
B-42 | Manchester City
B-43 | Oldham
B-44 | Rochdale
B-45 | Salford City
B-46 | Stockport
B-47 | Tameside
B-48 | Trafford
B-49 | Wigan
B-50 | Knowsley
B-51 | Liverpool City
B-52 | Sefton
B-53 | St Helens
B-54 | Wirral
B-55 | Barnsley
B-56 | Doncaster
B-57 | Rotherham
B-58 | Sheffield City
B-59 | Gateshead
B-60 | Newcastle Upon Tyne City
B-61 | North Tyneside
B-62 | South Tyneside
B-63 | Sunderland
B-64 | Birmingham City
B-65 | Coventry City
B-66 | Dudley
B-67 | Sandwell
B-68 | Solihull
B-69 | Walsall
B-70 | Wolverhampton
B-71 | Bradford City
B-72 | Calderdale
B-73 | Kirklees
B-74 | Leeds City
B-75 | City Of Wakefield
B-76 | Nottingham City
B-101 | Central Bedfordshire
B-102 | Buckinghamshire
B-103 | Cambridgeshire
B-104 | Cheshire East
B-105 | Cornwall
B-106 | Cumbria
B-107 | Derbyshire
B-108 | Devon
B-109 | Dorset
B-110 | Durham
B-111 | East Sussex
B-112 | Essex
B-113 | Gloucestershire
B-114 | Hampshire
B-115 | Hertfordshire
B-116 | Isle Of Wight
B-117 | Kent
B-118 | Lancashire
B-119 | Leicestershire
B-120 | Lincolnshire
B-121 | Norfolk
B-122 | North Yorkshire
B-123 | Northamptonshire
B-124 | Northumberland
B-125 | Nottinghamshire
B-126 | Oxfordshire
B-127 | Powys
B-128 | Shropshire
B-129 | Somerset
B-130 | Staffordshire
B-131 | Suffolk
B-132 | Surrey
B-133 | Warwickshire
B-134 | West Sussex
B-135 | Wiltshire
B-136 | North Somerset
B-137 | Bath And North East Somerset
B-138 | South Gloucestershire
B-139 | Bristol City
B-140 | Hartlepool
B-141 | Middlesbrough
B-142 | Redcar And Cleveland
B-143 | Stockton-On-Tees
B-144 | Kingston Upon Hull City
B-145 | North Lincolnshire
B-146 | North East Lincolnshire
B-147 | East Riding Of Yorkshire
B-148 | Conwy
B-149 | Isle Of Anglesey
B-150 | Blaenau Gwent
B-151 | Bridgend
B-152 | Caerphilly
B-153 | Cardiff
B-154 | Ceredigion
B-155 | Carmarthenshire
B-156 | Denbighshire
B-157 | Flintshire
B-158 | Merthyr Tydfil
B-159 | Monmouthshire
B-160 | Neath Port Talbot
B-161 | Newport City
B-162 | Pembrokeshire
B-163 | Plymouth City
B-164 | City Of Swansea
B-165 | Vale Of Glamorgan
B-166 | Thurrock
B-167 | Wrexham
B-168 | Halton
B-169 | Gwynedd
B-170 | City Of York
B-171 | Torfaen
B-172 | Warrington
B-173 | Rhondda Cynon Taff
B-174 | Luton
B-175 | Milton Keynes
B-176 | Derby City
B-177 | Bournemouth
B-178 | Poole
B-179 | Darlington
B-180 | Brighton And Hove
B-181 | Southampton City
B-182 | Portsmouth City
B-183 | Leicester City
B-184 | Rutland
B-185 | Stoke-On-Trent City
B-186 | Swindon
B-187 | Blackburn With Darwen
B-188 | Blackpool
B-189 | Southend-On-Sea
B-190 | Telford & Wrekin
B-191 | Peterborough City
B-192 | Herefordshire
B-193 | Worcestershire
B-194 | Medway
B-195 | Isles Of Scilly
B-196 | Torbay
B-197 | Cheshire West & Chester
B-198 | Bedford


### Countries

Area ID | Area Name
--------- | -----------
D-1 | Afghanistan
D-2 | Albania
D-3 | Algeria
D-5 | Andorra
D-6 | Angola
D-9 | Antigua And Barbuda
D-10 | Argentina
D-11 | Armenia
D-13 | Australia
D-14 | Austria
D-15 | Azerbaijan
D-17 | Bahamas
D-18 | Bahrain
D-19 | Bangladesh
D-20 | Barbados
D-21 | Belarus
D-22 | Belgium
D-23 | Belize
D-24 | Benin
D-26 | Bhutan
D-27 | Bolivia
D-28 | Bosnia And Herzegovina
D-29 | Botswana
D-30 | Brazil
D-31 | Brunei
D-32 | Bulgaria
D-33 | Burkina Faso
D-34 | Burundi
D-35 | Cambodia
D-36 | Cameroon
D-37 | Canada
D-38 | Cape Verde
D-40 | Central African Republic
D-41 | Chad
D-42 | Chile
D-43 | China
D-44 | Colombia
D-45 | Comoros
D-46 | Congo
D-48 | Costa Rica
D-49 | Croatia
D-50 | Cuba
D-51 | Cyprus
D-52 | Czech Republic
D-53 | Denmark
D-54 | Djibouti
D-55 | Dominica
D-56 | Dominican Republic
D-57 | Ecuador
D-58 | Egypt
D-59 | El Salvador
D-60 | Equatorial Guinea
D-61 | Eritrea
D-62 | Estonia
D-63 | Ethiopia
D-66 | Fiji
D-67 | Finland
D-68 | France
D-71 | Gabon
D-72 | The Gambia
D-74 | Georgia
D-75 | Germany
D-76 | Ghana
D-78 | Greece
D-80 | Grenada
D-83 | Guatemala
D-85 | Guinea
D-86 | Guinea-Bissau
D-87 | Guyana
D-88 | Haiti
D-89 | Honduras
D-91 | Hungary
D-92 | Iceland
D-93 | India
D-94 | Indonesia
D-95 | Iran
D-96 | Iraq
D-97 | Israel
D-98 | Italy
D-99 | Ivory Coast
D-100 | Jamaica
D-101 | Japan
D-103 | Jordan
D-104 | Kazakhstan
D-105 | Kenya
D-106 | Kiribati
D-107 | Kuwait
D-108 | Kyrgyzstan
D-109 | Laos
D-110 | Latvia
D-111 | Lebanon
D-112 | Lesotho
D-113 | Liberia
D-114 | Libya
D-115 | Liechtenstein
D-116 | Lithuania
D-117 | Luxembourg
D-119 | Macedonia
D-120 | Madagascar
D-122 | Malawi
D-123 | Malaysia
D-124 | Maldives
D-125 | Mali
D-126 | Malta
D-127 | Marshall Islands
D-129 | Mauritania
D-130 | Mauritius
D-132 | Mexico
D-133 | Micronesia
D-134 | Moldova
D-135 | Monaco
D-136 | Mongolia
D-138 | Morocco
D-139 | Mozambique
D-140 | Burma
D-141 | Namibia
D-142 | Nauru
D-143 | Nepal
D-144 | Netherlands
D-147 | New Zealand
D-148 | Nicaragua
D-149 | Niger
D-150 | Nigeria
D-152 | North Korea
D-154 | Norway
D-155 | Oman
D-156 | Pakistan
D-157 | Palau
D-159 | Panama
D-160 | Papua New Guinea
D-161 | Paraguay
D-162 | Peru
D-163 | Philippines
D-164 | Poland
D-165 | Portugal
D-167 | Qatar
D-168 | Ireland
D-170 | Romania
D-171 | Russia
D-172 | Rwanda
D-174 | St Kitts-Nevis
D-175 | St Lucia
D-176 | St Vincent And Grenadines
D-177 | Samoa
D-178 | San Marino
D-179 | Sao Tome And Principe
D-180 | Saudi Arabia
D-181 | Senegal
D-182 | Seychelles
D-183 | Sierra Leone
D-184 | Singapore
D-185 | Slovakia
D-186 | Slovenia
D-187 | Solomon Islands
D-188 | Somalia
D-189 | South Africa
D-190 | South Korea
D-191 | Spain
D-192 | Sri Lanka
D-193 | Sudan
D-194 | Surinam
D-195 | Swaziland
D-196 | Sweden
D-197 | Switzerland
D-198 | Syria
D-200 | Tajikistan
D-201 | Tanzania
D-202 | Thailand
D-204 | Togo
D-205 | Tonga
D-206 | Trinidad And Tobago
D-207 | Tunisia
D-208 | Turkey
D-209 | Turkmenistan
D-211 | Tuvalu
D-212 | Uganda
D-213 | Ukraine
D-214 | United Arab Emirates
D-216 | United States
D-217 | Uruguay
D-218 | Uzbekistan
D-219 | Vanuatu
D-221 | Venezuela
D-222 | Vietnam
D-224 | Yemen
D-225 | Serbia
D-226 | Democratic Republic Of The Congo
D-227 | Zambia
D-228 | Zimbabwe
D-233 | Montenegro
D-234 | Scotland
D-235 | Northern Ireland
D-236 | Antarctica
D-237 | Timor-Leste
D-238 | Occupied Palestinian Territories
D-239 | Kosovo
D-240 | Akrotiri And Dhekelia
D-241 | American Samoa
D-242 | Anguilla
D-243 | Aruba
D-244 | Bermuda
D-245 | British Indian Ocean Territory
D-246 | British Virgin Islands
D-247 | Cayman Islands
D-248 | Christmas Island
D-249 | Clipperton Island
D-250 | Cocos (Keeling) Islands
D-251 | Cook Islands
D-252 | Easter  Island
D-253 | Falkland Islands
D-254 | Faroe Islands
D-255 | French Guyana
D-256 | French Polynesia
D-257 | Gibraltar
D-258 | Greenland
D-259 | Guadeloupe
D-260 | Guam
D-261 | Guernsey
D-262 | Hong Kong
D-263 | Isle Of Man
D-264 | Jan Mayen
D-265 | Jersey
D-266 | Juan Fernandez Islands
D-267 | Macau
D-268 | Martinique
D-269 | Mayotte
D-270 | Montserrat
D-271 | Netherlands Antilles
D-272 | New Caledonia
D-273 | Niue
D-274 | Norfolk Island
D-275 | Northern Mariana Island
D-276 | Pitcairn Islands
D-277 | Puerto Rico
D-278 | Reunion
D-279 | Ross Dependency
D-280 | St Barthelemy
D-281 | St Helena
D-282 | St Martin
D-283 | St Pierre And Miquelon
D-284 | South Georgia
D-285 | Svalbard
D-286 | Taiwan
D-287 | Tokelau
D-288 | Turks And Caicos Islands
D-289 | U.S. Virgin Islands
D-290 | Wallis And Futuna
D-293 | Republic Of South Sudan
