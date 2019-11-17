---
layout: post
author: lpender
title:  "End-to-End Testing React Apps"
subtitle: "Long live RSpec"
date:   2019-11-11 19:09:05 -0500
categories: rspec, javascript, web-app, react, capybara
image: "/assets/images/posts/end-to-end-testing-react-apps/Red_onion_cross_section_04.jpg"
image_alt: "A rocket ship launching into space."
tags: [rspec, react, rails, end-to-end]
excerpt: "The Rails monolith is considered by many to be passé. But if your app
uses a front end framework such as React to talk to a API, chances are you're
not doing proper outside-in testing."
---

The Rails monolith is considered by some to be a classic go-to, by others
to be passé. But if your app uses a trendy front end framework such as
[React](https://www.reactjs.org) or [Vue.JS](https://www.vuejs.org) to talk to a
API, chances are you're not writing proper end-to-end tests.

Newer JavaScript testing tools, such as
[Enzyme](http://airbnb.io/enzyme/) and [Jest](https://facebook.github.io/jest/).
create abstractions that allow developers to test the "view" layer. But [testing
from the outside-in](https://thoughtbot.com/blog/testing-from-the-outsidein)
means that before we reach in and start testing internal components, we start
with end-to-end tests: tests that simulate what it's like to be a _user_ of your
app.

End-to-end tests are the most important class of tests that we have, as they
test the functionality that our clients and customers expect from the products
that we build. In fact, prematurely writing low-level unit and integration tests
for internal components can be counterproductive, especially for parts of your
app that you are iterating on, as they make refactoring and restructuring more
taxing.

## The Beauty of End to End tests

We'd like to have tests that look **less** like this:

```
const mockTryGetValue = jest.fn(() => false);
const mockTrySetValue = jest.fn();

jest.mock('save-to-storage', () => ({
  SaveToStorage: jest.fn().mockImplementation(() => ({
    tryGetValue: mockTryGetValue,
    trySetValue: mockTrySetValue,
  })),
}));
describe('MyComponent', () => {
  it('should set storage on save button click', () => {
    mockTryGetValue.mockReturnValueOnce(true);
    const component = mount(<MyComponent />);
    component.find('button#my-button-three').simulate('click');
    expect(mockTryGetValue).toHaveBeenCalled();
    expect(component).toMatchSnapshot();
    component.unmount();
  });
});
```

And more like this:

```
visit "/my-item/"

click_on "Edit"

expect(page).to have_content("Editing My Item")
```

By testing our JavaScript web app with RSpec, we rely on a robust library with a
simple, readable DSL that has been battle tested for years. By implementing
end-to-end test coverage, our developers are able to refactor and rewrite the
internals of our app at will with confidence that our app is performing as
expected, increasing both stability and in the long run, velocity.

## Introduce Ruby to your Repo

Open your React repository and follow the instructions below:

```
$ bundle init
```

We will need the following packages to get started:


{% highlight ruby %}
# Gemfile
source "https://rubygems.org"

gem "capybara"
gem "chromedriver-helper", "1.0.0"
gem "puffing-billy"
gem "pry"
gem "rspec"
gem "selenium-webdriver"
gem 'sinatra', '2.0.0.beta2' # https://github.com/sinatra/sinatra/issues/1055

{% endhighlight %}

Let's go ahead and install them!

```
$ bundle
```


### Rspec

Now initialize rspec

```
$ rspec --init
```

Now running rspec should show basically nothing:

```
$ rspec
```


### Sinatra

Next, let's set up a simple Sinatra app, which loads our JavaScript app.

This file should be essentially a copy of your `index.html` file.

The following example assumes:

- Your React app is compiling to `/dist/app.js`.
- It injects into `#app`.

Obviously, update the code below to match your app if necessary.

{% highlight html %}
<!-- spec/views/test_app.erb -->

<!DOCTYPE html>
<head>
  <title>My App</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="app.css"/>
</head>
<body>
  <div id="app"></div>
  <script type="text/javascript" src="app.js"/></script>
</body>
{% endhighlight %}


### Routing

Now we add basic routing for our Sinatra app.

**Note**: If you don't use hashUrls for routing, you may have to work a bit
harder on this file.

{% highlight ruby %}
# spec/test_app.rb

require 'bundler/setup'
require 'sinatra'

set :public_folder, 'dist/'

get '/' do
  erb :test_app
end
{% endhighlight %}


### Configuring RSpec

Now we set up RSpec to initialize and run our Sinatra app, using a configuration
of Chrome that will proxy cache any API calls through a gem called [Puffing
Billy](https://github.com/oesmith/puffing-billy).

You can read more about using Puffing Billy in their documentation.

{% highlight ruby %}
# spec/spec_helper.rb

ENV['RACK_ENV'] = 'test'

require_relative "test_app"

require 'billy/capybara/rspec'
require "capybara/rspec"
require "capybara/dsl"
require "pry"

Dir[Dir.pwd + "/spec/support/**/*.rb"].each { |file| require file }

Billy.configure do |config|
  config.cache = true
  config.cache_path = 'spec/fixtures/features' # remove to get fresh mocks
  config.cache_request_body_methods = ['post', 'get', 'put']
  config.ignore_params = []
  config.logger = nil # comment to see logs
  config.non_successful_cache_disabled = false
  config.non_successful_error_level = :warn
  config.non_whitelisted_requests_disabled = false
  config.persist_cache = true
end

Capybara.configure do |config|
  config.default_driver = :selenium_chrome_billy
  config.app = Sinatra::Application.new
end

RSpec.configure do |config|
  config.expect_with :rspec do |expectations|
    expectations.include_chain_clauses_in_custom_matcher_descriptions = true
  end

  config.mock_with :rspec do |mocks|
    mocks.verify_partial_doubles = true
  end

  config.shared_context_metadata_behavior = :apply_to_host_groups
end
{% endhighlight ruby %}

### Write your first test

We're ready to finally write our first test case:

{% highlight ruby %}
# spec/features/user-sees-app_spec.rb

RSpec.feature "User sees app", :type=> :feature do
  scenario "and sees that it says 'hello world'" do
    visit "/"

    expect(page).to have_text("Hello World")
  end
end
{% endhighlight %}

Type

```
$ rspec
```

to run your tests.

### What about my API calls?

If you've used RSpec before, you're probably used to using something like
[VCR](https://github.com/vcr/vcr) for recording and playing back external http
calls to APIs and other services.

That won't work here, because the calls aren't coming from a backend client like
Rails--they're AJAX calls coming from the browser.

Instead, we're using a library called [Puffing
Billy](https://github.com/oesmith/puffing-billy).

The first time you run the test, Puffing Billy will make calls to your actual
API, and cache the responses in `spec/fixtures/feature`.

To prevent it from sending and caching requests without your intent, set the
`non_whitelisted_requests_disabled` configuration parameter to `true`:

```
# spec/spec_helper.rb

Billy.configure do |config|
  ...
  config.non_whitelisted_requests_disabled = true
  ...
end
```

### Other concerns

#### Fixtures

One major difference between this setup and a traditional Rails/RSpec setup is
that you can't generate fixtures on the fly.

For example, in a traditional Rails/RSpec setup, I might use `FactoryBot` to
create test fixtures.

If you're interacting with an API seated in a separate repo, you don't have this
ability, because you cannot access the backend from your test code. So you need
to take some care to set up test fixtures remotely.

Your setup may differ, or you may choose to use your staging, QA, or production
server to generate test API responses. Personally I use
[mockable.io](https://www.mockable.io), and setup my test code to use
corresponding endpoints from that origin. Setting your app up to use mockable is
beyond the scope of this article.

#### Other setups

Some setups may not take kindly to using Sinatra, such as certain apps using
websockets or GraphQL. In these cases, you can write scripts to manually
start/stop your app in the background before running tests. Then use your apps
actual development URL as the `base_url` in your RSpec tests.

In some cases you may wish to hit actual endpoints i.e. a dedicated test
server. In this case you can ignore the Puffing Billy parts of this document.

### That's all folks!

Thanks for reading! I hope this changes your life.

