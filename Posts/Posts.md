---
topnav: 4
---
```dataviewjs
dv.span(await dv.io.load("_Includes/Header.md"))
```


```dataview
TABLE WITHOUT ID 
    embed(link(meta(thumbnail).path, "200")) as Thumbnail,
    link(file.link, title) as Title
FROM "Posts"
WHERE published = true
```
