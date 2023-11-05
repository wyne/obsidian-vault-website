---
topnav: 4
---

```dataview
TABLE WITHOUT ID 
    embed(link(meta(thumbnail).path, "200")) as Thumbnail,
    link(file.link, title) as Title
FROM "Posts"
WHERE published = true
```
