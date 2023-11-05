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
```dataviewjs
dv.pages('"Posts"')
    .where(p => p.thumbnail !== undefined)
    .map(item => {
        let thumb = dv.paragraph(`![[${item.thumbnail.path}|100]]`);
        return dv.el(
            "a",
            thumb,
            {
                attr: { href: item.file.path },
                cls: ["internal-link"]
            }
        )
    })
```

---
Last modified `$= dv.current().file.mtime`