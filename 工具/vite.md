## Vite

### build react

构建命令：
`npm create vite@latest app-name`

注意：
选择framework：react
选择variant：react-js + swc（swc是一个构建工具，用Rust写的）

#### 结构目录

- public
  - vite.svg
- src
  - assets
    - react.svg
  - App.css
  - App.jsx
  - index.css
  - main.jsx
- .eslintrc.cjs
- .gitignore
- index.html
- package.json
- README.md
- vite.config.js

#### 设置别名

即在包引用的时候可以使用`@`来代替`src`的path路径
需要修改`vite.config.js`文件
```
export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': '/src'
    }
  }
})
```

### 本地启动

`npm install`
`npm run dev`