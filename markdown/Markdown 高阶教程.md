# Markdown 高阶教程

标签（空格分隔）： 教程

---

### 1. 显示文章结构目录
使用 `[TOC]` 显示内容目录
示例：

[TOC]

注：`[TOC]` 前后两行都要留空格

### 2. 添加标签
在编辑区任意行的行首可以使用如下代码添加标签

标签： 语文 数学 Markdown

或者

tags: 语文 数学 Markdown

### 3. 删除线
使用 ~~ 表示一段删除线
示例：
~~这是一段删除线~~

### 4. 注脚
使用 [^keyword] 添加一个注脚
示例：

这是一个注脚[^footnote]的样例。
这是一个注脚[^footnote2]的样例。

### 5. LeTex公式
\$ 表示行内公式
示例：
质能守恒定律可以用公式 $E=mc^2$ 表示

\$\$ 表示整行公式
示例：
$$\sum_{i=1}^n a_i=0$$
$$f(x_1,x_x,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2$$
$$\sum^{j-1}_{k=0}{\widehat{\gamma}_{kj} z_k}$$

访问 [MathJax](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference) 获取更多的LeTex公式

### 6. 代码高亮显示
非代码示例：
```
$ sudo apt-get install vim
```

python示例：
``` python
@display 
def display():
   print('Hello Python')
```

c++示例：
```c++
@zhugp
for(int i = 0; i != 10; ++i)
   cout << i << ' ';
cout << endl;
```

### 7. 流程图
####示例
```flow
st=>start: Start:>https://www.baidu.com
io=>inputoutput: verification
op=>operation: Your Operation
cond=>condition: Yes or No?
sub=>subroutine: Your Subroutine
e=>end

st->io->op->cond
cond(yes)->e
cond(no)->sub->io
```
更多 [流程图](http://adrai.github.io/flowchart.js/) 请参考

### 8. 序列图
####示例1
```seq
Alice->Bob: Bob,how you are?
Note Right of Bob: Bob thanks
Bob->Alice: I am good thanks!
```

####示例2
```seq
Title: Here is a title
A->B: Normal line
B-->C: Dashed line
C->>D: Open arrow
D-->>A: Dashed open arrow
```
更多 [序列图](http://bramp.github.io/js-sequence-diagrams/) 请参考

### 9. 甘特图
有关 [甘特图](https://baike.baidu.com/item/甘特图/113232?fr=aladdin) 的定义请点击
示例：
```gantt
   title 项目开发流程
   section 项目确定
       需求分析    :a1, 2016-06-22, 3d
       可行性报告  :after a1, 5d
       概念验证    : 5d
   section 项目实施
       概要设计    :2016-07-05, 5d
       详细设计    :2016-07-08, 10d
       编码        :2016-07-15, 10d
       测试        :2016-07-25, 5d
   section 发布验收
       发布        :2d
       验收        :3d
```
更多 [甘特图](https://knsv.github.io/mermaid/#gant-diagrams) 请参考

### 10. Mermaid 流程图

```graphLR
    A[Hard edge] -->|Link text| B(Round edge)
    B --> C{Decision}
    C -->|One| D[Result one]
    C -->|Two| E[Result two]
```

#### 更多语法参考：[Mermaid 流程图语法参考](https://knsv.github.io/mermaid/#flowcharts-basic-syntax)

### 11. Mermaid 序列图

```sequence
    Alice->John: Hello John, how are you?
    loop every minute
        John-->Alice: Great!
    end
```

#### 更多语法参考：[Mermaid 序列图语法参考](https://knsv.github.io/mermaid/#sequence-diagrams)

### 12. 表格
####示例
|项目 | 价格 | 数量|
|---- | ----: | :----:|
| 计算机 | \$1600| 6 |
| 手机 | \$12| 12 |
| 管线 | \$1| 234 |

### 13. 定义型列表
名词1
:    定义1（左侧有一个可见的冒号和四个不可见的空格）

代码块1
:    代码块定义（左侧有一个可见的冒号和四个不可见的空格）

        代码块（左侧有八个不可见的空格）

### 14. html标签
####示例：
    <table>
        <tr>
            <th>项目</th>
            <th>价格</th>
            <th>数量</th>
        </tr>
        <tr>
            <td>计算机</td>
            <td>\$1600</td>
            <td>6</td>
        </tr>
        <tr>
            <td>手机</td>
            <td>\$12</td>
            <td>12</td>
        </tr>
        <tr>
            <td>管线</td>
            <td>\$1</td>
            <td>234</td>
        </tr>
    </table>

<table>
    <tr>
        <th>项目</th>
        <th>价格</th>
        <th>数量</th>
    </tr>
    <tr>
        <td>计算机</td>
        <td>\$1600</td>
        <td>6</td>
    </tr>
    <tr>
        <td>手机</td>
        <td>\$12</td>
        <td>12</td>
    </tr>
    <tr>
        <td>管线</td>
        <td>\$1</td>
        <td>234</td>
    </tr>
</table>

### 15. 内嵌图标
    <i class="icon-weibo"></i>
微博图标：<i class="icon-weibo"></i>
人人图标：<i class="icon-renren"></i>
更多图标请参考 [font-awesame](http://fortawesome.github.io/Font-Awesome/3.2.1/icons/)

### 16. 待办事项
使用带有[ ]和[x]的列表语法书写一个代办事项，支持字表嵌套和混合使用Markdown语法
####示例：
- [x] **2017年总结**
- [x] 学习了算法的基本知识
- [x] 学习python
- [x] 确定要努力的方向：大数据

- [ ] **2018年计划**
- [ ] 换工作
- [ ] 继续学习大数据
1. 学习基础课程
2. 复习与大数据有关数据库
3. 进行一些有意义的项目
- [ ] 继续学习其他有关的新技术


注：以下注脚说明 每一行都与其他行由空格分割，中括号和冒号无空格
[^footnote]: 这是第一个注脚的例子

[^footnote2]: 这是第二个注脚的例子