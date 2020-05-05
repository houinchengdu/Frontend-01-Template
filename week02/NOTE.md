## 02-1 编程语言通识与JavaScript语言设计

### 语言按语法分类

#### 非形式语言

中文，英文...

特点：非严格定义

#### 形式语言([乔姆斯基谱系](https://zh.wikipedia.org/wiki/乔姆斯基谱系))

0-型 无限制文法

​	`?::=?`

1-型 上下文相关语言

​	`?<A>?::=?<B>?`

2-型 上下文无关语言

​	`<A>::=?`

​	等号左边只能有一个非终结符

3-型 正则语言

​	`<A>::=<A>?`

​	`<A>::=?<A>`❌

​	只允许左递归

### 产生式（[巴科斯范式](https://zh.wikipedia.org/wiki/巴科斯范式))

<符号>::=<使用符号的表达式>

用尖括号（< >）括起来的名称来表示语法结构名

语法结构：

- 基础结构：终结符
  - 如：`*` ，不是由任何其它的语法结构构成
- 复合结构：非终结符（复合结构需要用其它语法结构定义）
  - 如：乘法表达式，由 `数字` + `*` + `数字` 三个东西复合在一起

引号和中间的字符表示终结符

- 可以有括号
- `*` 表示重复多次
- `|` 表示或
- `+` 表示至少一次

### 图灵完备性



## 02-2 

### WhiteSpace（空格）

#### [\<TAB>](https://www.fileformat.info/info/unicode/char/0009/index.htm)

CHARACTER TABULATION（制表符）

Tab键 空格 "	"

U+0009

#### [\<VT>](https://www.fileformat.info/info/unicode/char/0011/index.htm)

纵向制表符

U+0011

#### [\<FF>](https://www.fileformat.info/info/unicode/char/000c/index.htm)

FORM FEED（进纸）

U+000C

#### [\<SP>](https://www.fileformat.info/info/unicode/char/0020/index.htm) ✅

BACKSPACE

U+0020

**推荐使用，其它空格一律\u转义**

#### [\<NBSP>](http://www.fileformat.info/info/unicode/char/00a0/index.htm)

NO-BREAK SPACE

U+00A0

两个词中间加 `&nbsp;` 不会断词

例：`The&nbsp;Long and Winding Road.`

#### [\<ZWNBSP>](http://www.fileformat.info/info/unicode/char/FEFF/index.htm)

ZERO WIDTH NO-BREAK SPACE（零宽空格）

U+FEFF

JS的HTML代码前面留空行能防止某些服务端的bug导致第一个字符永远按[BOM（Byte order mark）](https://en.wikipedia.org/wiki/Byte_order_mark)解析（吞第一个字符），UTF-8的网页代码不应使用BOM

### LineTerminator（换行符）

#### [\<LF>](https://www.fileformat.info/info/unicode/char/000a/index.htm) ✅

`\n`

LINE FEED（换行）

U+000A

**推荐使用**

#### [\<CR>](https://www.fileformat.info/info/unicode/char/000d/index.htm)

`\r`

CARRIAGE RETURN（回车）

U+000D

### Comment（注释）

- 不能以 `/u…` 的方式代替 `*` 和 `/`

#### 单行注释

```
// 单行注释
```

#### 多行注释

- 不能嵌套

```JavaScript
/*
多行
注释
*/
```

### Token（JS中一切有效的输入元素）

#### Punctuator（符号）

```JavaScript
{ } ( ) [ ] . ... ; , < > <= >= == != === !== + - * % ** ++ -- << >> >>> & | ^ ! ~&& || ? : = += -= *= %= **= <<= >>= >>>= &= |= ^= / /=
```

#### IdentifierName（标识符名称）

##### Keywords（关键字）

```
await break case catch class const continu debugge default delete do else export extends finally for functio if import in instanc new return super switch this throw try typeof var void while with yield
```

##### Identifier（标识符）

###### 变量名

不能用 Keywords（关键字）

getter 里的 get 是个例外

###### 属性名

可以用 Keywords（关键字）

##### Future reserved Keywords

enum

#### Literal（直接量）

##### Number

###### 进制写法

- **DecimalLiteral（十进制）**
  - **第一种：**`十进制整数` + `.` + `0个或多个十进制数字` + `0个或1个指数部分`
    - **第二种：**`.` + `1个或多个十进制数字` + `0个或1个指数部分`
    - **第三种：**`十进制整数` + `0个或1个指数部分`
      - 十进制整数：
        - `0` 
        - `非0数字` + `0个或多个十进制数字`
          - 非0数字：**one of** `1 2 3 4 5 6 7 8 9`
      - 十进制数字：**one of** `0 1 2 3 4 5 6 7 8 9`
      - 指数部分：`e或E` + `有符号的整数`
        - 有符号的整数：
          - `十进制数字`
          - `+` + `十进制数字`
          - `-` + `十进制数字`
- **BinaryIntegerLiteral（二进制）**
  - `0b或0B` + `1个或多个二进制数字`
    - 二进制数字：**one of** `0 1`
- **OctalIntegerLiteral（八进制）**
  - `0o或0O` + `1个或多个位八进制数字`
      - 八进制数字：**one of** `0 1 2 3 4 5 6 7`
- **HexIntegerLiteral（十六进制）**
  - `0x或0X` + `1个或多个十六进制数字`
    - 十六进制数字：**one of** `0 1 2 3 4 5 6 7 8 9 a b c d e f A B C D E F`

---

##### Number实践

- **0.1 + 0.2 是否等于 0.3 的正确写法**

```
Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON
```

##### String

###### Character（字符）

###### Code Point（码点）

###### Encoding（编码）

- **ASCII** ✅

  写代码时应把内容限制在ASCII（不含扩展）范围内，若不想受限制应限制在BMP范围内（U+0000 - U+FFFF）。

- Unicode

  - UTF-8

  - UTF-16

    JavaScript基本上是以UTF-16实际在内存中存储，不承认BMP之外的字符是一个字符。

- UCS
  
  > JavaScript的String的 `fromCharCode` API只能处理Unicode的BMP范围内（U+0000 - U+FFFF）的字符，超出的需要用 `String.fromCodePoint` 和 `"".codePointAt` 。建议常用 `"".codePointAt` 。
  
  Unicode的BMP范围（U+0000 - U+FFFF），Unicode子集，JavaScript的字符串支持到BMP范围。
  
  - U+0000 至 U+FFFF
  
- GB（国标）
  - GB2312
  - GBK(GB13000)
  - GB18030
  
- ISO-8859（欧洲）
  
- BIG5（台湾）

###### Grammar（语法）

- 单引号
  - `''`

- 双引号
  - `""`（待补充）
    - 支持任何 `非""` 或 `\（反斜杠）` 的字母
    - `\`后面可以接转义

- 反引号

  - ` `` `

  解析过程

  ```JavaScript
  `I said: "${s1}", "${s2}`
  ```

  ```JavaScript
  `I said: "${  // 以 ` 开头，以 ${ 结尾
      s1        // JS代码
  }", "${       // 以 } 开头，以 ${ 结尾
      s2        // JS代码
  }" `          // 以 } 开头，以 ` 结尾
  ```

---

##### String实践

**查看一个数的二进制**

`97.toString(2); // 报错` ❌

- `97 .toString(2); // "1100001"` ✅
- `97..toString(2); // "1100001"` ✅
- `(97).toString(2); // "1100001"` ✅

`97.` 是一个合法的Number，词法上的优先原则会把 `.` 和 `97` 粘在一起。

##### Boolean

- `true`

- `false`

##### Object

##### Null

- `null`

##### Undefined

- `undefined`

##### Symbol

---

#### 正则表达式匹配直接量

✅

```JavaScript
/a/g
```

❌

```JavaScript
var a;
a      // 若换行就不是正则表达式，而是除法，a除了a，又除了g
/a/g
```

