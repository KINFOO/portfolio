---
type: post
title: Rails credentials, great but ouch!
description: Diving into Rails test specifics
date: 2025-04-01T14:56:13+02:00
lastmod: 2025-04-04
showTableOfContents: true
tags:
  - rails
  - ruby
  - documentation
draft: "true"
---

Using Rails builtin credentials, I nice and nasty surprises.
The guide is really slick, but is you want to enter use details: what a pain.
I mean to use value from credentials you go:
Let's say you have credentials looking like this:
```yaml
provider:
  api_key: hash
  api_secret: secret
  ```
  
You would access them like so:
```ruby
secret = Rails.application.credentials.dig(:provider, :api_key)
```
I later discovered that [dig](https://ruby-doc.org/3.4.1/Array.html#method-i-dig) is part of Ruby and is not specific to Rails. But even after reading documentation, I am still not sharp enough on the topic. Documentation provides examples:
```yaml
a = [:foo, [:bar, :baz, [:bat, :bam]]]
a.dig(1) # => [:bar, :baz, [:bat, :bam]]
a.dig(1, 2) # => [:bat, :bam]
```
But does not cover calling dig with`Symbol` as in `credentials`. 
```ruby
Rails.application.credentials.dig(:provider, :api_key)
```


with

```yaml
provider
  api_key: hash
  api_secret: secret
  ```
  Notice the missing `:`after `provider` 
  Now if you try to show credentials, it is fireworks
  ```sh
  $ rails credentials:show 
/Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activesupport-8.0.2/lib/active_support/encrypted_configuration.rb:123:in 'ActiveSupport::EncryptedConfiguration#deserialize': Invalid YAML in '/Users/kevin/Documents/finope/forgetheweb_website/config/credentials/development.yml.enc'. (ActiveSupport::EncryptedConfiguration::InvalidContentError)

  (/Users/kevin/Documents/finope/forgetheweb_website/config/credentials/development.yml.enc): mapping values are not allowed in this context at line 25 column 10
        from /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activesupport-8.0.2/lib/active_support/encrypted_configuration.rb:86:in 'ActiveSupport::EncryptedConfiguration#config'
        from /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activesupport-8.0.2/lib/active_support/encrypted_configuration.rb:113:in 'ActiveSupport::EncryptedConfiguration#options'
        from /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activesupport-8.0.2/lib/active_support/delegation.rb:186:in 'ActiveSupport::EncryptedConfiguration#method_missing'
        from /Users/kevin/Documents/finope/forgetheweb_website/config/environments/development.rb:84:in 'block in <main>'
        ```
You can browse the Internet, _no recovery possible_. Reset your credentials to a _previous safe state_ and re-edit.