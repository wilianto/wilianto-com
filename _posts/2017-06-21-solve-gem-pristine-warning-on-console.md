---
layout: post
title: "Solve gem pristine Warning on Console (Mac)"
date: 2017-06-21 16:52:00 +0700
categories: ruby
meta_keywords: ruby gem
meta_description: "How I solve warning from gem pristine"
comments: true
---

I changed rvm to rbenv on my local machine 2 months ago. After did that, I got errors on my console when running `bundle install` or others ruby command. 

This is warning I got:
```
Ignoring executable-hooks-1.3.2 because its extensions are not built.  Try: gem pristine executable-hooks --version 1.3.2
Ignoring gem-wrappers-1.2.4 because its extensions are not built.  Try: gem pristine gem-wrappers --version 1.2.4
Error loading RubyGems plugin "/Users/wilianto/.rvm/gems/ruby-2.1.2@global/gems/gem-wrappers-1.2.4/lib/rubygems_plugin.rb": cannot load such file -- gem-wrappers (Lo
adError)
```

For somedays, I ignored it. But it was disgusting, showed every time I ran a ruby command. I tried to solve it by uninstalling old gem (installed via rvm)

```
gem uninstall -i /Users/wilianto/.rvm/gems/ruby-2.1.2@global gem-wrappers
```

If uninstallation success, the console will show this success message:
```
Successfully uninstalled gem-wrappers-1.2.4
```



