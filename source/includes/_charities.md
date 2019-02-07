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
const CharityBaseClient = require('charity-base')

const charityBase = new CharityBaseClient({
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
const CharityBaseClient = require('charity-base')

const charityBase = new CharityBaseClient({
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