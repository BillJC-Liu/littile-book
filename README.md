# 小本子
## 作用：
  记录工作中遇到的一些点

## 1.引入文件中所有的 `.js` 文件方法
场景：将每一项`action`引入`store`中，平时我们需要`import`所有`action`文件，这样很麻烦。使用webpack提供的`require.context`即引入所有想要引入的文件。
```javascript
  // 比如我们的 action 文件在 modules 文件夹下
  // https://webpack.js.org/guides/dependency-management/#requirecontext
  // require.context  string  boolean  Reg  返回的是一个方法，该方法接收一个字符串参数
  // 导出的功能有3个属性：resolve，keys，id
  // resolve 是一个方法，返回已解析请求的模块ID
  // keys 是一个方法，他返回上下文可以可以处理的所有可能请求的数据
  const modulesFiles = require.context('./modules' , true , /\.js$/)
  // 数组  ['./app','./login','./home']
  const modulesKeys =  modulesFiles.keys()

  const modules  = modulesKeys.reduce((modules,modulePath)=>{
    // 将 keys 中的字符串中的逗号去掉
    // "$1" 的意思是匹配第一个表达式的内容，也就是(.*)匹配的内容，拿到 'app'  'login' 'home'这样的字符串
    const moduleName = modulePath.replace(/\.\/(.*)\.\w+$/ , "$1")
    // 去moduleFiles 缓存中拿到解析的文件
    const value = modulesFiles(modulePath)
    // value.default 是文件中 default 的内容
    modules[moduleName] = value.default
    return modules
  })

  // modules => {
  //   app: {... app 文件中default的内容},
  //   login: {... login 文件中default的内容},
  //   home: {... home 文件中default的内容},
  // }
```

