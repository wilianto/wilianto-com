---
title: Installing Nokogiri on OSX 10.11 El Capitan
date: 2015-12-28 09:52:00 Z
layout: post
meta_keywords: nokogiri el capitan, install nokogiri error
meta_description: How to solve error when installing Nokogiri on OSX 10.11 El Capitan
comments: true
---

Last month, I updated my OSX from Yosemite to El Capitan. But unfortunatelly, when I wanted to run my rails application. I got error installing Nokogiri gem.

```
Building native extensions.  This could take a while...
ERROR:  Error installing nokogiri:
	ERROR: Failed to build gem native extension.

    /Applications/rubystack/rvm/rubies/ruby-2.2.1/bin/ruby -r ./siteconf20151228-72923-1lost2e.rb extconf.rb
checking if the C compiler accepts  -I/Applications/rubystack/sqlite/include -I/Applications/rubystack/varnish/include -I/Applications/rubystack/mysql/include -I/Applications/rubystack/postgresql/include -I/Applications/rubystack/apache2/include -I/Applications/rubystack/subversion/include -I/Applications/rubystack/common/include -I/usr/local/include -I/usr/include -m64... *** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of necessary
libraries and/or headers.  Check the mkmf.log file for more details.  You may
need configuration options.

Provided configuration options:
	--with-opt-dir
	--without-opt-dir
	--with-opt-include
	--without-opt-include=${opt-dir}/include
	--with-opt-lib
	--without-opt-lib=${opt-dir}/lib
	--with-make-prog
	--without-make-prog
	--srcdir=.
	--curdir
	--ruby=/Applications/rubystack/rvm/rubies/ruby-2.2.1/bin/$(RUBY_BASE_NAME)
	--help
	--clean
/Applications/rubystack/rvm/rubies/ruby-2.2.1/lib/ruby/2.2.0/mkmf.rb:456:in `try_do': The compiler failed to generate an executable file. (RuntimeError)
You have to install development tools first.
	from /Applications/rubystack/rvm/rubies/ruby-2.2.1/lib/ruby/2.2.0/mkmf.rb:571:in `block in try_compile'
	from /Applications/rubystack/rvm/rubies/ruby-2.2.1/lib/ruby/2.2.0/mkmf.rb:522:in `with_werror'
	from /Applications/rubystack/rvm/rubies/ruby-2.2.1/lib/ruby/2.2.0/mkmf.rb:571:in `try_compile'
	from extconf.rb:80:in `nokogiri_try_compile'
	from extconf.rb:87:in `block in add_cflags'
	from /Applications/rubystack/rvm/rubies/ruby-2.2.1/lib/ruby/2.2.0/mkmf.rb:619:in `with_cflags'
	from extconf.rb:86:in `add_cflags'
	from extconf.rb:336:in `<main>'

extconf failed, exit code 1

Gem files will remain installed in /Applications/rubystack/rvm/gems/ruby-2.2.1/gems/nokogiri-1.6.7.1 for inspection.
Results logged to /Applications/rubystack/rvm/gems/ruby-2.2.1/extensions/x86_64-darwin-14/2.2.0-static/nokogiri-1.6.7.1/gem_make.out
```

I tried fix the problem, and I found the best solution that worked for me. I needed to install Nokogiri with spesific with-xml2-include parameters. Below is the command.

```
gem install nokogiri -- \
--with-xml2-include=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.11.sdk/usr/include/libxml2 \
--use-system-libraries
```

Hope it also works with you.

