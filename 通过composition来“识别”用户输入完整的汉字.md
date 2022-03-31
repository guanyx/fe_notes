# 如何识别Input输入的是完整的汉字

用“识别”其实不是很准确，也可以换一种说法，怎么让 `Input` 输入框只返回完整的汉字

## 典型场景
![百度搜索示例](https://github.com/guanyx/fe_notes/blob/main/images/2022.03.31%E7%99%BE%E5%BA%A6%E6%90%9C%E7%B4%A2%E8%AF%8D%E7%A4%BA%E4%BE%8B.png?raw=true)

<img alt="百度关联联想请求数" src="https://github.com/guanyx/fe_notes/blob/main/images/2022.03.31%E7%99%BE%E5%BA%A6%E5%85%B3%E8%81%94%E8%81%94%E6%83%B3%E8%AF%B7%E6%B1%82%E6%95%B0.png?raw=true" height="160px" /> <img alt="百度关联联想请求示例1" src="https://github.com/guanyx/fe_notes/blob/main/images/2022.03.31%E7%99%BE%E5%BA%A6%E5%85%B3%E8%81%94%E8%81%94%E6%83%B3%E8%AF%B7%E6%B1%82%E7%A4%BA%E4%BE%8B1.png?raw=true" height="160px" /> <img alt="百度关联联想请求示例2" src="https://github.com/guanyx/fe_notes/blob/main/images/2022.03.31%E7%99%BE%E5%BA%A6%E5%85%B3%E8%81%94%E8%81%94%E6%83%B3%E8%AF%B7%E6%B1%82%E7%A4%BA%E4%BE%8B2.png?raw=true" height="160px" /> <img alt="百度关联联想请求示例3" src="https://github.com/guanyx/fe_notes/blob/main/images/2022.03.31%E7%99%BE%E5%BA%A6%E5%85%B3%E8%81%94%E8%81%94%E6%83%B3%E8%AF%B7%E6%B1%82%E7%A4%BA%E4%BE%8B3.png?raw=true" height="160px" />

在关联搜索的场景下，无论加不加 节流(throttle)/防抖(debounce)，如果只在 [input](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/input_event) 事件里处理用户输入，难免都会发起如截图中的请求，可以到[百度](https://www.baidu.com/)体验一下

## 只想得到完整的汉字要怎么办？
[compositionstart](https://developer.mozilla.org/en-US/docs/Web/API/Element/compositionstart_event)

[compositionupdate](https://developer.mozilla.org/en-US/docs/Web/API/Element/compositionupdate_event)

[compositionend](https://developer.mozilla.org/en-US/docs/Web/API/Element/compositionend_event)

以上这三个事件，就是专门为了处理 *拼音输入法* 这一类的输入法而定义的

可以看一个简单的示例

```html
<input id="search" />
我输入了:<span id="result"></span>
```

```js
const input = document.getElementById("search");
const result = document.getElementById("result");

input.addEventListener('compositionend', (event) => {
  result.textContent = event.target.value
});
```

[CodePen](https://codepen.io/guanyx/pen/PoEJNzE)

## 拼音输入法和英文输入法切换

真实的用户输入，通常会在两种输入法之间切换，如何处理？

示例就不放了，大概就是 `compositionstart` 事件里加一个标记状态，`input` 事件中只处理非 `composition` 的情况，也就是英文输入法

