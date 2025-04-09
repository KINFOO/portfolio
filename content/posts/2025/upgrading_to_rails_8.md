---
type: post
title: Upgrading to Rails 8
description: The right start
date: 2025-04-09T09:56:21+02:00
lastmod: 2025-04-09
showTableOfContents: true
tags:
  - rails
  - ruby
  - documentation
draft: true
---

I was seeking the best way to handle OAuth 2 in a Rails application. I was
learning about wide spread _Devise_ library and then came to realize:
_OAuth 2_ is available right out of the box with _Rails 8_.

Sadly we had started our project with _Rails 7_. To access elegant _OAuth 2_,
I had to upgrade to _Rails 8_[^1]. To be more accurate, I have to migrate from _Devise to Rails 8_[^2]

## Generate authentication
First step is to _generate authentication_[^3]

[^1]: [Rails' upgrade guide](https://guides.rubyonrails.org/upgrading_ruby_on_rails.html)
[^2]: [Migrate from Devise to Rails 8](https://radanskoric.com/guest-articles/from-devise-to-rails-auth)
[^3]: [Generate Authentication](https://guides.rubyonrails.org/getting_started.html#adding-authentication)
