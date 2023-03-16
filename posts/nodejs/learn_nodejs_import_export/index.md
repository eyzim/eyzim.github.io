# [NodeJS] Import 和 export 程式碼


當程式內容越來越多，我們通常會把原本寫在同一個檔案的 code，分割成許多個文件，不用一直上下滾動滑鼠，方便查看內容。

因此，就遇到了這樣的問題：export 和 import

首先，我們會有一個很長很長的 `index.js`

```javascript
/* index.js */
let saySomething = (input) => {
	console.log(`\nI would like to say ${input} :)\n`);
};

let main = (talk) => {
	saySomething(talk);
};

main(process.argv[2]);
```

```
> node .\index.js exportSomething
Debugger attached.

I would like to say exportSomething :)

Waiting for the debugger to disconnect...
```

目前這樣看起來沒甚麼問題，但如果同一支 `index.js` 裡面塞了 1000 個 function，每次光是尋找相同的變數就要花很多時間查看，這時候就要把 `index.js` 依照各個 function 功能拆分許多檔案了。

我們的目標是將 `saySomething` 這個 function 移到別的檔案。

## 分割

首先，先來拆解檔案

`index.js` 變成

```javascript
/* index.js */
let main = (talk) => {
	saySomething(talk);
};

main(process.argv[2]);
```

新增另一個檔案 `say.js`

```javascript
/* say.js */
let saySomething = (input) => {
	console.log(`\nI would like to say ${input} :)\n`);
};
```

## export

再來是把 `say.js` 中的 `saySomething` 這個 function export

{{< admonition type=info title="export">}}
module.exports = { functionA, functionB };
{{< /admonition >}}

`say.js` 加入 export

```javascript
/* say.js */
let saySomething = (input) => {
	console.log(`\nI would like to say ${input} :)\n`);
};

module.exports = { saySomething };
```

## import

再來就要 import 啦，根據 [mdn 的教學](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Statements/import) 這時的 code 長這樣

`index.js`

```javascript
/* index.js */
import { saySomething } from "./say";

let main = (talk) => {
	saySomething(talk);
};

main(process.argv[2]);
```

`say.js`

```javascript
/* say.js */
let saySomething = (input) => {
	console.log(`\nI would like to say ${input} :)\n`);
};

module.exports = { saySomething };
```

然後就跳出了

```
> node .\index.js exportSomething

Debugger attached.
(node:26088) Warning: To load an ES module, set "type": "module" in the package.json or use the .mjs extension.
(Use `node --trace-warnings ...` to show where the warning was created)
Waiting for the debugger to disconnect...
D:\eyzim\index.js:1
import { saySomething } from "./say";
^^^^^^

SyntaxError: Cannot use import statement outside a module
at Object.compileFunction (node:vm:360:18)
at wrapSafe (node:internal/modules/cjs/loader:1088:15)
at Module.\_compile (node:internal/modules/cjs/loader:1123:27)
at Module.\_extensions..js (node:internal/modules/cjs/loader:1213:10)
at Module.load (node:internal/modules/cjs/loader:1037:32)
at Module.\_load (node:internal/modules/cjs/loader:878:12)
at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:81:12)
at node:internal/main/run_main_module:23:47

```

outside a module 是甚麼???

靠關鍵字大法：在 `package.json` 中加入

```json
"type": "module",
```

沒用。

還是一樣 outside a module。

每個檔案是一個單獨的 module，如果我想要在 `index.js` 中使用 `say.js`，必須將他們 bundle / 捆包起來，目前看起來只是單獨執行一個檔案而已。

## import 改成 require

那麼，我們換個做法！

(記得把剛剛在 `package.json` 中的修改移除)

{{< admonition type=info title="export">}}
const { functionA, functionB } = require("./subFile");
{{< /admonition >}}

`index.js`

```javascript
/* index.js */
const { saySomething } = require("./say");

let main = (talk) => {
	saySomething(talk);
};

main(process.argv[2]);
```

`say.js`

```javascript
/* say.js */
let saySomething = (input) => {
	console.log(`\nI would like to say ${input} :)\n`);
};

module.exports = { saySomething };
```

終於成功了，卡在這種問題上很困擾呀。

結論就是 `modules.export = { A }` 配上 ` const { A } = require("./檔案A")`

## Reference

-   [MDN export](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Statements/export)
-   [MDN import](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Statements/import)

