---
layout: post
author: lpender
title: Modern theme development in Shopify
categories: shopify
tags: [shopify]
excerpt: "Doing theme modern theme development in Shopify is hard, but there are
some tricks to the trade."
date: 2019-11-18 14:29 -0800
---

## Life without MVC

Part of the reason that Shopify theme development is so difficult is that we are
not given access to the Model or the Controller layers of a traditional MVC
structured repository, places where developers would traditionally store logic

We hedge against this by moving to client-side MVC with a framework such as
Vue.JS, so that we can have a proper separation of concerns.

## Environment variables

Again, because of the lack of access to controllers and models, global
configuration becomes a difficult task. Luckily, we aren't going to find a lot
of private api keys in the Liquid code anyway, since we're not making any
server-to-server calls from Liquid code.

We can put config right into the JavaScript:

```
# assets/js/env/index.js

let config = {
  development: {
    store_env: 'development',
    loyalty_domain: 'https://mack-weldon-loyalty.ngrok.io',
  },
  production: {
    store_env: 'production',
    loyalty_domain: 'https://mw-loyalty-app-2.herokuapp.com'
  },
  qa: {
    store_env: 'qa',
    loyalty_domain: 'https://mw-loyalty-app-2-qa.herokuapp.com'
  },
  shared: {
    loyalty_path_to_js: "/mw-loyalty/mw-loyalty.js"
  },
}

window.ENV = {
  shop_env: function() {
    let domain = window.location.hostname;

    if (domain.indexOf("qa") > -1) {
      return config.qa.store_env
    } else if (domain.indexOf("dev") > -1) {
      return config.development.store_env
    } else {
      return config.production.store_env
    }
  },
  loyalty_domain: function() {
    return config[ENV.shop_env()].loyalty_domain
  },
  loyalty_js_url: function() {
    return ENV.loyalty_domain() + config.shared.loyalty_path_to_js
  }
}
```
