# CharityBase v4 Docs

Docs site for the deprecated CharityBase v4 REST API.

**The v4 API will stop working in Q3 2019.**  Please migrate to the GraphQL API, documented at [charitybase.uk/api-portal](https://charitybase.uk/api-portal).


Docs Development
------------------------------

This section is only useful for people developing the documentation (e.g. making a correction to submit by pull request).

### Prerequisites

You're going to need:

 - **Linux or macOS** — Windows may work, but is unsupported.
 - **Ruby, version 2.3.1 or newer**
 - **Bundler** — If Ruby is already installed, but the `bundle` command doesn't work, just run `gem install bundler` in a terminal.

### Getting Set Up

```shell
# Run locally:
bundle install
bundle exec middleman server

# Or with vagrant:
vagrant up
```

You can now see the docs at http://localhost:4567


### Deployment

Create a production build:

```shell
bundle exec middleman build --clean
```

Deploy `build` with Now:

```shell
now && now alias
```


Thanks
--------------------

These docs were generated using [Slate](https://github.com/lord/slate).
