---
layout: post
author: lpender
title: Modern theme development in Shopify, Part 1
categories: shopify
tags: [shopify]
excerpt: "Doing theme modern theme development in Shopify is hard, but there are
some tricks to the trade. In Part 1, we explore how to set environment variables
in a Shopify theme."
date: 2019-11-18 14:29 -0800
---

## Life without MVC

Part of the reason that Shopify theme development is so difficult is that we are
not given access to the Model or the Controller layers of a traditional MVC
structured repository, places where developers would traditionally store logic.

As such, we often see Liquid code strewn with complex logic which is difficult
to read, modify, fix, or extend.

We often hedge against this by moving to client-side MVC with a framework such
as Vue.JS, so that we can have a proper separation of concerns.

## Environment variables

One of the difficult aspects of theme development is the inability to set or
access environment variables, making impossible the separation of concerns
required for a proper [12 factor app](https://12factor.net/).

In continuing with the theme from above, we move to storing `ENV` variables in
`JS`, rather than `Liquid`, which gives us the ability to generate modular,
well-written code.

A concern is that it may be improper to store `ENV` variables in the front end
code. Because client-side code is inherently open-source and insecure, it's
important not to accidentally store any secrets in the `JS` environment.

Luckily, we aren't going to find a lot of private API keys in the Liquid code
anyway. Because `Liquid` is already a view layer, it's not able to perform
server-to-server calls.

One reason you might wish to add `ENV` variables is if your Shopify theme relies
on custom apps to extend functionalities. In this case, you would want your
`staging` store to access the corresponding assets on the `staging` app so that
you can properly run acceptance.

We put config right into the JavaScript:

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
  qa: {
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

    if (domain.indexOf("qa") > -1) {
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

Access the variables from your theme code as follows:

```
# snippets/mysnippet.liquid

$.getScript({
  url: ENV..my_js_url,
})
```
