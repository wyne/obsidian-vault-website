---
share: "true"
topnav: 1
---
```dataviewjs
dv.span(await dv.io.load("_Includes/Header.md"))
```

This website is an experiment in using [Obsidian.md](https://obsidian.md/) as a website creation tool. It's a remake of [justinwyne.com](https://justinwyne.com) where I post about interests and share travel photos.
## Photos
```dataviewjs
const filePath = (file) => {
    if (file === undefined) { return "none"}
    return file.startsWith("http") ?
        file :
        this.app.vault.adapter.getResourcePath(file)
}

dv.pages('"Photos"').where(p => p.thumbnail !== undefined)
    .sort(item => item.file.name, 'asc')
    .map(item => {
        let bg = `url('${filePath(item.thumbnail.path)}')`;
        let link = dv.span(item.title, {cls: ["custom-gallery-title"]});
        //let link = "Link text"
        return  dv.el(
            "a",
            link,
            {
                cls: ["custom-gallery", "internal-link"],
                attr: {
                    href: `${item.file.path}`,
                    style: [`background-image: ${bg}`],
                    dataBg: `${bg}`
                }
            }
        )
    })
```


## Posts

```dataviewjs
const filePath = (file) => {
    if (file === undefined) { return "none"}
    return file.startsWith("http") ?
        file :
        this.app.vault.adapter.getResourcePath(file)
}
dv.pages('"Posts"').where(p => p.thumbnail !== undefined)
    .sort(item => item.file.name, 'asc')
    .map(item => {
        let bg = `url('${filePath(item.thumbnail.path)}')`;
        let link = dv.span(item.title, {cls: ["custom-gallery-title"]});
        //let link = "Link text"
        return  dv.el(
            "a",
            link,
            {
                cls: ["custom-gallery", "internal-link"],
                attr: {
                    href: `${item.file.path}`,
                    style: [`background-image: ${bg}`],
                    dataBg: `${bg}`
                }
            }
        )
    })
```

---
Last modified `$= dv.current().file.mtime`