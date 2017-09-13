---
title: Auto Convert Keywords Arguments in HTTP Request Spec in Rails 5
date: 2017-02-16 09:52:00 Z
categories:
- ruby,
- rails,
- rspec
layout: post
meta_keywords: auto convert, keyword arguments, rails 5, rspec controller test
meta_description: Insert params keyword argument automatically on all spec and test
  in Rails 5.
comments: true
---

```
DEPRECATION WARNING: Using positional arguments in functional tests has been deprecated,
in favor of keyword arguments, and will be removed in Rails 5.1.

Deprecated style:
get :show, { id: 1 }, nil, { notice: "This is a flash message" }

New keyword style:
get :show, params: { id: 1 }, flash: { notice: "This is a flash message" },
  session: nil # Can safely be omitted.
```

It is the rspec controller test warning I got when trying to upgrade from Rails 4.x to 5.x. In Rails 5 we need to change it, because it only accepts keyword arguments as parameter. 

{% highlight ruby %}
require "rails_helper"

describe PagesController do
  describe "GET /home" do
    it "responses ok" do
      get :home, id: 2, per_page: 5

      expect(response).to have_http_status :ok
      expect(response).to render_template :home
      expect(response).to render_template :blank
    end
  end
end
{% endhighlight %}

Change to be like this

{% highlight ruby %}
require "rails_helper"

describe PagesController do
  describe "GET /home" do
    it "responses ok" do
      get :home, params: { id: 2, per_page: 5 }

      expect(response).to have_http_status :ok
      expect(response).to render_template :home
      expect(response).to render_template :blank
    end
  end
end
{% endhighlight %}

In Rails 5 we have parameters to be more clear belongs to which part.
It's look like an easy change, just add params key as parameter on rspec controller request. But how if we have a lot of controllers that need to be updated. Of course it would be a boring task and waste a lot of time to change it one by one.

Fortunately, I found an awesome gem [https://github.com/tjgrathwell/rails5-spec-converter](https://github.com/tjgrathwell/rails5-spec-converter). So what we need is just install it on our development machine and run a line of command in terminal and ... TADA! All of our test will be updated with params key automatically.