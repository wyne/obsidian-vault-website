---
share: "true"
topnav: 1
---
```dataviewjs
dv.span(await dv.io.load("_Includes/Header.md"))
```

This website is an experiment in using [Obsidian.md](https://obsidian.md/) as a website creation tool. It's a remake of [justinwyne.com](https://justinwyne.com) where I post about interests and share travel photos.
## Photos
```dataview
LIST FROM "Photos"
```

## Posts
```dataviewjs
dv.pages('"Posts"')
    .where(p => p.thumbnail !== undefined)
    .map(item => {
        let thumb = dv.paragraph(`![[${item.thumbnail.path}|200]]`);
        let link = dv.el(
            "a",
            thumb,
            {
                attr: { href: item.file.path },
                cls: ["internal-link"]
            }
        )
        return link;
    })
```

---
Last modified `$= dv.current().file.mtime`