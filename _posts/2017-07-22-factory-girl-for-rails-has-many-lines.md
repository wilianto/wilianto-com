---
title: Factory Girl for Rails Has Many Lines
date: 2017-07-22 09:52:00 Z
categories:
- ruby
layout: post
meta_keywords: factory girl has many, factory has many lines, factory girl ignore attribute
meta_description: How to create rails has many lines factory with factory girl
comments: true
---

This is a factory girl for rails has many relation example. We can use this for declaring how many booking lines I want to create. 

The technique uses an extra attribute `number_of_lines` to specify how many booking lines will be created. We ignore this attribute, so Factory Girl will not save it as ActiveRecord attribute.

{% highlight ruby %}
FactoryGirl.define do
  factory :booking_line do
    booking
    item
    qty { 1 }
  end
end

FactoryGirl.define do
  factory :booking do
    note { Faker::Lorem.sentence }
    address { Faker::Address.street_name }
    user

    trait :with_lines do
      ignore do
        number_of_lines 1
      end

      after :create do |booking, evaluator|
        create_list :booking_line, evaluator.number_of_lines, booking: booking
      end
    end
  end
end
{% endhighlight %}

We can use it like this on our test.

{% highlight ruby %}
create_list :booking, 2, :with_lines, number_of_lines: 3, user: user
{% endhighlight %}

:)
