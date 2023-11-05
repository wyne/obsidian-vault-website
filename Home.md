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
LIST WITHOUT ID link(file.path, title) FROM "Posts"
WHERE published = true
```
#### Dataview non-JS
```dataview
TABLE WITHOUT ID 
    embed(link(meta(thumbnail).path, "200")) as Thumbnail,
    file.link as Title
FROM "Posts"
WHERE published = true
```

---
Last modified `$= dv.current().file.mtime`