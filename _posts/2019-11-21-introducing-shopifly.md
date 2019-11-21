---
layout: post
title: 'Introducing ShopifLy'
subtitle: 'Fly CLI tools to make Shopify theme development manageable'
date: 2019-11-21 12:40 -0800
categories: shopify
tags: [shopify]
author: lpender
sitemap: false
---

## Why Fly?

If you are doing modern theme development in Shopify, chances are you're doing a
lot of manual work to keep your branches in sync with your themes across all
environments.

We built [ShopifLy CLI dev tools]() to make theme development in Shopify much less
unwieldy.

## Branches === Themes.

Rather than having every permutation of every theme in every environment stored
in the `config.yml`, ShopifLy enforces a simple set of conventions:

1. Enforced parity between _branch_ names and _theme_ names.
2. Choose which _shop_ you are working on.

Rather than repeatedly copying and pasting the same API key and configuration
into your `config.yml`, we set the _shop_ that we are working on locally,
surface it clearly in the CLI, and rely on ShopifLy to enforce
parity between you local branch names and Shopify themes.

## Getting Started

We're going to have to add a bit of Ruby into our Shopify repository. No big
deal!

```
% bundle init
```

```
# Gemfile

gem 'shopifly'

```

```
% bundle install
```

This should add the `fly` command to our CLI.

Next we setup a `config.shops.yml` file in the root of our Shopify theme that
identifies the shops that we will be working with:

```
# config.shops.yml

shared_config:
  directory: ../shopify/
  ignore_files:
    - config/settings_data.json

shops:
  default: "development"
  development:
    password: xxx
    store: my-shop-dev.myshopify.com
  staging:
    password: xxx
    store: my-shop-staging.myshopify.com
  production:
    password: xxx
    store: my-shop.myshopify.com

```

We want to ignore a few files so that we're not sharing secrets or local config
in the repository.

```
# .gitignore

config.shops.yml
.current_shop
```

## Let's Fly

We initialize our repo by running:

```
% fly init
```

You should see output that confirms that your `config.shops.yml` is formatted
correctly and that your current shop is now set to `development` by the file
`.current_shop`

To switch the shop that you are currently using:

```
% fly ss staging
```

This will switch your `.current_shop` file so that all `fly` commands now point
to the relevant theme in your `staging` shop.

Now, instead of running `theme` commands with the `--env` flag, you can run
`fly` commands instead and know that they will go to the correct theme.

I.e. rather than running:

```
% theme watch --env=staging_master
```

You can run

```
% git checkout master
% fly ss staging
% fly watch
```

## Creating Branch Parity

Here's the real beauty. Let's say that I wanted to start a feature and test it
on a theme based on the `production` store.

```
% git checkout master
% fly ss production
% git checkout -b my-new-feature
% fly watch
```

This will do the following:

- Check if the theme `my-new-feature` exists on the `production` shop.
  - If so start watching for changes and uploading them to that theme.
  - If not:
    - Prompt user to create the remote theme?
      - Zip the contents of the `shared_config.directory` specified in your
        `config.shops.yml` file.
      - Create a Shopify theme with these contents on `production`.
    - Start watching for local changes and uploading them.

That's it! `fly` wraps the `theme` interface, so that any command you pass to
theme you can pass to `fly` instead.

So rather than:

- Going to the Admin interface
- Duplicating the live theme
- Renaming it
- Copying the theme_id
- Pasting it into your local `config.yml`
- Remembering to update it whenever you switch branches.

You simply use `fly` to set the shop and it automatically manages parity between
branches.

## Removing a branch.

To remove a branch:

```
fly destroy my-branch-name
```

This will destroy the branch locally and remove the remote Shopify theme.

## Installing Git Hooks

If you want to simplify enforcement of branch parity:

```
% fly enforce
```

This will install a set of git hooks that will do the following:

- On change of branch:
  - Check if a Shopify theme already exists for the branch that you are on,
    within the shop that you are currently on.
    - If so configure Shopifly commands to use this theme.
    - If not:
      - Prompt to create the remote theme:
        - Zip the contents of `shared_config.directory` from your
          `config.shops.yml` file.
        - Create a Shopify theme with these contents.
      - Configure Shopifly commands to use this theme

## Other notes

It's usually helpful to add your `.current_shop` to your command prompt. That's
outside the scope of this article, but to give you an idea, I have a
`.prompt.zsh` file that looks like this:

```
# modify the prompt to contain git branch name if applicable
git_prompt_info() {
  current_branch=$(git current-branch 2> /dev/null)
  if [[ -n $current_branch ]]; then
    echo " %{$fg_bold[green]%}$current_branch%{$reset_color%}"
  fi
}
shopify_shop_info() {
  current_shop=$(cat .current_shop)
  if [[ -n $current_shop ]]; then
    echo " %{$fg_bold[red]%}$current_shop%{$reset_color%}"
  fi
}
setopt promptsubst
PS1='${SSH_CONNECTION+"%{$fg_bold[green]%}%n@%m:"}%{$fg_bold[blue]%}%c%{$reset_color%}$(git_prompt_info)$(shopify_shop_info) %# '
```

## Conclusion

Shopify development is still not the panacea that we might hope for, but it's
little tools like this that make it easier and easier. We hope you enjoy
ShopifLy.
