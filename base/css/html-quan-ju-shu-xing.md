# HTML 全局属性

全局属性是所有 HTML 元素共有的属性; 它们可以用于所有元素，即使属性可能对某些元素不起作用。

我们可以在所有的HTML元素上指定全局属性，甚至是在标准里没有指定的元素。这意味着任何非标准元素仍必须能够应用这些属性，即使使用这些元素意味着文档不再是html5兼容的。

| 属性 | 描述 |
| :--- | :--- |
| `id` |  定义唯一标识符（ID），该标识符在整个文档中必须是唯一的。|
| `class` | 一个以空格分隔的元素的类名（classes ）列表 |
| `data-\*` | 一类自定义数据属性，它赋予我们在所有 HTML 元素上嵌入自定义数据属性的能力 |
| `style` | 含要应用于元素的CSS样式声明。 |
| `title` | 包含表示与其所属元素相关信息的文本。 |
| `tabindex` | 整数属性，指示元素是否可以获取输入焦点（可聚焦），是否应该参与顺序键盘导航，如果是，则表示哪个位置。 |
| `is` | 允许您指定标准HTML元素应该像已注册的自定义内置元素一样 |
| `dir` | 一个指示元素中文本方向的枚举属性。 |
| `lang` | 帮助定义元素的语言:不可编辑元素所在的语言，或者应该由用户编写的可编辑元素的语言。 |
| `slot` | 将shadow DOM阴影关联树中的一个沟槽分配给一个元素：具有slot属性的元素被分配给由 `<slot>` 元素创建的沟槽，其name属性的值与slot属性的值匹配。 |
| `hidden` | 布尔属性表示该元素尚未或不再相关 |
| `draggable` | 一种枚举属性，指示是否可以 使用 Drag and Drop API 拖动元素。 |
| `accesskey` | 提供了为当前元素生成键盘快捷键的提示。这个属性由空格分隔的字符列表组成。浏览器应该使用在计算机键盘布局上存在的第一个。 |
| `autocapitalize` | 控制用户的文本输入是否和如何自动大写 |
| `contenteditable`| 一个枚举属性（enumerated attribute），表示元素是否可被用户编辑。 |
| `inputmode` | 向浏览器提供有关在编辑此元素或其内容时要使用的虚拟键盘配置类型的提示。 |
| `itemid` | 项的唯一全局标识符。 |
| `itemprop` | 用于向项添加属性。 |
| `itemref` | 只有不是具有 `itemscope` 属性的元素的后代，它的属性才可以与使用 `itemref` 项目相关联。 |
| `itemscope` | `itemscope`（通常）与 `itemtype` 一起使用，以指定包含在关于特定项目代码块中的HTML。 |
| `itemtype` | 指定将用于在数据结构中定义 `itemprops`（项属性）的词汇表的URL。 |
| `part` | 元素的部件名称的空格分隔列表。 |
| `dropzone` | 枚举属性，指示可以使用 Drag and Drop API 在元素上删除哪些类型的内容。 |
| `spellcheck` | 枚举属性定义是否可以检查元素是否存在拼写错误。 |
| `translate`  | 枚举属性，用于指定在页面本地化时是否转换元素的属性值及其 Text 节点子节点的值，或者是否保持它们不变。 |
| `contextmenu`(废弃) |  |

除了基本的HTML全局属性之外，还存在以下全局属性:

* `xml:lang` 和 `xml:base` ——两者都是从XHTML规范继承，但为了兼容性而被保留的。
* 多重 `aria-*` 属性，用于改善可访问性。
* 事件处理程序属性：

  onabort, onautocomplete, onautocompleteerror, onblur, oncancel, oncanplay, oncanplaythrough, onchange, onclick, onclose, oncontextmenu, oncuechange, ondblclick, ondrag, ondragend, ondragenter, ondragexit, ondragleave, ondragover, ondragstart, ondrop, ondurationchange, onemptied, onended, onerror, onfocus, oninput, oninvalid, onkeydown, onkeypress, onkeyup, onload, onloadeddata, onloadedmetadata, onloadstart, onmousedown, onmouseenter, onmouseleave, onmousemove, onmouseout, onmouseover, onmouseup, onmousewheel, onpause, onplay, onplaying, onprogress, onratechange, onreset, onresize, onscroll, onseeked, onseeking, onselect, onshow, onsort, onstalled, onsubmit, onsuspend, ontimeupdate, ontoggle, onvolumechange, onwaiting



