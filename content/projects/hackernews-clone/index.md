+++
author = "Christopher Leggett"
title = "Hackernews Clone"
date = 2021-02-09T21:12:56-05:00
description = "Hackernews clone utilizing a GraphQL API backend and a Vue-Apollo frontend."
tags = ["go", "graphql", "apollo", "vuejs", "webdev", "backend", "frontend"]
categories = ["Portfolio"]
github = "https://github.com/leggettc18/hackernews-clone-api"
portfolio_score = 50
+++

This is a small educational project I wrote up to teach myself GraphQL and how to implement a
GraphQL API in Go. It's nothing **too** fancy, but it might be useful to someone learning GraphQL
themselves.

<!--more-->

For the [backend](https://github.com/leggettc18/hackernews-clone-api), it utilizes the graph-gophers
packages for parsing the schema, mapping incoming requests to the correct resolvers, and handling
subscriptions. It has a working setup for Websocket Subscriptions, Pagination, etc. Feel free to
look it over, take what I wrote with a grain of salt, but you may learn something new!

For the [frontend](https://github.com/leggettc18/hackernews-vue-apollo), it utilizes the [Vue
version of the Apollo](https://apollo.vuejs.org/) library. I followed a tutorial that may
or may not be slightly out of date, so some review and revision may be in order. Keep that in
mind if you choose to use it as an educational resource.

Feedback is appreciated if you come across these and find any glaring issues, or ways to improve
them as educational resources. Feel free to leave an issue on whichever repo the code changes would
go in.
