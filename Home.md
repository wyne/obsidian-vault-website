---
share: "true"
topnav: 1
---
```dataviewjs
dv.span(await dv.io.load("_Includes/Header.md"))
```
## Photos
```dataview
LIST FROM "Photos"
```
## Posts
```dataview
LIST title FROM "Posts"
WHERE published = true
```

```page-gallery
# Any options given at the root level of the configuration
# will be used as defaults for all views (but can be overridden
# in any individual view). 
title: "Posts"
fields: [title]
filter: false
columns: 2
orientation: landscape
# If you don't include an explicit `views` option (which needs
# to be an array), then page-gallery will just use all the root
# level options as a single unnamed view.
views:
  - name: Yosemite
    from: '"Posts"'
```



---
Last modified `$= dv.current().file.mtime`