# Node类型

Node接口定义了整个DOM原始数据类型，该接口将由DOM中的所有节点类型实现。Node接口所描述的类型在文档树中表示一个单一的节点。所有实现Node接口的对象都提供了一些方法来操作子节点，但是并不是所有对象都有子节点，例如：Text节点就没有子节点。

每个节点都有一个nodeType属性，用于表示节点的类型。在DOM中任何节点都将是这12种类型之一

```js
ELEMENT_NODE                   = 1;     // Element类型
ATTRIBUTE_NODE                 = 2;     // Attr类型
TEXT_NODE                      = 3;     // Text类型
CDATA_SECTION_NODE             = 4;     // CDATASection类型
ENTITY_REFERENCE_NODE          = 5;     // EntityReference类型
ENTITY_NODE                    = 6;     // Entity类型
PROCESSING_INSTRUCTION_NODE    = 7;     // ProcessingInstruction类型
COMMENT_NODE                   = 8;     // Comment类型
DOCUMENT_NODE                  = 9;     // Document类型
DOCUMENT_TYPE_NODE             = 10;    // DocumentType类型
DOCUMENT_FRAGMENT_NODE         = 11;    // DocumentFragment类型
NOTATION_NODE                  = 12;    // Notation类型
```

### nodeName, nodeValue和attributes属性

对于节点信息，可以使用nodeName, nodeValue及attribues来获取，根据不同的节点类型其返回值不尽相同，具体如下表：

| Interface              | nodeName                             | nodeValue                                                    | attributes     |
| ---------------------- | -------------------------------------| -------------------------------------------------------------| -------------- |
| Attr                   | same as `Attr.name`                  | same as `Attr.value`                                         | null           |
| CDATASection           | "#cdata-section"                     | same as CharacterData.data, the content of the CDATA Section | null           |
| Comment                | "#comment"                           | same as CharacterData.data, the content of the comment       | null           |
| Document               | "#document"                          | null                                                         | null           |
| DocumentFragment       | "#document-fragment"                 | null                                                         | null           |
| DocumentType           | same as DocumentType.name            | null                                                         | null           |
| Element                | same as Element.tagName              | null                                                         | NamedNodeMap   |
| Entity                 | entity name                          | null                                                         | null           |
| EntityReference        | name of entity referenced            | null                                                         | null           |
| Notation               | notation name                        | null                                                         | null           |
| ProcessingInstruction  | same as ProcessingInstruction.target | same as ProcessingInstruction.data                           | null           |
| Text                   | "#text"                              | same as CharacterData.data, the content of the text node     | null           |

注意在上面表格所列出的12种类型中根据最新的DOM标准(DOM4)`ATTRIBUTE_NODE`, `CDATA_SECTION_NODE`, `ENTITY_NODE `, `ENTITY_REFERENCE_NODE`和`NOTATION_NODE`已成为历史（或已废弃），不再推荐使用。而其它类型将在其它文章中一一介绍。

对于属性attributes只有当节点类型为Element时，返回值才为NamedNodeMap，其它节点类型均反回`null`值。换句话说，Element类型是使用attributes属性的唯一一个DOM节点类型。

### 节点关系

每一个节点都有一个`childNodes`属性，其中保存着一个`NodeList`对象。NodeList是一个类数组对象，用于保存一组有序的节点，可以通过位置来访问这些节点，并且它能够根据DOM结构变化来实时更新自己的数据。NodeList对象拥有一个`length`属性用于表示访问NodeList的那一刻，其中包含的节点数量，还有一个`item()`方法用于访问给定索引位置处的节点。

```js
// 示例html结构

//<div id="parent">
//  <div>item 1</div>
//  <div>item 2</div>
//  <div>item 3</div>
//  <div>item 4</div>
//</div>

var parent = document.getElementById("parent");

parent.childNodes.length; // 9
// 通过方框号访问节点
parent.childNodes[1]; // <div>item 1</div>
// 通过item()方法
parent.childNodes.item(1); // <div>item 1</div>
// 添加一个新节点
var newNode = document.createElement("div");
var text = document.createTextNode("item 5");
newNode.appendChild(text);
parent.appendChild(newNode);
// 更新后的节点数量
parent.childNodes.length; // 10
// 获取最后一个节点
parent.childNodes[parent.childNodes.length - 1]; // <div>item 1</div>
```

上例中，示例html结构中，总共只有4个子div，为什么`parent.childNodes.length`会得到9这个结果呢？原因在于NodeList对象中包含了`TEXT_NODE`节点，它实际上将两个div之间的回车都算上了，而在IE8及以下得到的结果是4，并不包含文本节点。

由于NodeList是一个类数组对象，很多时候我们想对它进行更多的操作，因此需要转换成真正的数组。

```js
// 在IE8及之前版本无效
var arrayOfNodes = Array.prototype.slice.call(someNode.childNodes, 0);
```

### 节点属性：
 * parentNode
 * ownerDocument
 * firstChild
 * lastChild
 * previousSibling
 * nextSibling
 * localName
 * baseURI --> window.location.href
 * namespaceURI
 * textContent

### 方法
 * hasChildNodes()
 * hasAttributes()
 * compareDocumentPosition(node);
 * appendChild() --> new Node
 * insertBefore(newNode, refNode) --> new Node
 * replaceChild(newNode, oldNode) --> replaced Node
 * removeChild(oldNode) --> delete Node
 * cloneNode(bool) --> new Node

### parentNode接口

属性：
 * children
 * firstElementChild
 * lastElementChild
 * childElementCount

方法:
 * query(relativeSelector) --> Element
 * queryAll(relativeSelectors) --> Elements
 * querySelector(selectors)
 * querySelectorAll()

### NonDocumentTypeChildNode接口

属性：
 * previousElementSibling
 * nextElementSibling

### ChildNode接口

方法：
 * remove()

### Document接口

属性：
 * implementation --> DOMImplementation
 * URL
 * domain   // 页面的域名
 * referrer   // 保存着链接到当前页面的那个页面的URL
 * origin
 * compactMode
 * characterSet
 * contentType
 * doctype --> DocumentType
 * documentElement --> Element
 * body
 * title

方法：
 * getElementById(localName)
 * getElementsByTagName(localName) --> HTMLCollection
 * getElementsByTagNameNS(namespace, localName) --> HTMLCollection
 * getElementsByClassName(className) --> HTMLCollection

 * createElement(localName) --> Element
 * createElementNS(namespace, qualifiedName) --> Element
 * createDocumentFragment() --> DocumentFragment
 * createTextNode(data) --> Text
 * create



### 网络参考

 * [W3C DOM](http://www.w3.org/TR/dom/)
 * [What's wrong with extending the DOM](http://perfectionkills.com/whats-wrong-with-extending-the-dom/)
 * [跟随 Web 标准探究DOM -- Node 与 Element 的遍历](http://www.cnblogs.com/joyeecheung/p/4168521.html)



