# JustinWyne.com
    
```dataviewjs
let nav = dv.pages().where(p => p.topnav > 0).sort( p => p.topnav)
let u = nav.file.map(f => {
    if (f.name == dv.current().file.name) {
        return f.name
    } else {
        return f.link
    }
})
dv.el("span", u.join(" | "))
```

---