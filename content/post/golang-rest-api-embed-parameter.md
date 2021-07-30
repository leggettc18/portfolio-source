+++
author = "Christopher Leggett"
title = "Golang REST API Tip - Use an Embed Parameter to Embed Related Resources"
date = 2021-07-22T23:37:34-04:00
description = "A practical example demonstrating how to handle an embed parameter in a Golang REST API to embed or preload related resources."
tags = ["go", "rest", "api", "embed"]
+++

In a REST API, you often have relations between resources. While you can certainly
return IDs for these related objects and require the API consumer to query them,
that can be somewhat inconvenient for your users. On the other hand, you certainly
don't want to eagerly load these on every request, or you could be sending
unnecessarily large payloads in your responses. So what can you do?

In my case, I chose to support a query parameter with the name `embed`. Semantically,
the logic is that we are "embedding" the related resource in the response for
the resource requested by the path. For example, in a bookmark management app,
you may be requesting a folder and embed the bookmarks in the folder
with the following URL.

```Bash
<baseUrl>/folders/<id>/?embed=bookmarks
```

With that, you would receive a response body similar to the following:

```JSON
{
    "id": 1,
    "name": "folder",
    "bookmarks": [
        {
            "id": 1,
            "name": "My Website",
            "url": "https://chrisleggett.me"
        },
        {
            "id": 2,
            "name": "Devmarks Demo",
            "url": "https://demo.devmarks.app"
        }
    ]
}
```

Side Note: I picked the word "embed" here, but there is no standard around this.
It would be just as valid for you to choose "preload" or "include". As long
as your choice is documented with the rest of your api documentation, it
ultimately doesn't matter.

Now how would you actually implement this? Below I will provide some code excerpts
that show how I chose to implement this. My examples below use `gorilla/mux` for
routing, and `gorm` for database access via an ORM, but with some light modification
this concept should be able to be implemented with other libraries as well.
Additionally, I do not claim that I am the first to come up with this, nor do
I claim that this is the only way or even the best way to accomplish this.

First off, I recommend extracting the embed query parameter from the URL and
saving it in the context via middleware so it can be pulled out of the context
later in any method down the line. As such, we need to setup a key for the
context to use for storing and retrieving the embed values. I define this
in a separate helpers package in order to avoid cyclical imports.

```Go
// helpers/ctx.go
package helpers

type contextKey struct {
	key string
}
var EmbedsKey = contextKey{"embeds"}

// Wherever you choose to define this middleware
...
import <project_url>/helpers
...
func apiMiddleware(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		values := r.URL.Query()
		embeds := strings.Split(values.Get("embed"), ",")
		ctx := r.Context()
		ctx = context.WithValue(ctx, helpers.EmbedsKey, embeds)
		next.ServeHTTP(w, r.WithContext(ctx))
		
	})
}
```

As you can see, the helpers package defines a contextKey struct and defines an
exported instance of that key called `EmbedsKey`. Wherever we define our middleware,
we then import the helpers package and use the `EmbedsKey` to save an array of
strings that we extracted from the `embeds` query parameter. We then move onto
our next middleware/final handler passing in the `ResponseWriter` and the `Request`
object with our modifed context inside.

Make sure you use the middleware in your router.

```Go
import 	"github.com/gorilla/mux"

r := mux.NewRouter()
r.Use(apiMiddleware)
// your routes and handlers here
```

Next, your request goes through your handler and eventually makes it to your 
database code. Make sure your handler function extracts the context from the 
`Request` (`ctx := r.Context())` and then passes it down to your database handling package. 
This is where I extract the embeds array we saved in the context
earlier. GORM has a `Preload` function that we can use to preload associations
defined in your models. However, you need some way to validate the embed values
to make sure they are valid associations in your database, as there's nothing
stopping an api consumer from putting anything at all in the embeds value.

To start off with, let's wrap the logic around setting up the `*gorm.DB` instance
with the preloads.

```Go
package db

type Database struct {
	*gorm.DB
}

// You should probably put this in your helpers package,
// I have it here for clarity.
func contains(array []string, s string) bool {
	for _, x := range array {
		if x == s {
			return true
		}
	}
	return false
}

func (db *Database) preloadEmbeds(valid []string, embeds []string) *gorm.DB {
	var instance = db.DB
	for _, embed := range embeds {
		if contains(valid, embed) {
			instance = instance.Preload(strings.Title(embed))
		}
	}
	return instance
}
```

This function takes an array of valid embed values and the ones that were
provided in the request. It then returns an instance of `*gorm.DB` with the
Preloads prepared. It currently just ignores any invalid embed values that are
provided. If no embed values are provided, then it simply returns the same `*gorm.DB` 
instance it was called on. As this returns a `*gorm.DB`, we can immediately chain a `First()` or
`Find()` method, for instance, off of this function. For example:

```Go
// db/folder.go
func (db *Database) GetFolderByID(ctx context.Context, id uint) (*model.Folder, error) {
	var folder model.Folder
	embeds, ok := ctx.Value(helpers.EmbedsKey).([]string)
	if !ok {
		return nil, errors.New("embeds parsing error")
	}
	return &folder, errors.Wrap(db.preloadEmbeds(model.FolderValidEmbeds(), embeds).
        First(&folder, id).Error, "unable to get folder")
}

// model/folder.go
package model

type Folder struct {
    ID    uint `json:"id"`
	Name  string `json:"name"`

	Bookmarks []Bookmark `gorm:"many2many:bookmark_folder;" json:"bookmarks"`
}

// Add strings to the array to allow embedding that resource through the
// embed query paramter.
func FolderValidEmbeds() []string {
	return []string{"bookmarks"}
}
```

Unfortunately Go does not have a good equivalent to static methods, so the next
best thing is to define a namespaced exported function in the models package
that returns an array of strings which represent the list of valid embed values.
I named this one `FolderValidEmbeds`. If later on we develop a relationship
between Bookmarks and Tags and we want to provide embed support for it, for example, 
you would need to define a similar function called `BookmarkValidEmbeds` that returns 
an array of strings containing `"tags"`.

At this point, your API handler should write out the returned object as json
to your api consumer. If you wish to see this in practice, check out my open source
bookmark manager api. It is still in active development, but besides some minor
reorganization this idea probably won't change. Check it out [here](https://github.com/leggettc18/devmarks/tree/main/api)!