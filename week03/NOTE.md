# 每周总结可以写在这里

待修改

## 3-1 Expression（表达式）

### Grammar（语法）（还没听懂）

#### Left Handside（左手表达式）

**Member**、**New** 和 **Call** 被叫做 Left Handside（等号的左边）。

​	例：

​		**a.b** = c;

​		**a+b** = c;

##### Member

```
var o = {x: 1};
o.x + 2 // x是o的Member
```

- a.b

- a[b]

- foo\`string`

  - ```JavaScript
    var name = "hou";
    function foo() {
        console.log(arguments);
    }
    foo`Hi ${name}!`
    ```

- super.b

- super['b']

- new.target

- new Foo()

##### New

- new Foo

  例：

  ​	new a()()

  ​	new new a()

##### Call

比 New 的优先级更低

- foo()

- super()

- foo()['b']

- foo().b

- foo()\`abc`

#### Right Handside（右手表达式）

##### Update

- a ++

- a --

- -- a

- ++ a

  例：

  ​	++ a ++

  ​	++ (a ++)

### Runtime（运行时）
![脑图](https://raw.githubusercontent.com/houinchengdu/Frontend-01-Template/master/week03/Syntax（语法）.png)

并未完成作业，很糟糕，正在艰难并缓慢的第三周完成作业和补充前两周作业。
