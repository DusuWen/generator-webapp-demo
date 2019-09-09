# generator-webapp-demo
一个静态前端工程模板，使用gulp提供开发时热重载编译预览，生产时混淆压缩打包。
### 开始

准备:

* Node.js
* Npm

开始开发，运行：

```shell
$ npm run start
```

工具会在本地起一个服务，自动在默认浏览器打开，并且监听文件变化重新编译加载页面。

在浏览器里测试，运行以下指令：

```shell
$ npm run serve:test
```

为生产环境打包，运行以下指令：

```shell
$ npm run build
```

预览检查生产环境所打的包，运行以下指令：

```shell
$ npm run serve:dist
```

### 文件目录

```text
|- /.tmp  // 存放serve时的临时编译文件
|- /app   // 存放开发文件
|- /dist  // 存放build时的生产文件
|- /test  // 存放test文件
|- gitignore
|- favicon.ico
|- gulpfile.js // gulp流配置文件
|- package.json
|- package-lock.json
|- README.md
```

## 任务列表

获得可执行任务列表, 运行以下指令:

```sh
$ npm run tasks
```

## 获取gulp插件

Gulp plugins (the ones that begin with `gulp-`) don't have to be explicitly imported. They are automatically picked up by [gulp-load-plugins] and available through the `$` variable.

## 浏览器支持

You can configure browser support for Autoprefixer and @babel/preset-env by modifying the [browserslist] configuration, which in this case is the `browserslist` field in your `package.json`.

### Modernizr

`modernizr.json` contains Modernizr configuration. You can use [this file][modernizr-config-all] as a reference for all available options.

## 代码风格

We use ESLint for linting JavaScript code. You can define rules in your `package.json` under the `eslintConfig` field. Alternatively, you can add an `.eslintrc` file to your project root, where you can [configure][eslint-config] ESLint using JavaScript, JSON or YAML.

###  `no-undef` 规则 和 测试

The ESLint rule [`no-undef`][no-undef] will warn about usage of explicitly undeclared variables and functions. Because our tests use global functions like `describe` and `it` (defined by the testing framework), ESLint will consider those as warnings.

Luckily, the fix is easy—add an `.eslintrc` file to the `test/spec` directory and let ESLint know about your testing framework. For example, if you're using Mocha, add this to `.eslintrc`:

```json
{
  "env": {
    "mocha": true
  }
}
```

Configuration from this `.eslintrc` will merge with your project-wide configuration.

## 开发

We use the `.tmp` directory mostly for compiling assets like SCSS files. It has precedence over `app`, so if you had an `app/index.html` template compiling to `.tmp/index.html`, http://localhost:9000 would point to `.tmp/index.html`, which is what we want.

This system can be a little confusing with the `watch` task, but it's actually pretty simple:

* notify LiveReload when compiled assets change
* run the compile task when source assets change

E.g. if you have Less files, you would want to notify LiveReload when Less files have compiled, i.e. when `.tmp/styles/**/*.css` change, but you would want to compile Less files by running the `styles` task when source files change, i.e. `app/styles/**/*.less`.

### 添加新资源

#### Sass

A common practice is to have a single, "main", Sass file, then use `@import` statements to add other partials. For example, let's say you created stylesheet for your navigation, `app/styles/_nav.scss`, you can then import it in `app/styles/main.scss` like this:

```scss
@import "nav";
```

#### JavaScript

Our build step uses special `build` comment blocks to mark which assets to concatenate and compress for production. You can see them at the top and bottom of `app/index.html`.

You have to add your own JS files manually. For example, let's say you created `app/scripts/nav.js`, defining some special behavior for the navigation. You should then include it in the comment blocks for your _source_ JS files, where `app/scripts/main.js` is located:

```html
<!-- build:js scripts/main.js -->
<script src="scripts/main.js"></script>
<script src="scripts/nav.js"></script>
<!-- endbuild -->
```

Upon build these will be concatenated and compressed into a single file `scripts/main.js`.

The file name in the comment block and the first source aren't related, their name being the same is a pure coincidence. The file name in the comment block specifies how the final optimized file will be called, while the sources should map to your source files.

## 调试 `gulpfile.js`

Gulp tasks are not meant to be run directly, but instead through npm scripts. However, sometimes you want to run a tasks in order to debug it. If you don't have Gulp install globally, you can run the local CLI using `npx gulp`, so this is how you would run the `lint` task:

```sh
$ npx gulp lint
```

Keep in mind that only exported tasks are available to the CLI:

```js
function myPrivateTask() {
  // not available to CLI
}

function myPublicTask() {
}

// available to CLI as "myPublicTask"
exports.myPublicTask = myPublicTask
```

[gulp]: https://github.com/gulpjs/gulp
[gulp-docs]: https://gulpjs.com/docs/en/getting-started/quick-start
[yo]: https://github.com/yeoman/yo
[gulp-load-plugins]: https://github.com/jackfranklin/gulp-load-plugins
[browserslist]: https://github.com/browserslist/browserslist
[modernizr-config-all]: https://github.com/Modernizr/Modernizr/blob/master/lib/config-all.json
[eslint-config]: https://eslint.org/docs/user-guide/configuring
[no-undef]: https://eslint.org/docs/rules/no-undef
