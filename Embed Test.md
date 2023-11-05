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
AND thumbnail != undefined
```


#### JS implementation (dv.paragraph)
```dataviewjs
dv.pages('"Posts"')
    .where(p => p.thumbnail !== undefined)
    .map(item => {
        return dv.el(
            "a",
            dv.paragraph(`![[${item.thumbnail.path}|100]]`),
            {
                attr: { href: item.file.path },
                cls: ["internal-link"]
            }
        )
    })
```

#### JS With `getResourcePath`
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
        //let link = dv.span("link", {cls: ["custom-gallery-title"]});
        let link = "Link text"
        return  dv.el("a", link, { cls: ["custom-gallery", "internal-link"], attr: {href: `${item.file.path}`, style: [`background-image: ${bg}`]}})
    })
```


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
