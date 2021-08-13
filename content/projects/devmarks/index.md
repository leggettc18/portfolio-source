+++
title = "Devmarks"
date = 2020-09-13T17:22:48-04:00
Descripion = "Bookmark Manager for Teams of Developers"
Tags = ["developers", "organization", "bookmarks", "go", "rest", "api", "devmarks"]
Categories = ["Portfolio"]
github = "https://github.com/leggettc18/devmarks"
image = "devmarks.svg"
portfolio_score = 1000
+++

Devmarks is a bookmark manager intended for teams of developers to share 
bookmarks to documentation, intranet sites, and other valuable information. It 
is still in development so certain aspects of it are subject to change.

<!--more-->

Planned features include organizing bookmarks into folders and tags, and 
organizing users into organizations to more quickly share bookmarks and folders 
with a group of users.

On the more technical side, the app uses a VueJS 3.0 + Typescript frontend, 
with a REST API backend written in Golang. It uses PostgresQL for its database,
leveraging the GORM library for ORM capabilities, and takes advantage of JWT token-based 
authentication. Currently authentication is just done with an email and password. 
OAuth is on the list of things to do as the project advances.

The Web Frontend is written in VueJS 3.0. It utilizes Vue Router for routing,
HeadlessUI for some basic building blocks, HeroIcons for icons,and Tailwind CSS 
for styling. I have utilized OpenAPI 3.0 generators to describe the backend API
with an openapi.yml file and generated a Typescript/Axios client on the frontend,
which I have wrapped up in the frontend to make a few of my common use cases
more convenient.

There is currently a demo hosted at [demo.devmarks.app](https://demo.devmarks.app).
This is still in what I would call alpha stages, so there may be bugs and
data may be wiped at any time. Feel free to 
[submit an issue](https://github.com/leggettc18/devmarks/issues) 
if you come across any bugs!
