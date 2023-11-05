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

```dataview
TABLE embed(link(meta(thumbnail).path, "500")) as ""
FROM "Posts"
WHERE published = true
AND thumbnail != undefined
```

JS implementation
```dataviewjs
dv.view(dv.pages('"Posts"')
    .where(p => p.thumbnail !== undefined)
    .map(item =>  item.thumbnail)
)
```

```dataviewjs
const filePath = (file) => {
    if (file === undefined) { return "none"}
    return file.startsWith("http") ?
        file :
        this.app.vault.adapter.getResourcePath(file)
}

// Todo [PageContentService.ts](https://github.com/tokenshift/obsidian-page-gallery/blob/8a113315218ffde554d1a6ae6cdc6ee5cbbdfdc3/src/PageContentService.ts#L109)

dv.table(["Name", "image", "image2"], dv.pages('"Posts"')
    .sort(item => item.file.name, 'asc')
    .map(item => [
        item.file.link,
        `${embed(link(meta(item.thumbnail).path, "500"))}`
    ])
)
```


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
        let bg = `url('${filePath(item.thumbnail)}')`;
        let link = dv.span("link", {cls: ["custom-gallery-title"]});
        return  dv.el("a", link, { cls: ["custom-gallery"], attr: {href: `${item.file.link}`, style: [`background-image: ${bg}`]}})
    })
```

%% Disable for website
```page-gallery
# Any options given at the root level of the configuration
# will be used as defaults for all views (but can be overridden
# in any individual view). 
title: "Posts Gallery"
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

%%

---
Last modified `$= dv.current().file.mtime`