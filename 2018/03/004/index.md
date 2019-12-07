# scrapy语法


## xpath 语法

| 表达式       | 说明                                     |
| ------------ | ---------------------------------------- |
| article      | 选取所有 article 元素的所有子节点        |
| /article     | 选取根元素 article                       |
| article/a    | 选取所有属于 article 的子元素的 a 元素   |
| //div        | 选取所有 div 子元素                      |
| article//div | 选取所有属于 article 元素的后代 div 元素 |
| //@class     | 选取所有名为 class 的属性                |

## xpath 语法-谓语

| 表达式               | 说明                                         |
| -------------------- | -------------------------------------------- |
| /article/div[1]      | 选取属于 article 子元素的第一个 div 元素     |
| /article/div[last()] | 选取属于 article 子元素的最后一个 div 元素   |
| /article/div[last()] | 选取属于 article 子元素的倒数第二个 div 元素 |
| //div[@lang]         | 选取所有拥有 lang 属性的 div 元素            |
| //div[@lang='eng']   | 选取所有 lang 属性为 eng 的 div 元素         |

## xpath 语法-其他

| 表达式             | 说明                            |
| ------------------ | ------------------------------- |
| /div/\*            | 选取属于 div 元素的所有子节点   |
| //\*               | 选取所有元素                    |
| //div[@*]          | 选取所有带属性的 div 元素       |
| //div/a \| //div/p | 选取所有 div 元素的 a 和 p 元素 |
| //span \| //ul     | 选取文档中的 span 和 ul 元素    |

## css 选择器

| 表达式             | 说明                                           |
| ------------------ | ---------------------------------------------- |
| \*                 | 选择所有节点                                   |
| \#contriner        | 选择 id 为 contriner 的节点                    |
| .container         | 选取所有 class 包含 container 的节点           |
| li a               | 选取所有 li 下的所有 a 节点                    |
| ul + p             | 选择 ul 后面的第一个 p 元素                    |
| div#container > ul | 选取 id 为 container 的 div 的第一个 ul 子元素 |

## css 选择器进阶

| 表达式                    | 说明                                   |
| ------------------------- | -------------------------------------- |
| ul ~ p                    | 选取与 ul 相邻的所有 p 元素            |
| a[title]                  | 选取所有有 title 属性的 a 元素         |
| a[href="xxx"]             | 选取所有 href 属性为 xxx 值的 a 元素   |
| a[href*="xxx"]            | 选取所有 href 属性包含 xxx 的 a 元素   |
| a[href^="xxx"]            | 选择所有 href 属性以 xxx 开头的 a 元素 |
| a[href$=".jpg"]           | 选取所有 href 属性以.jpg 结尾的 a 元素 |
| input[type=radio]:checked | 选择选中的 radio 的元素                |
| div:not(#container)       | 选取所有 id 非 container 的 div 属性   |
| li:nth-child(3)           | 选取第三个 li 元素                     |
| tr:nth-child(2n)          | 第偶数个 tr                            |
