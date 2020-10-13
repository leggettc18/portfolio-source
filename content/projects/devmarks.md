+++
title = "Devmarks"
date = 2020-09-13T17:22:48-04:00
Descripion = "Bookmark Manager for Teams of Developers"
Tags = ["developers", "organization", "bookmarks"]
Categories = ["Portfolio"]
+++

Devmarks is a bookmark manager intended for teams of developers to share bookmarks to documentation, intranet sites, and other valuable information. It is still in development so certain aspects of it are subject to change.

Planned features include organizing bookmarks into folders and tags, and organizing users into organizations to more quickly share bookmarks and folders with a group of users. On the more technical side, it should have live-updating with WebSockets.

The app currently consists of two parts, [an API](https://github.com/leggettc18/devmarks-api) and [a web frontend](https://github.com/leggettc18/devmarks-frontend-web). Other parts, such as a desktop app or browser extension, could also in theory be developed from the API.

The API is written in Go and is a fairly basic REST API, with bearer token authentication. Currently the web-based frontend sends a json request with a username and password and then receives the bearer token. I may eventually implement OAuth but I need to understand it better first.

The Web Frontend is written in VueJS using Vue-Router, Vuex, Typescript, and Vuetify.
