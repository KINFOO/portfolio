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
draft: true
---

Using Rails builtin credentials, the guide[^1] is really slick. **But** when you
enter use details: surprise!

Here is an example.Let's say you have credentials looking like:

```yaml
provider:
  api_key: hash
  api_secret: secret
```

You would access them like so:

```ruby
secret = Rails.application.credentials.dig(:provider, :api_key)
```

Even though `dig`[^2] is part of Ruby and is not specific to Rails, I am still
feeling not sharp enough on the topic. Documentation provides examples:

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

```bash
$ rails credentials:show
/Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activesupport-8.0.2/lib/active_support/encrypted_configuration.rb:123:in 'ActiveSupport::EncryptedConfiguration#deserialize': Invalid YAML in '/Users/kevin/Documents/finope/forgetheweb_website/config/credentials/development.yml.enc'. (ActiveSupport::EncryptedConfiguration::InvalidContentError)

(/Users/kevin/Documents/finope/forgetheweb_website/config/credentials/development.yml.enc): mapping values are not allowed in this context at line 25 column 10
      from /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activesupport-8.0.2/lib/active_support/encrypted_configuration.rb:86:in 'ActiveSupport::EncryptedConfiguration#config'
      from /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activesupport-8.0.2/lib/active_support/encrypted_configuration.rb:113:in 'ActiveSupport::EncryptedConfiguration#options'
      from /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activesupport-8.0.2/lib/active_support/delegation.rb:186:in 'ActiveSupport::EncryptedConfiguration#method_missing'
      from /Users/kevin/Documents/finope/forgetheweb_website/config/environments/development.rb:84:in 'block in <main>'
```

You can browse the Internet, _no recovery possible_. Reset your credentials to a _previous safe state_ and re-edit.

[^1]: [Custom creedentials](https://guides.rubyonrails.org/security.html#environmental-security)
[^2]: [dig](https://ruby-doc.org/3.4.1/Array.html#method-i-dig)
