---
type: post
title: Rails test crash course
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

Running `rails test` looks like:

```bash
/Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/pg-1.5.9/lib/pg/connection.rb:703: [BUG] Segmentation fault at 0x0000000120390b28
ruby 3.4.2 (2025-02-15 revision d2930f8e7a) +PRISM [arm64-darwin24]

-- Crash Report log information --------------------------------------------
   See Crash Report log file in one of the following locations:
     * ~/Library/Logs/DiagnosticReports
     * /Library/Logs/DiagnosticReports
   for more details.
Don't forget to include the above Crash Report log file in bug reports.

-- Control frame information -----------------------------------------------
c:0059 p:---- s:0342 e:000341 CFUNC  :connect_poll
c:0058 p:0351 s:0338 e:000337 METHOD /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/pg-1.5.9/lib/pg/connection.rb:703
c:0057 p:0141 s:0325 e:000324 METHOD /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/pg-1.5.9/lib/pg/connection.rb:844
c:0056 p:0007 s:0316 e:000315 METHOD /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/pg-1.5.9/lib/pg/connection.rb:772
c:0055 p:0012 s:0310 e:000309 METHOD /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/pg-1.5.9/lib/pg.rb:63
c:0054 p:0006 s:0304 e:000303 METHOD /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activerecord-8.0.2/lib/active_record/connection_adapters/postgresql
c:0053 p:0008 s:0298 e:000297 METHOD /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activerecord-8.0.2/lib/active_record/connection_adapters/postgresql
c:0052 p:0020 s:0293 e:000292 METHOD /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activerecord-8.0.2/lib/active_record/connection_adapters/postgresql
...
-- Ruby level backtrace information ----------------------------------------
/Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/minitest-5.25.5/lib/minitest.rb:86:in 'block in autorun'
/Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/minitest-5.25.5/lib/minitest.rb:285:in 'run'
/Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activesupport-8.0.2/lib/active_support/testing/parallelize_executor.rb:19:in 'start'
/Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activesupport-8.0.2/lib/active_support/testing/parallelization.rb:36:in 'start'
/Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activesupport-8.0.2/lib/active_support/testing/parallelization.rb:36:in 'map'
/Users/kevin/.rbenv/versions/3.4.2/lib/ruby/gems/3.4.0/gems/activesupport-8.0.2/lib/active_support/testing/parallelization.rb:36:in 'each'
<internal:numeric>:257:in 'times'
...

-- Threading information ---------------------------------------------------
Total ractor count: 1
Ruby thread count for this ractor: 4

-- Machine register context ------------------------------------------------
  x0: 0x00000001f5ed4d2c  x1: 0x0000000000000000  x2: 0x0000000186d47bb4
  x3: 0x000000000000000c  x4: 0x0000000186e1b024  x5: 0x000000016b88b170
  x6: 0x0000000000000000  x7: 0x0000000000000810 x18: 0x0000000000000000
 x19: 0x000000013081bb70 x20: 0x0000000120390b2a x21: 0x0000000fffffc104
 x22: 0x0000000000000002 x23: 0x00000001252fbeb0 x24: 0x0000000000000001
 x25: 0x0000000000000000 x26: 0x0000000000000000 x27: 0x0000000130991938
 x28: 0x0000000130991908  lr: 0x0000000186d49f08  fp: 0x000000016b88b180
  sp: 0x000000016b88b140

-- C level backtrace information -------------------------------------------
/Users/kevin/.rbenv/versions/3.4.2/lib/libruby.3.4.dylib(rb_vm_bugreport+0xb6c) [0x104efffa8]
/Users/kevin/.rbenv/versions/3.4.2/lib/libruby.3.4.dylib(rb_bug_for_fatal_signal+0x100) [0x104d3b940]
/Users/kevin/.rbenv/versions/3.4.2/lib/libruby.3.4.dylib(sigsegv+0x84) [0x104e60f54]
/usr/lib/system/libsystem_platform.dylib(_sigtramp+0x38) [0x18703b624]
/usr/lib/system/libsystem_trace.dylib(_os_log_preferences_refresh+0x24) [0x186d49f08]
[0x186d4a884]
[0x186d06428]
[0x186d08d00]
[0x121b1af54]
[0x121b1aa40]


-- Other runtime information -----------------------------------------------

* Loaded script: bin/rails

* Loaded features:

    0 enumerator.so
    1 thread.rb
    2 fiber.so
    3 rational.so
    4 complex.so
    5 ruby2_keywords.rb
    6 /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/3.4.0/arm64-darwin24/enc/encdb.bundle
    7 /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/3.4.0/arm64-darwin24/enc/trans/transdb.bundle
    8 /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/3.4.0/arm64-darwin24/rbconfig.rb
    9 /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/3.4.0/rubygems/compatibility.rb
   10 /Users/kevin/.rbenv/versions/3.4.2/lib/ruby/3.4.0/rubygems/defaults.rb
...
```

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
