# 常用语言正则特性一览表

本页内容节选自[《正则指引》（第2版）](https://book.douban.com/subject/30352656/)

## 字符组 (character class)

特性 | .NET | Java | JavaScript | PHP | Python | Ruby | Objective-C | Golang |
---|:---:|:---:|:-:|:--:|:--:|:------:|:----:|:--:|
`\Q…\E`| × | √ | × |  √ | × |  ×      |    √  | √   |
`\w \s \d`| 说明[^1] | ASCII匹配规则 | ASCII匹配规则，`\s`除外 | ASCII匹配规则 | 说明[^2] |  ASCII匹配规则 | Unicode匹配规则 | ASCII匹配规则   |
POSIX字符组| × | ASCII字符 | x | ASCII字符 | x | 说明[^3] | x | ASCII字符   |
   

[^1]: 可以指定 **RegexOptions.ECMAScript** 恢复到ASCII匹配规则

[^2]: Python 2默认采用ASCII匹配规则，但指定 **Re.U** 模式可以变换为Unicode匹配规则；Python 3默认采用Unicode匹配规则，但指定 **Re.A** 模式可以变换为ASCII匹配规则

[^3]: Ruby 1.9的POSIX字符组支持Unicode字符，且 **不需要** 显式指定Unicode模式

## 括号相关 (parentheses)

特性 | .NET | Java | JavaScript | PHP | Python | Ruby | Objective-C | Golang |
---|:---:|:---:|:-:|:--:|:--:|:------:|:----:|:--:|
表达式中使用`\1`到 `\9` 引用分组|√|√|√|√|√|√|√|√|
替换中引用分组|`$num`|`$num`|`$num`|`$num`|`\g<num>`|`\num`|`$num`|`$num`|
命名分组|说明[^4]|x|x|说明[^5]|说明[^6]|说明[^7]|说明[^8]|说明[^9]|

[^4]: .NET中用 `(?\<name>regex)` 表示命名分组，用 `\k<name>` 在表达式中引用分组，替换中用 `${name}` 引用命名分组

[^5]: PHP中用 `?P<name>regex)` 表示命名分组，用 `(P=name)` 在表达式中引用分组，替换中 **不能** 引用命名分组

[^6]: Python中用 `(?P<name>regex)` 表示命名分组，用 `(P=name)` 在表达式中引用分组，替换中用 `\g<name>` 引用命名分组

[^7]: 只有Ruby 1.9支持命名分组，用 `(?<name>regex)` 表示命名分组，用 `\k<name>` 在表达式中引用分组，替换中用 `\k<name>` 引用命名分组

[^8]: Objective-C中用 `(?<name>regex)` 表示命名分组，用 `\k<name>`在表达式中引用分组，在替换中引用的方法**未知**

[^9]: Golang中用 `(?P<name>regex)` 表示命名分组，命名分组的反向引用和在替换中引用的方法**未知**


# 断言 (assertion)

特性 | .NET | Java | JavaScript | PHP | Python | Ruby | Objective-C | Golang |
---|:---:|:---:|:-:|:--:|:--:|:------:|:----:|:--:|
`\b`（与`\w`规则相同）| √|说明[^10]| √| √| √|说明[^11]|说明[^12]| √|  
^| √| √| √| √| √|说明[^13]| √| √|
`$`| √| √|说明[^14]| √| √|同上| √| √|
`\A`| √| √| ×| √| √| √| √| √|  
`\z`| √| √| ×| √| ×| √| √| √|  
`\Z`| √| √| ×| √|说明[^15]| √| √| ×|  
`(?=regex)` `(?!regex)`| √| √| √| √| √| √| √| ×|  
`(?<=regex)` `(?<!regex)`| √|说明[^16]|说明[^17]|说明[^18]|说明[^19]|说明[^20]|说明[^21]| ×|

[^10]: Java中的 `\w` 采用 **ASCII匹配规则**，但 `\b` 采用 **Unicode匹配规则**

[^11]: Ruby 1.9中的 `\b` 采用 **Unicode匹配规则**

[^12]: Objective-C中 `\b` 和 `\w` 默认采用 **Unicode匹配规则**，如果指定ASCII匹配规则，受影响的**只有** `\b`

[^13]: Ruby默认采用多行模式，`^` 和 `$` **可以匹配** 行的起始或结束位置

[^14]: JavaScript中的 `$` **无法匹配** 文本末尾行结束符之前的位置

[^15]: Python中的 `\Z` 等价于其他语言中的 `\z` 

[^16]: 逆序环视中的正则表达式能匹配的文本长度**必须有上限**

[^17]: JavaScript必须到 **ES2017(TC39)**之后才支持逆序环视，此时逆序环视的表达式**没有限制**

[^18]: PHP中的逆序环视中的正则表达式匹配文本的长度**可以不确定**，可以是若干个值，但**必须是固定值**，多选结构**只能出现在顶层**

[^19]: Python的逆序环视中的正则表达式匹配的文本长度必须是固定的
 
[^20]: Ruby 1.8 **不支持** 逆序环视，Ruby 1.9的逆序环视中的正则表达式匹配的文本长度 **必须是固定的** 
 
[^21]: 逆序环视中的正则表达式能匹配的文本长度**必须有上限**


# 匹配模式 (match mode)

特性 | .NET | Java | JavaScript | PHP | Python | Ruby | Objective-C | Golang |
---|:---:|:---:|:-:|:--:|:--:|:------:|:----:|:--:|
不区分大小写模式 `i`| √| √| √| √| √| √| √| √| 
单行模式 `s`| √| √| √| √| √| √| √| √| 
多行模式 `m`| √| √| √| √| √| [^22]| √| √| 
注释模式 `x`| √| √| ×| √| √| √| √| ×| 
`(?-modifier)`停用模式| √| √| ×| √| ×| √| √| √| 
`(?modifier:regex)`模式仅限括号内表达式| √| √| ×| √| ×| √| √| √| | 

[^22]: Ruby **默认采用多行模式**

# Unicode

特性 | .NET | Java | JavaScript | PHP | Python | Ruby | Objective-C | Golang |
---|:---:|:---:|:-:|:--:|:--:|:------:|:----:|:--:|
Unicode Property| √| √| ×| √| ×|[^23] | √| √| 
Unicode Script| ×| ×| ×| √| ×|同上|无文档但可用|无文档但可用|
Unicode Block| √| √| ×| ×| ×| ×| ×| ×| 

[^23]: 只有Ruby 1.9支持，使用时需 **显式指定Unicode模式**
