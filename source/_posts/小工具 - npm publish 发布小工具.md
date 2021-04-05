---
title: npm publish 发布小工具
author: GleanWu
tags:
  - Node
---

> 自己在写 npm 发布的时候，发现还有手动改版本号，所以自己就写了一个小工具 一键改版本号并发布

```javascript
// publish.js
/**
 * npm publish 发布工具
 */
const semver = require('semver')
const shell = require('shelljs')
const path = require('path')
const fs = require('fs')
const resolve = (dir) => {
  return path.normalize(process.cwd() + '/' + dir)
}
const packagepath = resolve('/package.json')
const packageConfig = require(packagepath)
let currentVersion = packageConfig.version
const nextVersion = semver.inc(currentVersion, 'prerelease', 'beta')

const changeVersion = (file) => {
  return new Promise((resolve, reject) => {
    fs.readFile(packagepath, function(err, data) {
      if (err) {
        reject(err)
      }
      let packageText = data.toString()
      packageText = JSON.parse(packageText)
      packageText.version = nextVersion
      const packageTextNew = JSON.stringify(packageText, null, 2)
      fs.writeFile('./package.json', packageTextNew, function(err) {
        if (err) {
          reject(err)
        }
        resolve('修改成功' + nextVersion)
      })
    })
  })
}

/**
 * 脚本执行回调
 */
const shellCallback = (shell_val) => {
  console.log(shell_val)
  return new Promise((resolve, reject) => {
    shell.exec(shell_val) === 0 ? resolve(shell_val + ' Success') : reject(shell_val + shell.exec(shell_val))
  })
}
/**
 * 发布新版本
 */
const publishFun = () => {
  shellCallback('npm publish').then(
    (res) => {
      console.log(res)
    },
    (err) => {
      console.log(err)
      shell.exit(1)
    }
  )
}
/**
 * 执行文件成功后执行push
 */
const gitFun = () => {
  const br = shell.exec('git branch | grep "*"').split(' ')[1] // 获取当前分支
  shell.exec('git add .')
  shell.exec('git commit -m"feat: newversion"')
  shell.exec(`git pull origin ${br}`)
  shell.exec('git add .')
  shell.exec('git commit -m"feat: newversion"')
  shell.exec(`git push origin ${br}`)
  if (process.argv[2] && process.argv[2] == '-p') {
    publishFun()
  }
  return true
}

console.log(process.argv)
if (process.argv[2] && process.argv[2] == '-p') {
  changeVersion().then(
    (res) => {
      console.log(res)
      gitFun()
    },
    (err) => {
      console.log(JSON.stringify(err))
    }
  )
} else {
  gitFun()
}
```

使用的时候直接`node publish.js` 就可以了 这个是一个很基础的版本；大家可以在这个基础上进行好多的扩展，比如同步 git 仓库代码出现异常或者想要废弃指定版本都是可以的；

我的思路很简单，就是通过`node`的`shell`这库，调用相应的`shell`脚本进行实现，下期介绍另一个小工具，一键发布部署；因为自己在搞博客，就捣鼓一些能为自己节省时间的小工具，思路都比较简单；但是个人觉得挺有意思
