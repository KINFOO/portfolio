---
type: post
title: Testing with Rails
description: Diving into Rails test specifics
date: 2025-04-01T14:56:13+02:00
lastmod: 2025-04-01
showTableOfContents: true
tags:
  - continuous integration
  - integration test
  - rails
  - ruby
  - test
  - unit test
  - postgresql
---

Lately, I was refactoring authentication on a Rails application, a big
refactoring on a recent application. I found myself filling authentication form
several times by minute.

I decided to code a test to _save time on filling this form_.

## Run tests

First let's check if tests are configured.

```sh
$ rails test
/Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activerecord-8.0.2/lib/active_record/connection_adapters/postgresql_adapter.rb:69:in 'ActiveRecord::ConnectionAdapters::PostgreSQLAdapter.new_client': connection to server at "::1", port 5432 failed: Connection refused (ActiveRecord::ConnectionNotEstablished)
        Is the server running on that host and accepting TCP/IP connections?
connection to server at "127.0.0.1", port 5432 failed: Connection refused
        Is the server running on that host and accepting TCP/IP connections?
```

Surprise!

Tests look wired to database out of the box.

Reading documentation[^1], I get that it is in the spirit of _Rails testing_.
You're always end up finding yourself testing something with some model.

[^1]: [Rails' test documentation](https://guides.rubyonrails.org/testing.html)
[^2]: [Integration test](https://guides.rubyonrails.org/testing.html#implementing-an-integration-test)
[^3]: [Tests helpers](https://guides.rubyonrails.org/testing.html#test-helpers)
