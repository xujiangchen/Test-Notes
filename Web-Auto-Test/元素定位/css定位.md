## CSS 选择器

| 选择器                                                       | 例子                  | 例子描述                                             |
| ------------------------------------------------------------ | --------------------- | ---------------------------------------------------- |
| [.*class*]() | .intro | 选择 class="intro" 的所有元素。                      |
| [.*class1*.*class2*]()  | .name1.name2    | 选择 class 属性中同时有 name1 和 name2 的所有元素。  |
| [.*class1* .*class2* ]()    | .name1 .name2         | 选择作为类名 name1 元素后代的所有类名 name2 元素。   |
| [#*id*]()  | #firstname            | 选择 id="firstname" 的元素。                         |
| [*]()     | *                     | 选择所有元素。                                       |
| [*element*]() | p                     | 选择所有 `<p>` 元素。                                  |
| [*element*.*class*]() | p.intro               | 选择 class="intro" 的所有 `<p>` 元素。                 |
| [*element*,*element*]() | div, p                | 选择所有 `<div>` 元素和所有 `<p>` 元素。                 |
| [*element* *element*]() | div p                 | 选择 `<div>` 元素内的所有 `<p>` 元素。                   |
| [*element*>*element*]() | div > p               | 选择父元素是 `<div>` 的所有 `<p>` 元素。                 |
| [*element*+*element*]() | div + p               | 选择紧跟 `<div>` 元素的首个 `<p>` 元素。                 |
| [*element1*~*element2*]() | p ~ ul                | 选择前面有 `<p>` 元素的每个 `<ul>` 元素。                |
| [[*attribute*\]]() | [target]              | 选择带有 target 属性的所有元素。                     |
| [[*attribute*=*value*\]]() | [target=_blank]       | 选择带有 target="_blank" 属性的所有元素。            |
| [[*attribute*~=*value*\]]() | [title~=flower]       | 选择 title 属性包含单词 "flower" 的所有元素。        |
| [[*attribute*\|=*value*\]]() | [lang\|=en]           | 选择 lang 属性值以 "en" 开头的所有元素。             |
| [[*attribute*^=*value*\]]() | a[href^="https"]      | 选择其 src 属性值以 "https" 开头的每个 <a> 元素。    |
| [[*attribute*$=*value*\]]() | a[href$=".pdf"]       | 选择其 src 属性以 ".pdf" 结尾的所有 <a> 元素。       |
| [[*attribute**=*value*\]]() | a[href*="w3schools"]  | 选择其 href 属性值中包含 "abc" 子串的每个 <a> 元素。 |
| [:active]() | a:active              | 选择活动链接。                                       |
| [::after]() | p::after              | 在每个 `<p>` 的内容之后插入内容。                      |
| [::before]() | p::before             | 在每个 `<p>` 的内容之前插入内容。                      |
| [:checked]() | input:checked         | 选择每个被选中的 `<input>` 元素。                      |
| [:default]() | input:default         | 选择默认的 `<input>` 元素。                            |
| [:disabled]() | input:disabled        | 选择每个被禁用的 `<input>` 元素。                      |
| [:empty]() | p:empty               | 选择没有子元素的每个 `<p>` 元素（包括文本节点）。      |
| [:enabled]() | input:enabled         | 选择每个启用的 `<input>` 元素。                        |
| [:first-child]() | p:first-child         | 选择属于父元素的第一个子元素的每个 `<p>` 元素。        |
| [::first-letter]() | p::first-letter       | 选择每个 `<p>` 元素的首字母。                          |
| [::first-line]() | p::first-line         | 选择每个 `<p>` 元素的首行。                            |
| [:first-of-type]() | p:first-of-type       | 选择属于其父元素的首个 `<p>` 元素的每个 `<p>` 元素。     |
| [:focus]() | input:focus           | 选择获得焦点的 input 元素。                          |
| [:fullscreen]() | :fullscreen           | 选择处于全屏模式的元素。                             |
| [:hover]() | a:hover               | 选择鼠标指针位于其上的链接。                         |
| [:in-range]() | input:in-range        | 选择其值在指定范围内的 input 元素。                  |
| [:indeterminate]() | input:indeterminate   | 选择处于不确定状态的 input 元素。                    |
| [:invalid]() | input:invalid         | 选择具有无效值的所有 input 元素。                    |
| [:lang(*language*)]() | p:lang(it)            | 选择 lang 属性等于 "it"（意大利）的每个 `<p>` 元素。   |
| [:last-child]() | p:last-child          | 选择属于其父元素最后一个子元素每个 `<p>` 元素。        |
| [:last-of-type]() | p:last-of-type        | 选择属于其父元素的最后 `<p>` 元素的每个 `<p>` 元素。     |
| [:link]() | a:link                | 选择所有未访问过的链接。                             |
| [:not(*selector*)]() | :not(p)               | 选择非 `<p>` 元素的每个元素。                          |
| [:nth-child(*n*)]() | p:nth-child(2)        | 选择属于其父元素的第二个子元素的每个 `<p>` 元素。      |
| [:nth-last-child(*n*)]() | p:nth-last-child(2)   | 同上，从最后一个子元素开始计数。                     |
| [:nth-of-type(*n*)]() | p:nth-of-type(2)      | 选择属于其父元素第二个 `<p>` 元素的每个 `<p>` 元素。     |
| [:nth-last-of-type(*n*)]() | p:nth-last-of-type(2) | 同上，但是从最后一个子元素开始计数。                 |
| [:only-of-type]() | p:only-of-type        | 选择属于其父元素唯一的 `<p>` 元素的每个 `<p>` 元素。     |
| [:only-child]() | p:only-child          | 选择属于其父元素的唯一子元素的每个 `<p>` 元素。        |
| [:optional]() | input:optional        | 选择不带 "required" 属性的 input 元素。              |
| [:out-of-range]() | input:out-of-range    | 选择值超出指定范围的 input 元素。                    |
| [::placeholder]() | input::placeholder    | 选择已规定 "placeholder" 属性的 input 元素。         |
| [:read-only]() | input:read-only       | 选择已规定 "readonly" 属性的 input 元素。            |
| [:read-write]() | input:read-write      | 选择未规定 "readonly" 属性的 input 元素。            |
| [:required]() | input:required        | 选择已规定 "required" 属性的 input 元素。            |
| [:root]() | :root                 | 选择文档的根元素。                                   |
| [::selection]() | ::selection           | 选择用户已选取的元素部分。                           |
| [:target]() | #news:target          | 选择当前活动的 #news 元素。                          |
| [:valid]() | input:valid           | 选择带有有效值的所有 input 元素。                    |
| [:visited]() | a:visited             | 选择所有已访问的链接。                               |

## 实际案例

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
 <head>
  <title> css locate </title>
 </head>
 <body>
  <div class="formdiv">
    <form name="fnfn">
        <input name="username" type="text"></input>
        <input name="password" type="text"></input>
        <input name="continue" type="button"></input>
        <input name="cancel" type="button"></input>
        <input value="SYS123456" name="vid" type="text">
        <input value="ks10cf6d6" name="cid" type="text">
    </form>
    <div class="subdiv">
        <a href="http://www.baidu.com" id="baiduUrl">baidu</a>
        <ul id="recordlist">
            <p>Heading</p>
            <li>Cat</li>
            <li>Dog</li>
            <li>Car</li>
            <li>Goat</li>
        </ul>
    </div>
  </div>
 </body>
</html>
```



| **cssSelector**                             | **匹配**                                                    |
| ------------------------------------------- | ----------------------------------------------------------- |
| **css=div**                                 | `<div class="formdiv">`                                       |
| **css=div.formdiv**                         | `<div class="formdiv">`                                      |
| **css=#recordlist**   **css=ul#recordlist** | `<ul id="recordlist">`                                        |
| **css=div.subdiv p**                        | `<p>Heading</p>`                                              |
| **css=div.subdiv>ul>p**                     | `<p>Heading</p>`                                              |
| **css=form+div**                            | `<div class="subdiv">`                                        |
| **css=p+li**                                | `<li>Cat</li>`                                                |
| **css=p~li**                                | `<li>Cat</li>`  得到4个li中的第一个                           |
| **css=form>input[name=username]**           | `<input name="username" type="text"></input>`             |
| **css=input[name$=id][value^=SYS]**         | `<input value="SYS123456" name="vid" type="text">`            |
| **css=input[value\*='SYS']**                | `<input value="SYS123456" name="vid" type="text">`            |
| **css=a:link**                              | `<a href="http://www.baidu.com">baidu</a>`                    |
| **css=input:first-child**                   | `<input name="username" type="text"></input>`                 |
| **css=input:last-child**                    | `<input value="ks10cf6d6" name="cid" type="text">`            |
| **css=li:nth-child(2)**                     | `<li>Cat</li>` 因为这个li是ul下的第二个元素，所以是child（2） |
| **css=:root**                               | `<html>`                                                      |

