active\_link\_to
================

Creates a link tag of the given name using a URL created by the set of
options. Please see documentation for `link_to`, as `active_link_to` is
basically a wrapper for it. This method accepts an optional :active
parameter that dictates if the given link will have an extra css class
attached that marks it as 'active'.

## Install
### In Rails 3.*

In Gemfile

    gem 'active_link_to'
    
and run `bundle install`

### In older Rails
  
    $ sudo gem install active_link_to

Then you probably want to declare it in your environment.rb

    config.gem 'active_link_to'
  
and freeze it (preferably):

    $ rake gems:unpack GEM=active_link_to

### Super Simple Example
Here's a link that will have class attached if it happens to be rendered 
on page with path `/people` or any child of that page like `/people/123`

    active_link_to 'People', '/people'
    # => <a href="/people" class="active">People</a>

This is exactly the same as:

    active_link_to 'People', '/people', :active => { :when => :self }
    # => <a href="/people" class="active">People</a>

### Options
Here's available options that can be inserted into `:active` hash

* `:when => predefined symbol || Regexp || Array tuple of controller/actions || Boolean` - This controls
  when link is considered to be 'active'
* `:active_class => 'class_name'` - When link is 'active', css
  class of that link is set to the default 'active'. This parameter 
  allows it to be changed
* `:inactive_class => 'class_name'` - Opposite of the :active_class
  By default it's blank. However you can change it to whatever you want.
* `:disable_link => Boolean` - When link is active, sometimes you
  want to render span tag instead of the link. This parameter is for that.

### Examples
Most of the functionality of `active_link_to` depends on the current
url. Specifically, `request.fullpath` value. We covered the basic example
already, so let's try something more fun.

We want to highlight the link that matches immediate url, and not the children
nodes as well

    # For URL: /people/24
    active_link_to 'People', people_path, :active => { :when => :self_only }
    # => <a href="/people">People</a>

    # For URL: /people
    active_link_to 'People', people_path, :active => { :when => :self_only }
    # => <a href="/people" class="active">People</a>

If we need to set link to be active based on some regular expression, we can do
that as well. Let's try to activate links urls of which begin with 'peop':

    # For URL: /people/44
    active_link_to 'People', people_path, :active => { :when => /^peop/ }
    # => <a href="/people" class="active">People</a>

    # For URL: /aliens/9
    active_link_to 'People', people_path, :active => { :when => /^peop/ }
    # => <a href="/people">People</a>

What if we need to mark link active for all URLs that match a particular controller,
or action, or both? Or any number of those at the same time? Sure, why not:

    # For URL: /people/56/edit
    # For matching multiple controllers and actions:
    active_link_to 'Person Edit', edit_person_path(@person), :active => {
      :when => [['people', 'aliens'], ['show', 'edit']]
    }

    # for matching all actions under given controllers:
    active_link_to 'Person Edit', edit_person_path(@person), :active => {
      :when => [['people', 'aliens'], []]
    }

    # for matching all controllers for a particular action
    active_link_to 'Person Edit', edit_person_path(@person), :active => {
      :when => [[], ['edit']]
    }
    # => <a href="/people" class="active">People</a>

Sometimes it should be easy as setting a true or false:

    active_link_to 'People', people_path, :active => { :when => true }
    # => <a href="/people" class="active">People</a>

    active_link_to 'People', people_path, :active => { :when => false }
    # => <a href="/people">People</a>

Lets see what happens when we push some css class modifier parameters

    # For URL: /people/12
    active_link_to 'People', people_path, :active => { :active_link => 'awesome_selected' }
    # => <a href="/people" class="awesome_selected">People</a>

    active_link_to 'Aliens', aliens_path, :active => { :inactive_link => 'bummer_inactive' }
    # => <a href="/aliens" class="bummer_inactive">Aliens</a>

Some weird people think that link should change to a span if it indicated current page:

    # For URL: /people
    active_link_to 'People', people_path, :active => { :disable_link => true }
    # => <span class="active">People</span>

### Copyright

Copyright (c) 2009 Oleg Khabarov, The Working Group Inc. See LICENSE for details.
