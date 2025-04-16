---
type: post
title: Rails test crach course
description: When tests do not cooperate
date: 2025-04-16T10:21:13+02:00
lastmod: 2025-04-16
tags:
  - rails
  - ruby
  - unit test
---

When running test in Rails I came across several configuration failure and
crashes, let me share them

## Parallel tests crashes sometimes

### Do you have platform filtering?

I experienced a configuration leading me to test using macOs with wrong
platform filtering. I and to update my `Gemfile` like so

```diff
- gem 'debug', platforms: %i[ruby mri mingw x64_mingw windows]
+ gem 'debug'
```

Then update `Gemfile.lock` with

```bash
bundle update
```

No more crashes

### Are your `Gemfile` and `Gemfile.lock` synchronized?

I also came across a case where my `Gemfile` had on dependency not referenced
in my `Gemfile.lock`.

Just try an update to fix all this.

```sh
bundle update
rails test
```

You should be all set

## System test does not work?

When you run `rails test:system`, it fails and generate weird screenshots in
`tmp/screenshots`?

Try this:

```sh
rails assets:precompile
rails test:system
```

It should be fixed.
