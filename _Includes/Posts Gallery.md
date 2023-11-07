```dataviewjs
const filePath = (file) => {
    if (file === undefined) { return "none"}
    return file.startsWith("http") ?
        file :
        this.app.vault.adapter.getResourcePath(file)
}

dv.pages('"Posts"').where(p => p.thumbnail !== undefined)
    .sort(item => item.file.name, 'desc')
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
