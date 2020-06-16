# ———————————XML文件—————————— #



<p id="t"></p>

## :book:目录 ##

:arrow_double_down:<a href="#a1">XML概述 </a>

:arrow_double_down:<a href="#a2">XML解析 </a>

:arrow_double_down:<a href="#a3">dom4j </a>

<p id="a1"><p>
  
### :custard:XML概述  ###

:arrow_double_up:<a href="#t">返回目录</a>


我们知道属性文件（property file）来描述程序配置。属性文件包含了一组名/值对，例如：

```
fontname-Times Roman fontsize=12
windowsize=400200
Color=050100
```

你可以在Properties类中仅使用一个方法调用即可读入这样的属性文件。这是一个很好的特性，但这还不够。在许多情况下，想要描述的信息的结构比较复杂，属性文件不能很方便地处理它。例如，对于下面例子中的fontname/fontsize项，使用以下的单一项将更符合面向对象的要求：

`font=Times Roman 12`

但是，这时对字体描述的分析就变得很讨厌了，必须确定字体名在何处结束，字体大小在何处开始。

属性文件采用的是一种单一的平面层次结构。你常常会看到用如下的键名解决这种局限性：

```
title.fontmane=Helvetica 
title.fontsize=36
body.fontname=Times
body.fontsize=12
```

属性文件格式的另一个缺点是要求键是惟一的。如果要存放一个值序列，则需要另一个变通方法，例如：

```
menu.item.1=Times Roman
menu.item.2=Helvetica 
menu.item.3-Goudy 01d Style 
```

XML格式解决了这些问题，因为它能够表示层次结构，这比属性文件的平面表结构更灵活。


```xml
<configuration>
  <title>
    <font>
      <name>Helvetica</name>
      <size>36</size>
    </font>
  </title>
  <body>
    <font>
      <name>Times Roman</name>
      <size>12</size>
    </font>
  </body>
  <window>
    <width>400</width>
    <height>200</height>
  </window>
  <color>
    <red>0</red>
    <green>50</green>
    <blue>100</blue>
  </color>
  <menu>
    <item>Times Roman</item>
    <item>Helvetica</item>
    <item>Goudy 01d Style</item>
  </menu>
</configuration>
```

XML格式能够表达层次结构，并且重复的元素不会被曲解。
正如上面看到的，XML文件的格式非常直观，它与HTML文件非常相似。这完全是有理由的，因为XML和HTML格式是古老的标准通用标记语言（Standard Generalized Markup Language，SGML）的衍生语言。

**XML文档的结构**

XML文档应当以一个文档头开始，例如：`<？xml version="1.0"？>`或者`<?xml version="1.0"encoding="UTF-8"?>`
严格来说，文档头是可选的，但是强烈推荐你使用文档头。



最后，XML文档的正文包含根元素，根元素包含其他元素。例如：

```xml
<?xml version="1.0"?>
<!DOCTYPE configuration...>
  <configuration>
    <title>
      <font>
        <name>Helvetica</name>
        <size>36</size>
      </font>
    </title>
  </configuration>
 ```
 
 元素可以有子元素（child element）、文本或两者皆有。在上述例子中，font元素有两个子元素，它们是name和size。name元素包含文本“Helvetica”。
 
 XML元素可以包含属性，例如：
 
```xml
<size unit="pt">36</size>
```

何时用元素，何时用属性，在XML设计中存在一些分歧。例如，将font做如下描述：

```xml
<font name="Helvetica"size="36"/>
```

似乎比下面更简单一些：

```xml
<font>
  <name>Helvetica</name>
  <size>36</size>
</font>
```

但是，属性的灵活性要差很多。假设你想把单位添加到size的值中去，如果使用属性，那么必须把单位添加到属性值中去：

```xml
<font nanme="Helvetica"size="36pt"/>
```

现在必须对字符串“36pt”进行解析，而这正是XML设计用来避免的那种麻烦。
把属性加到size元素中则简单多了：

```xml
<font>
  <name>Helvetica</name>
  <size unit="pt">36x/size>
</font>
```

一个常用的经验法则是，属性只应该用来修改值的解释，而不是用来指定值。如果你发现自己陷入了争论，在纠结于某个设置是否是对某个值的解释所作的修改，那么你就应该对属性说“不”，转而使用元素，许多有用的文档根本就不使用属性。

<p id="a2"><p>
  
### :custard:XML解析  ###

:arrow_double_up:<a href="#t">返回目录</a>

要处理XML文档，就要先解析（parse）它。解析器是这样一个程序：它读入一个文件，确认这个文件具有正确的格式，然后将其分解成各种元素，使得程序员能够访问这些元素。

Java库提供了两种XML解析器：

* 像文档对象模型（Document Object Model，DOM）解析器这样的树型解析器（tree parser），它们将读入的XML文档转换成树结构。
* 像XML简单API（SimpleAPI for XML，SAX）解析器这样的流机制解析器（streaming parser），它们在读入XML文档时生成相应的事件。

DOM解析器对于实现我们的大多数目的来说都更容易一些，所以我们首先介绍它。如果你要处理很长的文档，用它生成树结构将会消耗大量内存，或者如果你只是对于某些元素感兴趣，而不关心它们的上下文，那么在这些情况下你应该考虑使用流机制解析器。

DOM解析器的接口已经被W3C标准化了。org.w3c.dom包包含了这些接口类型的定义，比如：Document和Element等。不同的提供者，比如Apache Organization和IBM，都编写了实现这些接口的DOM解析器。Java XML处理API（Java API for XML Processing，JAXP）库使得实际上可以以插件形式使用这些解析器中的任意一个。但是JDK中也包含了自己的DOM解析器。在本章中，我们使用的就是这个解析器。

要读入一个XML文档，首先需要一个DocumentBuilder对象，可以从DocumentBuilder Factory中得到这个对象，例如：

```java
public static void main(String[] args) {
		DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
		try {
			DocumentBuilder builder = factory.newDocumentBuilder();
			Document doc = builder.parse("config.xml");
		} catch (ParserConfigurationException e) {
			e.printStackTrace();
		} catch (SAXException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
```


对于读入文件可以是多种类型：

```java
Document doc =builder.parse(File);
Document doc=builder.parse(URL);
Document doc=builder.parse(InputStrean);
```

Document对象是XML文档的树型结构在内存中的表现，它由实现了Node接口及其各种子接口的类的对象构成


可以通过调用getDocumentE1ement方法来启动对文档内容的分析，它将返回根元素。

```java
	Element e = doc.getDocumentElement();
```

如示例xml文件config.xml:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<root author="lumnca">
    <name>Lumnca</name>
    <age>47</age>
    <tools>
        <tool>
            <name version="1.2">Date</name>
        </tool>
        <tool>
            <name version="1.3">Math</name>
        </tool>
    </tools>
</root>
```

我们来试着解析：


**获取根节点**

```
public static void main(String[] args) {
		DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
		try {
			DocumentBuilder builder = factory.newDocumentBuilder();
			Document doc = builder.parse("config.xml");

			Element e = doc.getDocumentElement();

			System.out.println(e.getTagName());            //标签名    root
			System.out.println(e.getPrefix());             //父级元素  null
			System.out.println(e.getAttribute("author"));  //获取属性  author

		} catch (ParserConfigurationException e) {
			e.printStackTrace();
		} catch (SAXException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
```


如果要获取子元素就需要遍历：

```java
			DocumentBuilder builder = factory.newDocumentBuilder();
			Document doc = builder.parse("config.xml");

			NodeList nodeList = doc.getChildNodes();

			for (int i = 0; i <nodeList.getLength() ; i++) {
				Node n = nodeList.item(i);
				System.out.println(n.getTextContent());
				System.out.println(n.getPrefix());
			}

```

你会发现输出有很多空格，这是由于与html解析dom一样，父标签与子标签之间有一个空白节点。

可以如下过滤：

```java
for (int i = 0; i <nodeList.getLength() ; i++) {
				Node n = nodeList.item(i);
				if(n instanceof Element){
					Element e = (Element)n;
					System.out.println(e.getTagName());
 }
```

  
正如将在下一节中所看到的那样，如果你的文档有DTD，那么你就可以做得更好。这时，解析器知道哪些元素没有文本节点的子元素，而且它会帮你剔除空白字符。
在分析name和age元素时，你肯定想获取它们包含的文本字符串。这些文本字符串本身都包含在Text类型的子节点中。既然知道了这些Text节点是惟一的子元素，就可以用getFirst-Child方法而不用再遍历一个NodeList。然后可以用getData方法获取存储在Text节点中的字符串。


```java
for (int i = 0; i <nodeList.getLength() ; i++) {
				Node n = nodeList.item(i);
				if(n instanceof Element){
					Element e = (Element)n;
					Text text =(Text) e.getFirstChild();
					System.out.println(text.getData().trim());
				}
			}
```


当然xml也可以通过具体的名称来获取dom节点：

```java
      DocumentBuilder builder = factory.newDocumentBuilder();
			Document doc = builder.parse("config.xml");

			NodeList nodeList = doc.getElementsByTagName("name");
			System.out.println(nodeList.getLength());

			for (int i = 0; i < nodeList.getLength(); i++) {
				Node n = nodeList.item(i);
				if(n instanceof  Element){
					System.out.println(n.getTextContent());
				}
			}
```

如上就是获取name标签的信息，由于name标签不止一个所以才需要数组遍历。当然还有很多解析方法和工具不过下面要介绍dom4j解析工具


<p id="a3"><p>
  
### :custard:Dom4j概述  ###

:arrow_double_up:<a href="#t">返回目录</a>

DOM4J是 dom4j.org 出品的一个开源 XML 解析包。DOM4J应用于 Java 平台，采用了 Java 集合框架并完全支持 DOM，SAX 和 JAXP。

 * DOM4J 使用起来非常简单。只要你了解基本的 XML-DOM 模型，就能使用。
 
 * Dom：把整个文档作为一个对象。
 
DOM4J 最大的特色是使用大量的接口。它的主要接口都在org.dom4j里面定义

读写XML文档主要依赖于org.dom4j.io包，[下载地址](https://mvnrepository.com/artifact/dom4j/dom4j/1.6.1)有DOMReader和SAXReader两种方式。因为利用了相同的接口，它们的调用方式是一样的。

```java
  public static void main(String[] args) {
		SAXReader saxReader = new SAXReader();
		try {
			Document document = saxReader.read(new File("config.xml")); // 读取XML文件,获得document对象
		} catch (DocumentException e) {
			e.printStackTrace();
		}

	}
```

获取根节点：

```java
      Document document = saxReader.read(new File("config.xml")); // 读取XML文件,获得document对象
			Element e = document.getRootElement();
			System.out.println(e.getName());
 ```
 
 新增一个节点以及其下的子节点与数据:
 
 ```java
 	public static void main(String[] args) {
		SAXReader saxReader = new SAXReader();
		try {
			Document document = saxReader.read(new File("config.xml")); // 读取XML文件,获得document对象
			Element e = document.getRootElement();

			Element adde = e.addElement("sex");
			adde.setText("男");

			System.out.println(e.elementText("sex")); //获取根目录下的sex标签文本

		} catch (DocumentException e) {
			e.printStackTrace();
		}
	}
 ```
 
 注意节点不会被加入文件，只是临时可用。
 
 写入一个xml文件
 
 ```java
 public static boolean doc2XmlFile(Document document, String filename) {  
    boolean flag = true;  
    try {  
        XMLWriter writer = new XMLWriter(new OutputStreamWriter(  
                new FileOutputStream(filename), "UTF-8"));  
        writer.write(document);  
        writer.close();  
    } catch (Exception ex) {  
        flag = false;  
        ex.printStackTrace();  
    }  
    System.out.println(flag);  
    return flag;  
}
```

Element对象的操作方法
|方法|描述|
|:--:|:--|
|getQName() |元素的QName对象|
|getNamespace()|元素所属的Namespace对象|
|getNamespacePrefix()|元素所属的Namespace对象的prefix|
|getNamespaceURI()|元素所属的Namespace对象的URI|
|getName()|元素的local name|
|getQualifiedName()|元素的qualified name|
|getText()|元素所含有的text内容，如果内容为空则返回一个空字符串而不是null|
|getTextTrim|元素所含有的text内容，其中连续的空格被转化为单个空格，该方法不会返回null|
|attributeIterator()|元素属性的iterator，其中每个元素都是Attribute对象|
|attributeValue()|元素的某个指定属性所含的值|
|elementIterator()|元素的子元素的iterator，其中每个元素都是Element对象|
|element()|元素的某个指定（qualified name或者local name）的子元素|
|elementText()|元素的某个指定（qualified name或者local name）的子元素中的text信息|
|getParent|元素的父元素|
|getPath()|元素的XPath表达式，其中父元素的qualified name和子元素的qualified name之间使用"/"分隔|
|isTextOnly()|是否该元素只含有text或是空元素|
|isRootElement()|是否该元素是XML树的根节点|
