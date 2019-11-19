---
layout: post
author: lpender
title: Modern theme development in Shopify, Part One
categories: shopify
tags: [shopify]
excerpt: "Doing theme modern theme development in Shopify is hard, but there are
some tricks to the trade. Here we explore a strategy for settting environment
variables in a Shopify theme."
date: 2019-11-18 14:29 -0800
---

## Challenges of Theme Development

Shopify theme development does not allow access to the Model or the Controller
layers like a traditional MVC structured repository. These are places where
developers would traditionally store logic and computation before delivering
pre-processed data to the presentation layer.

As such, we often see Liquid code strewn with complex logic which is difficult
to read, modify, fix, or extend.

We often hedge against this by moving to client-side MVC with a framework such
as Vue.JS, which allows us to develop with a proper separation of concerns.

## Environment variables

Environment variables (or the lack thereof) are one of the challenges created by
view-only development that Shopify theme development insists on. Liquid does not
allow `ENV` variables, nor does it not easily lend itself to server-side
solutions that would mimic that sort of functionaltiy, rendering adherence to
the principles of a proper [12 factor app](https://12factor.net/) nearly
impossible.

In this unit, we elect to store `ENV`-like variables in a `window` level
(global) JavaScript, object, rather than `Liquid`. This gives us the ability to
generate modular, well-written code.

A valid concern is that it may be improper to store `ENV` variables in the front
end code. Because client-side code is inherently open-source and insecure, it's
important not to accidentally store any secrets in the `JS` environment.

Luckily, we aren't going to find a lot of private API keys in the Liquid code
anyway. Because `Liquid` is already a view layer, it's not able to perform
server-to-server calls. There are already few secrets in `Liquid` code (with
some exceptions, i.e. values that are hashed before being surfaced).

One reason you might wish to add `ENV` variables is if your Shopify theme relies
on custom apps. In this case, you would want your `staging` _store_ to access
the corresponding assets on your `staging` _app_ so that you can properly run
acceptance.

We put configuration right into the JavaScript:

```
# assets/js/env/index.js

let config = {
  development: {
    store_env: 'development',
    my_domain: 'https://custom-domain.ngrok.io',
  },
  production: {
    store_env: 'production',
    my_domain: 'https://my-app.herokuapp.com'
  },
  staging: {
    store_env: 'staging',
    my_domain: 'https://my-app-staging.herokuapp.com'
  },
  shared: {
    my_path_to_js: "/assets/my_asset.js"
  },
}

window.ENV = {
  shop_env: function() {
    let domain = window.location.hostname;

    if (domain.indexOf("staging") > -1) {
      return config.staging.store_env
    } else if (domain.indexOf("dev") > -1) {
      return config.development.store_env
    } else {
      return config.production.store_env
    }
  },
  my_domain: function() {
    return config[ENV.shop_env()].my_domain
  },
  my_js_url: function() {
    return ENV.my_domain() + config.shared.my_path_to_js
  }
}
```

Ouala. We can access the variables from our theme code as follows:

```
# snippets/mysnippet.liquid

$.getScript({
  url: ENV.my_js_url,
})
```
