---
title: vscode-自定义代码片段
date: 2021-03-28 21:05:01
categories:
  - vscode
tags:
  - vscode
---



## 配置路径

在Windows10下的文件位置

用户定义的补全：

> %HOMEPATH%\AppData\Roaming\Code\User\snippets

vscode内置的补全：
> Microsoft VS Code\resources\app\extensions\关联的语言\snippets



## 配置说明

打开后会有一个示例，如：

```json
"Print to console": {
	"prefix": "log",
		"body": [
			"console.log('$1');",
            "$2"
        ],
	"description": "Log output to console"
    }
```

- `Print to console` 代码片段名称；
- `prefix` 插件前缀；
- `body` 插件内容可以是字符串，也可以为数组，若为数组每个元素都做为单独的一行插入；
- `description` 插件描述。



## Snippet 语法

制表位(Tabstops)：

> 使用制表位可以在代码片段中移动光标位置（如使用Tab切换到下一个位置），使用$1,$2来指定光标的位置，数字代表光标的移动的顺序，$0代表光标的最后位置。如果有多个相同的制表位会在编译器里同时出现多个光标（类似编译器的块编辑模式）。

占位符(Placeholders)

> 占位符是带默认值的制表位，占位符的文本会被插入到制表位所在位置并且全选以方便修改，占位符可以嵌套使用，比如${1:another ${2:placeholder}}。

选择项(Choice)

> 占位符可以有多选值，每个选项的值用`逗号`分隔，选项的开始和结束用管道符号（|）将选项包含，例如: ${1|one,two,three|}，当插入代码片段，选择制制表位的时候，会列出选项供用户选择。

变量(Variables)

> 使用`$name`或者`${name|default}`可以插入变量的值，如果变量未被赋值则插入`default `的值或者空值 。当变量未被定义，则将变量名插入，变量将被转换为占位符。

### 系统变量

| 变量             | 说明                           |
| ---------------- | ------------------------------ |
| TM_SELECTED_TEXT | 当前选定的文本或空字符串       |
| TM_CURRENT_LINE  | 当前行的内容                   |
| TM_CURRENT_WORD  | 光标所处单词或空字符串         |
| TM_LINE_INDEX    | 行号（从零开始）               |
| TM_LINE_NUMBER   | 行号（从一开始）               |
| TM_FILENAME      | 当前文档的文件名（含后缀名）   |
| TM_FILENAME_BASE | 当前文档的文件名（不含后缀名） |
| TM_DIRECTORY     | 当前文档所在目录               |
| TM_FILEPATH      | 当前文档的完整文件路径         |
| CLIPBOARD        | 当前剪贴板中内容               |

日期时间相关：

| 变量                     | 说明                       |
| ------------------------ | -------------------------- |
| CURRENT_YEAR             | 当前年份                   |
| CURRENT_YEAR_SHORT       | 当前年份的后两位           |
| CURRENT_MONTH            | 格式化为两位数字的当前月份 |
| CURRENT_MONTH_NAME       | 当前月份的全称             |
| CURRENT_MONTH_NAME_SHORT | 当前月份的简称             |
| CURRENT_DATE             | 当天月份第几天             |
| CURRENT_DAY_NAME         | 当天周几                   |
| CURRENT_DAY_NAME_SHORT   | 当天周几的简称             |
| CURRENT_HOUR             | 当前小时                   |
| CURRENT_MINUTE           | 当前分钟                   |
| CURRENT_SECOND           | 当前秒数                   |

注释相关：

| 变量                | 说明                                      |
| ------------------- | ----------------------------------------- |
| BLOCK_COMMENT_START | 块注释开始标识，如 PHP`/* `或 HTML `<!--` |
| BLOCK_COMMENT_END   | 块注释结束标识，如 PHP`*/ `或 HTML `-->`  |
| LINE_COMMENT        | 行注释，如： PHP `//` 或 HTML `<!-- -->`  |

若想将语法关键字当做普通字符使用则需要使用转义字符`\`（反斜杠）进行转义。