# Authzed Ruby Client

[![Ruby Gems](https://img.shields.io/gem/v/authzed?include_prereleases)](https://rubygems.org/gems/authzed)
[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)
[![Build Status](https://github.com/authzed/authzed-rb/workflows/build/badge.svg)](https://github.com/authzed/authzed-rb/actions)
[![Mailing List](https://img.shields.io/badge/email-google%20groups-4285F4)](https://groups.google.com/g/authzed-oss)
[![Discord Server](https://img.shields.io/discord/844600078504951838?color=7289da&logo=discord "Discord Server")](https://discord.gg/jTysUaxXzM)
[![Twitter](https://img.shields.io/twitter/follow/authzed?color=%23179CF0&logo=twitter&style=flat-square)](https://twitter.com/authzed)

This repository houses the Ruby client library for Authzed.

[Authzed] is a database and service that stores, computes, and validates your application's permissions.

Developers create a schema that models their permissions requirements and use a client library, such as this one, to apply the schema to the database, insert data into the database, and query the data to efficiently check permissions in their applications.

Supported client API versions:
- [v1alpha1](https://docs.authzed.com/reference/api#authzedapiv1alpha1)
- [v0](https://docs.authzed.com/reference/api#authzedapiv0)

You can find more info on each API on the [Authzed API reference documentation].
Additionally, Protobuf API documentation can be found on the [Buf Registry Authzed API repository].

See [CONTRIBUTING.md] for instructions on how to contribute and perform common tasks like building the project and running tests.

[Authzed]: https://authzed.com
[Authzed API Reference documentation]: https://docs.authzed.com/reference/api
[Buf Registry Authzed API repository]: https://buf.build/authzed/api/docs/main
[CONTRIBUTING.md]: CONTRIBUTING.md

## Getting Started

We highly recommend following the **[Protecting Your First App]** guide to learn the latest best practice to integrate an application with Authzed.

If you're interested in examples of a specific version of the API, they can be found in their respective folders in the [examples directory].

[Protecting Your First App]: https://docs.authzed.com/guides/first-app
[examples directory]: /examples

## Basic Usage

### Installation

This project is packaged as the gem `authzed` on [Ruby Gems].

The command to install the library is:

```sh
gem install authzed
```

[Ruby Gems]: https://rubygems.org

### Initializing a client

In order to successfully connect, you will have to provide a [Bearer Token] with your own API Token from the [Authzed dashboard] in place of `t_your_token_here_1234567deadbeef` in the following example:

[Bearer Token]: https://datatracker.ietf.org/doc/html/rfc6750#section-2.1
[Authzed Dashboard]: https://app.authzed.com

```rb
require 'authzed'


client = Authzed::Api::V0::Client.new(
    target: 'grpc.authzed.com:443',
    interceptors: [Authzed::GrpcUtil::BearerToken.new(token: 't_your_token_here_1234567deadbeef')],
)
```

### Performing an API call

```rb
require 'authzed'

emilia = Authzed::Api::V0::User.for(namespace: 'blog/user', object_id: 'emilia')
read_first_post = Authzed::Api::V0::ObjectAndRelation.new(
    namespace: 'blog/post',
    object_id: '1',
    relation: 'read'
)

# Is Emilia in the set of users that can read post #1?
resp = client.acl_service.check(
  Authzed::Api::V0::CheckRequest.new(test_userset: read_first_post, user: emilia)
)
```
