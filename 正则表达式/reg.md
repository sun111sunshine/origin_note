# Reg

## test 返回 true 或者 false

### 单个字符

- 直接匹配某个字母 /a/
- 转译字符（`注：*是一个特殊字符，我们需要你通过反斜杠使其失效`）

| 特殊字符   | 正则表达式 | 记忆方式                                  |
| ---------- | ---------- | ----------------------------------------- |
| 换行符     | \n         | new line                                  |
| 换页符     | \f         | form feed                                 |
| 回车符     | \r         | return                                    |
| 空白符     | \s         | space                                     |
| 制表符     | \t         | tab                                       |
| 垂直制表符 | \v         | vertical tab                              |
| 回退符     | [\b]       | backspace,之所以使用[]符号是避免和\b 重复 |

### 多个字符

| 匹配区间                                      | 正则表达式 | 记忆方式            |
| --------------------------------------------- | ---------- | ------------------- |
| 除了换行符之外的任何字符                      | .          | 句号,除了句子结束符 |
| 单个数字, [0-9]                               | \d         | digit               |
| 除了[0-9]                                     | \D         | not digit           |
| 包括下划线在内的单个字符，[A-Za-z0-9_]        | \w         | word                |
| 非单字字符                                    | \W         | not word            |
| 匹配空白字符,包括空格、制表符、换页符和换行符 | \s         | space               |
| 匹配非空白字符                                | \S         | not space           |

### 循环与重复

| 匹配区间      | 正则表达式 |
| ------------- | ---------- |
| 0 个或者 1 个 | ?          |
| 大于等于 0 个 | \*         |
| 大于等于 1 个 | +          |

### 特定次数

```
{x}: x次

{min, max}： 介于min次到max次之间

{min, }: 至少min次

{0, max}： 至多max次

```

### 位置边界

- 单词边界（如果想严格匹配某个字符可以在两端加上\b,例如，严格匹配 cat，可以这样处理`/\bcat\b/`）

- 字符串边界 `/^cat$/`严格匹配某个字符

| 边界和标志 | 正则表达式 |
| ---------- | ---------- |
| 单词边界   | \b         |
| 非单词边界 | \B         |
| 字符串开头 | ^          |
| 字符串结尾 | \$         |
| 多行模式   | m 标志     |
| 忽略大小写 | i 标志     |
| 全局模式   | g 标志     |

### 子表达式

- 分组

- 回溯引用

所谓回溯引用（backreference）指的是模式的后面部分引用前面已经匹配到的子字符串。你可以把它想象成是变量，回溯引用的语法像\1,\2,....,其中\1 表示引用的第一个子表达式，\2 表示引用的第二个子表达式，以此类推。而\0 则表示整个表达式。

`回溯引用结合分组，通过\1,\2,\3这些调用之前的元素\0则表示调用整个表达式`

回溯引用在替换字符串

example: `abc abc 123`,将`abc`中的`c`替换为`x`

### 环视

环视只进行子表达式的匹配，不占有字符，匹配到的内容不保存到最终的匹配结果，是零宽度的。环视匹配的最终结果就是一个位置。

环视的作用相当于对所在位置加了一个附加条件，只有满足这个条件，环视子表达式才能匹配成功

环视按照方向划分有顺序和逆序两种，按照是否匹配有肯定和否定两种，组合起来就有四种环视。顺序环视相当于在当前位置右侧附加一个条件，而逆序环视相当于在当前位置左侧附加一个条件。

最好的例子是 千分位转换

#### 肯定和否定

- 肯定：(?=exp) 和 (?<=exp)
- 否定：(?!exp) 和 (?<!exp)

#### 顺序和逆序

- 顺序：(?=exp) 和 (?!exp)
- 逆序：(?<=exp) 和 (?<!exp)

#### 两种类型名称组合

- 肯定顺序：(?=exp)
- 否定顺序：(?!exp)
- 肯定逆序：(?<=exp)
- 否定逆序：(?<!exp)

[参考](https://www.cnblogs.com/Zjmainstay/p/regexp-lookaround.html)

### 练习题

- 正数或者小数 `/^-?\d+(\.\d+)?$/.test('-1111111.1111')`
- 匹配年月日日期 格式 2018-12-6 `/^[1-9]\d{3}-(1?[0-2]|[1-9])-([1-9]|[1-2]\d|3[0-1])$/.test('2018-10-12')`
- 匹配 qq 号 表示 5 位到 12 位 qq.第一位为非 0 `/^[1-9]\d{4,9}$/.test('1158810459')`
- 11 位的电话号码 `/^1(3|5|7|8)\d{9}$/.test('18700000000')`
- 长度为 8-10 位的用户密码 ： 包含数字字母下划线 `/^(\w|\.){8,10}$/.test('')`
- 匹配验证码：4 位数字字母组成的 `/^(\d|[A-Z]|[a-z]){4}$/.test('66sW')`
- 匹配邮箱地址 `/^\w{5,10}@(\d|[A-Z][a-z]){2,5}\.([A-Z]|[a-z]){2,3}$/.test('origin_wan@163.com')`
- 1-2*((60-30+(-40/5)*(9-2*5/3+7/3*99/4*2998+10*568/14))-(-4*3)/(16-3*2))
  1）从上面算式中匹配出最内层小括号以及小括号内的表达式
- 2019-5-20 => 2019/5/20 `'2018/2/10'.replace(/\//g,'.')`
- 去除链接中的指定字段

```js
function urlFilter(url, param) {
  const regStr = `&?${param}=(\\w|\\.)+[^&]?`;
  const reg = new RegExp(regStr);
  console.log("filter", url.replace(reg, ""));
  return url.replace(reg, "");
}

urlFilter(
  "http://is.snssdk.com/magic/runtime/?id=5705&ip=10.1.10.108&link_source=hhhh",
  "ip"
);
```

- 必须包含字母（不区分大小写）、数字，6-16 位密码`/^(?=.*?[A-Za-z])(.*?[0-9])[a-zA-Z0-9]{6,16}$/.test('123dadas')`

`(?=.*?[a-z])`限制必须有字母

`(?=.*?\d)`限制必须有数字

`(?![a-z\d]+$)`限制从开头到结尾不能全为数字和字母

`.+`在没有限定的情况下可以是任意字符

`^和$` 限定字符串的开头和结尾

- 千分为转换

1290`''`
