# stylelint 搭配 stylelint-order，更随心所欲的编码 CSS

## 为什么需要校验 CSS 规则

对于编程语言进行「语法、书写」校验，能有效「归并」不同开发者的「不同风格」，还能检验出一些语法错误。  
比如 [eslint](https://github.com/eslint/eslint) 就能校验 JS 代码的「鸡肋糟粕」。  
对于 CSS 而言，不能算是严格意义的「编程语言」，但是在前端体系中却不能小觑。  
CSS 是以描述为主的「样式表」，如果「描述」得「混乱、没有规则」，对于其他开发者一定是一个「定时炸弹 💣『特别是有强迫症的人群 😊』」  
CSS 看似简单，想要写出漂亮的 CSS 还是相当困难。[CSS 为什么这么难学？ - CSS 不是科学，而是艺术](https://zhuanlan.zhihu.com/p/29888231)  
所以校验 CSS 规则的行动迫在眉睫，立即执行。

## 团队协作在 CSS 书写遇到的问题

请看以下场景：

小冯：你的 CSS 为什么不把 `0.1` 写成 `.1`  
小杰：CSS 解析器一样能识别，不好较真好么  
小冯：好吧 😨，那为什么你的逗号后面没有空格，我看着很难受啊  
小杰：我看着不难受就好  
小冯：😨😨😨，那你能不能不要新建一个空的 CSS 文件啊！！！  
…

不论是在社区、MR、平时交流中，相似的场景层出不穷，  
这就是因为 CSS 规则不统一，导致的弊端「冰山一角」

## CSS 哪些东西需要校验

单纯从代码层面来说，CSS 校验的东西其实蛮少的。  
比如：属性顺序、小于 1 的小数要不要去掉 0、选择器之间要不要加空格…  
不过要细细的追究，校验的东西还是挺多的，比如 [List of rules](https://stylelint.io/user-guide/rules/#list-of-rules) 列出了好多需要校验的规则。

叮叮叮~~~，有个东西要说一下，CSS 语言本身对「规则」不敏感，几乎是你想怎么写就怎么写，只要合乎「语法」。

## 怎么校验 CSS 规则

首先得有一个规则，其次开发者得遵守规则。  
如何遵守：

1. 提交 「Merge Request」的时候，以「Code Review」的形式「人工校验」。「好蠢啊，费时费力，效果差」
1. `git commit` 的时候「自动校验」，校验通过才能提交成功「(＾－＾)V 真好~~~」

### 通过 stylelint 校验 CSS 规则

#### 简单步骤

- 安装 [stylelint](https://github.com/stylelint/stylelint)、[stylelint-order](https://github.com/hudochenkov/stylelint-order)、[stylelint-config-standard](https://github.com/stylelint/stylelint-config-standard)

`npm i --save-dev stylelint stylelint-order stylelint-config-standard`

- 增加 stylelint 配置文件

项目根目录添加文件 `.stylelintrc` 基本配置文件：

```json
{
  "extends": "stylelint-config-standard",
  "plugins": [],
  "rules": {}
}
```

具体的配置文件内容，欢迎参考：[点我呀](https://raw.githubusercontent.com/hangyangws/article/master/src/data/.stylelintrc)  
注：配置文件使用的 CSS 属性排序规则来自 [这里](https://github.com/Wizard67/note-css-order#properties-属性)

- 在 package.json 的 scripts 字段中添加相关命令

```json
{
  "scripts": {
    "lint-css": "stylelint 'src/**/*.css' --fix",
  }
}
```

这样就可以手动执行 `npm run lint-css` 校验 CSS 了。  
`'src/**/*.css'` 以 glob 语法表示 CSS 文件的路径。  
`--fix` 表示让 stylelint 尽可能的自动修复 CSS 代码「部分规则还是需要抛出错误，开发者手动修复」

- 安装 [lint-staged](https://github.com/okonet/lint-staged)、[husky](https://github.com/typicode/husky)

`npm i --save-dev lint-staged husky`

- 增加 lint-staged 配置文件

项目根目录添加文件 `.lintstagedrc` 基本配置文件：

```json
{
  "src/**/*.css": [
    "stylelint --fix",
    "git add"
  ]
}
```

这样就会在执行 `git commit` 之前会自动以 `stylelint --fix` 的方式校验 `src/**/*.css` CSS 文件

#### stylelint 的更多使用方式

stylelint 不仅仅可以用于项目中，还可以用于编辑器，比如「Sublime Text」，详细使用规则，这里不赘述。 [移步阅读](https://stylelint.io/)

## 写在最后

虽然有各种各样的工具能「辅佐」开发者工作，注意，是「辅佐」不是「帮助」。  
因为开发者自己需要明确「为什么」要这样校验，我们不能被工具「牵着鼻子走」，是我们「命令」工具这样校验。  
嗯，这点很重要。  
不然别人问这样做的好处，千万不要「一脸茫然」。

---

·感谢阅读·
