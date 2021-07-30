+++
title = "Devmarks"
date = 2020-09-13T17:22:48-04:00
Descripion = "Bookmark Manager for Teams of Developers"
Tags = ["developers", "organization", "bookmarks", "go", "rest", "api", "devmarks"]
Categories = ["Portfolio"]
github = "https://github.com/leggettc18/devmarks-api"
image = "devmarks.svg"
portfolio_score = 1000
+++

Devmarks is a bookmark manager intended for teams of developers to share 
bookmarks to documentation, intranet sites, and other valuable information. It 
is still in development so certain aspects of it are subject to change.

<!--more-->

Planned features include organizing bookmarks into folders and tags, and 
organizing users into organizations to more quickly share bookmarks and folders 
with a group of users. On the more technical side, it should have live-updating 
with WebSockets.

The app currently consists of two parts, 
[an API](https://github.com/leggettc18/devmarks-api) and 
[a web frontend](https://github.com/leggettc18/devmarks-frontend-web). Other 
parts, such as a desktop app or browser extension, could also in theory be developed 
from the API.

The API is written in Go and is a fairly basic GraphQL API, with JWT token-based 
authentication. Currently authentication is just done with a username and password. 
OAuth is on the list of things to do as the project advances.

The Web Frontend is written in VueJS 3.0. It utilizes Vue Router for routing,
HeadlessUI for some basic building blocks, HeroIcons for icons,and Tailwind CSS 
for styling. It makes heavy use of the 
[Apollo GraphQL Client](https://v4.apollo.vuejs.org/), to run queries
and mutations and update the in-memory cache, and it uses 
[graphql-codegen](https://www.graphql-code-generator.com/) to
generate Typescript types for all the GraphQL queries and such.

There is currently a demo hosted at [demo.devmarks.app](https://demo.devmarks.app).
This is still in what I would call alpha stages, so there may be bugs and
data may be wiped at any time. Feel free to 
[submit an issue](https://github.com/leggettc18/devmarks-frontend-web/issues) 
if you come across any bugs!
