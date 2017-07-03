---
title: CharityBase Docs

toc_footers:
  - <a href='https://charitybase.uk/api-portal'>Sign Up for an API Key</a>
  - <a href='https://github.com/tythe-org/charity-base-docs'>Edit on GitHub</a>

includes:
  - errors

search: true
---

# Introduction

CharityBase is a comprehensive database of UK charity data containing details on the activities, history, areas of operation and finances of 350,000 charities and their subsidiaries.

The bulk of the database is formed by combining [files](http://data.charitycommission.gov.uk/) published by the Charity Commission (released under [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/)) with additional fields included in their charity search websites ([legacy](http://apps.charitycommission.gov.uk/showcharity/registerofcharities/RegisterHomePage.aspx) and [current](http://beta.charitycommission.gov.uk/)). This data is then restructured, cleaned and supplemented with content scraped from other sources.  It's updated every month.

The purpose is to make the data accessible for every kind of user: researchers, analysts, donors, grant-makers, journalists and web developers. This site provides documentation on how to use the open source API to access the data. We're also working on a more human-friendly web search tool and CSV downloader, for those who don't get on with the API.

# Endpoint

> Successful JSON response (status 200):

```json
{
  "version": "v0.2.0",
  "totalMatches": null,
  "query": {
    ...
  },
  "charities": [{
    ...
  }]
}
```

The CharityBase API has one endpoint which returns a list of registered charities.

`GET https://charitybase.uk/api/v0.2.0/charities`

<aside class="notice">
Copy this URL into your browser to see a typical response.
</aside>

By specifying URL query string parameters you can:

* choose which properties are returned for each charity
* filter, order and page through the results

Usage of the URL parameters is explained in the following documentation.

# Projection

> Request each charity's logo, income, registration history and number of volunteers:

```shell
?fields=favicon,mainCharity.income,registration,beta.people.volunteers
```

The `charities` property of the response is a list of objects, each with the following properties: `charityNumber`, `subNumber`, `name` and `registered`.  Additional properties defined in the [schema](#schema), including nested properties, can be "projected" onto the results using the `fields` parameter.

<aside class="notice">
If the <code>search</code> filter parameter is used, the <code>score</code> of the text-matching will also be returned as a property on each charity.
</aside>

URL Parameter | Description
--------- | -----------
`fields` | Comma-separated list of properties.

# Filtering

> Registered, main charities with no reported income:

```shell
?registered=true&subNumber=0&!mainCharity.income
```

> De-registered subsidiaries of charity 207043 with "lecture" in the name:

```shell
?registered=false&subNumber>0&charityNumber=207043&search=lecture
```

The results returned can be constrained by specifying values of certain properties.  As well as the standard equality operator for URL parameters, some of these filter parameters can be constrained with other operators including `<`, `>` and `!`.

URL Parameter | Description
--------- | -----------
`search` | Search terms matched against all working names.
`charityNumber` | Identifier assigned by the Charity Commission.
`subNumber` | Subsidiary number (`0` for main charities).
`mainCharity.income` | Gross revenue.
`registered` | Current registration status (`true` or `false`).



# Ordering

> Find charities in descending order of income, then ascending charity number (with incomes included in results):

```shell
?sort=-mainCharity.income,charityNumber&fields=mainCharity.income
```

By default results are returned in order of ascending `charityNumber` and then `subNumber`.  An exeption is when the `search` filter is used in which case results are ordered by descending `score` i.e. most relevant first.

This behaviour can be overridden using the `sort` parameter, which expects a comma-separated list of fields.  Prefix fields with `-` to sort in descending order.

URL Parameter | Description
--------- | -----------
`sort` | Comma-separated list of properties.



# Pagination

## Counting Results

> Explicitly request the total results count:

```shell
?countResults
```

By default the value of `totalMatches` in the response is `null` because counting total matches can slow some queries considerably.

URL Parameter | Description
--------- | -----------
`countResults` | If included, total matches will be counted.


## Skip & Limit

> Return the third page of results with 30 charities per request:

```shell
?limit=30&skip=60
```

Set the number of charities returned for each request with the `limit` parameter.  Its default value is `10` and the maximum allowed value is `50`.

If there are more results than the limit value, you can page through them using `skip`.  It takes any positive integer but choose multiples of `limit` to get sequential pages.

URL Parameter | Description
--------- | -----------
`limit` | Number of results to return per request.
`skip` | Number of results to skip.


# Schema

```javascript
{
  charityNumber : Number,
  subNumber : Number,
  name : String,
  registered : Boolean,
  govDoc : String,
  areaOfBenefit : String,
  contact : {
    correspondant : String,
    phone : String,
    fax : String,
    address : [String],
    postcode : String
  },
  mainCharity : {
    companyNumber : String,
    trustees : Boolean,
    fyEnd : String,
    welsh : Boolean,
    incomeDate : Date,
    income : Number,
    groupType : String,
    email : String,
    website : String
  },
  accountSubmission : [{
    submitDate : Date,
    arno : String,
    fyEnd : String 
  }],
  returnSubmission : [{
    submitDate : String,
    arno : String
  }],
  areaOfOperation : [{
    aooType : { type : String, enum : ['A', 'B', 'D'] },
    aooKey : Number,
    welsh : Boolean,
    master : Number
  }],
  class : [Number],
  financial : [{
    fyStart : Date,
    fyEnd : Date,
    income : Number,
    spending : Number
  }],
  otherNames : [{
    name : String,
    nameId : Number,
  }],
  objects : [String],
  partB : [{
    arno : String,
    fyStart : Date,
    fyEnd : Date,
    income : {
      voluntary : Number,
      trading : Number,
      investment : Number,
      activities : Number,
      other : Number,
      total : Number,
      unknown : {
        inc_leg : Number,
        inc_end : Number
      }
    },
    spending : {
      voluntary : Number,
      trading : Number,
      activities : Number,
      governance : Number,
      invManagement : Number,
      other : Number,
      total : Number,
      unknown : {
        exp_grant : Number,
        exp_support : Number,
        exp_dep : Number
      }
    },
    assets : {
      fixed : {
        investment : Number,
        total : Number
      },
      current : {
        cash : Number,
        investment : Number,
        total : Number
      },
      pension : {
        total : Number
      },
      credit : {
        oneYear : Number,
        longTerm : Number,
        total : Number
      },
      net : Number,
      unknown : {
        invest_gain : Number,
        asset_gain : Number,
        pension_gain : Number,
        reserves : Number
      }
    },
    funds : {
      endowment : Number,
      restricted : Number,
      unrestricted : Number,
      total : Number
    },
    people : {
      employees : Number,
      volunteers : Number
    },
    consolidated : Boolean,
    charityOnly : Boolean
  }],
  registration : [{
    regDate : Date,
    remDate : Date,
    remCode : String
  }],
  trustees : [String],
  beta : {
    activities : String,
    people : {
      trustees : Number,
      employees : Number,
      volunteers : Number
    }
  },
  favicon: Buffer,
  geo: {
    address : {
      type : {
        type: String,
        required: true,
        enum: ['Point'],
        default: 'Point',
      },
      coordinates: [Number],
    }
  },
}
```

This is the v0.2.0 schema.  There are over 100 properties (including nested properties) available for each charity registered in England & Wales.  It's due to be refactored into a more intuitive structure (and documented) which will help with the importing of Scottish & Northern Irish charities and allow for more powerful search features.
