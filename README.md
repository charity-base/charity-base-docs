# CharityBase Docs

Source code for the [CharityBase API Documentation](https://charity-base.github.io/charity-base-docs/)

To use the API you'll need to generate a key from the [API Portal](https://charitybase.uk/api-portal)

The API itself is also open source: [charity-base-api](https://github.com/charity-base/charity-base-api)

Docs Development
------------------------------

This section is only useful for people developing the documentation (e.g. making a correction to submit by pull request).

### Prerequisites

You're going to need:

 - **Linux or macOS** — Windows may work, but is unsupported.
 - **Ruby, version 2.3.1 or newer**
 - **Bundler** — If Ruby is already installed, but the `bundle` command doesn't work, just run `gem install bundler` in a terminal.

### Getting Set Up

1. Fork this repository on GitHub.
2. Clone *your forked repository* (not our original one) to your hard drive with `git clone https://github.com/YOURUSERNAME/charity-base-docs.git`
3. `cd charity-base-docs`
4. Initialize and start charity-base-docs. You can either do this locally, or with Vagrant:

```shell
# either run this to run locally
bundle install
bundle exec middleman server

# OR run this to run with vagrant
vagrant up

# To build for production
bundle exec middleman build --clean
```

You can now see the docs at http://localhost:4567

Thanks
--------------------

These docs were generated using [Slate](https://github.com/lord/slate).
