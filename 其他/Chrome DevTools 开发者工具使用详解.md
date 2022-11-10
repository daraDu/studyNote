# Chrome DevTools 开发者工具使用详解

[TOC]

## 一、八大功能模块面板详解

`当前版本：版本： 93.0.4577.63（正式版本） （64 位）`

> DevTools是很多功能的集合，而在窗口顶部的工具栏是对这些功能的分组，当前版本主要有八个功能组，分别对应八个面板：
>
> - **Elements**：在Elements面板中可以通过DOM树的形式查看所有页面元素，同时也能对这些页面元素进行所见即所得的编辑
> - **Console**：一方面用来记录页面在执行过程中的信息（一般是通过各种console语句来输出），另一方面用来当做shell窗口
> - **Network**：在Network面板中可以查看通过网络请求的资源的详细信息以及请求这些资源的耗时
> - **Sources**： Sources面板主要用来调试页面中的JavaScript
> - **Performance**： Performance面板
> - **Memory**： Memory面板
> - **Application**： Application面板
> - **Lighthouse**： Lighthouse面板

### 1、Elements面板

> Elements面板主要用于对页面HTML和CSS的检查以及可视化编辑

![1](./image/1.png)

> 可以看到整个面板被拆分成3个部分，左上为主体dom树，左下角为选中的元素节点，右边为选中节点的样式

#### ①、DOM树

- ##### 1）、检查页面元素

  > - 右击页面任意一处，选择检查/审查元素，查看选中区域对应的DOM元素
  >
  > - 点击开发者工具左上角小箭头图标，当鼠标显示为蓝色时
  >
  >   > 鼠标悬停 DOM 树上的任意一个节点，页面会用淡蓝色的蒙板在页面上标记 DOM 节点对应的页面
  >   >
  >   > 鼠标点击页面任意一处，可以查看选中内容对应的DOM元素
  >
  > - 按键盘的向上向下键可以在展开的节点之间进行切换，向左向右键可以收缩和展开节点

- ##### 2）、编辑DOM

  >你可以任意修改DOM树上的任意信息，比如修改节点的类型、属性，或者改变DOM节点的所属关系等等。
  >
  >不过需要注意的是，这些修改是临时的，不会保存，页面一刷新所有修改都将重置

  - 常见的操作

    > - 双击元素标签，修改DOM节点类型，比如将p改成div
    > - 双击元素属性，修改DOM节点属性，比如修改节点的id，class
    > - 选择一个DOM节点，按enter键，然后按tab键选择想修改的属性或标签
    > - 选择一个DOM节点，并将其拖到目标位置，可以改变页面元素的结构
    > - 选择一个DOM节点，按delete键删除
    > - Ctrl + Z / Cmd + Z，撤回操作

  - 选中后右键更多操作

    > - `Add Attribute`：为选中节点添加一个属性
    > - `Edit Attribute`：修改选中节点中选中属性
    > - `Edit as HTML`：将选中节点当做 HTML 进行编辑
    > - `Delete element`：删除节点
    > - `Copy→`:复制选中的节点，可以复制选中节点的选择器、XPath、元素本身、outerHTML 等，也能剪切、粘贴节点，我们一般选择复制节点的选择器
    > - `Hide element`：隐藏节点
    > - `Force state→`:4个伪类-选中则表示所选节点处于相应的状态，比如选中 `:hover` 则表示所选节点当前正处于鼠标悬停的状态
    > - `Expand all`：展开所选节点下的所有子节点
    > - `Collapse all`：收缩所选节点下的所有子节点，包括自己
    > - `Scroll into View`：如果所选的 DOM 节点对应的页面元素不在可视区域内的话，点击这个选项则会将页面滚动到可以显示对应的页面元素的位置

#### ②、elements的内置面板

![2](./image/2.png)

>- ##### Style：实时编辑与所选元素相关的样式（常用）
>
>- ##### Computed：检查编辑所选元素的盒模型（常用）
>
>- Layout:主要用来看页面网格参数的（布局）
>
>- Event Listeners：查看所选元素相关的 JS 事件监听
>
>- DOM Breakpoints: DOM 断点
>
>- Properties：所选节点对应的对象及父类们
>
>- Accessibility:你可以查看整个页面中的Accessibility Tree，该 Tree 是DOM tree 的子集，对应节点是由对屏幕阅读器（screen reader）有关和有用的展示内容。

1）Styles面板

> Styles 面板可以允许你通过各种方式来修改元素的样式，并且会想方设法使得你调试时简单方便。

![2](./image/3.png)

①**element.style：**

> 代表所选元素的内联样式。内联样式高于.header里的样式

