---
title: 一行命令自动更换ip并输出
date: 2019-05-23 22:46:07
spoiler: 省略.
cta: 'other'
---

### 1. 获取本地ip地址
<!-- more -->
```jsx
let os = require('os')
const ifaces = os.networkInterfaces()
console.log(ifaces)

Object.keys(ifaces).forEach((ifname) => {
    // var alias = 0
    ifaces[ifname].forEach(iface => {
        if ('IPv4' !== iface.family || iface.internal !== false) {
            return
        }
        console.log(ifname, iface.address)
        // if (alias >= 1) {
        //     console.log(ifname + ':' + alias, iface.address)
        // } else {
        //     console.log(ifname, iface.address)
        // }
        // ++alias
    })
})
```
### 2. 修改config文件
### 3. 输出地址