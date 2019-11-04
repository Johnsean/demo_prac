# Lesson 45 webpack 工程化

 流水线式 ：  `node sass/scss`    `node bable` `aotuprefixed`

> 1.  sass/scss 等 ==》 css       `/ -w`   sass/scss 保存后自动生成 css
> 2.  js  ES6   ==> ES5       `/-w` 同上
> 3.  css自动补前缀  ---- 照顾兼容性
> 4.   . . . . .等等

在 webpack 工具 就是在上述类 自动化工具 集合在webpack 一行命令解决

> 不用开那么多窗口 



尝试 使用 ***parceljs***



***node sass/scss***    : `npm install node-sass `  

`./node_modules/.bin/node-sass -v`

`./node_modules/.bin/node-sass src/main.scss  dist/css/main.css`

`./node_modules/.bin/node-sass src/main.scss -w dist/css/main.css`

---

***node bable*** :

`npm install --save-dev @babel/core @babel/cli`

`npm init`  生成`package.json`

``` javascript
{
  "devDependencies": {
+   "@babel/cli": "^7.0.0",
+   "@babel/core": "^7.0.0"
  },
+   "scripts": {
+     "build": "babel src -d lib"
+   },
}
```

`npm run build`   run 上面的script   目录src 翻译到 lib目录下



--src  source未翻译   --dist  distribution 待发布代码  --vendors 第三方

--node_modules 第三方包

