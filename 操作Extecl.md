# ———————————操作Extecl—————————— #

<p id="t"></p>

## :book:操作Extecl ##

:arrow_double_down:<a href="#a1">Apache POI</a>

:arrow_double_down:<a href="#a2">创建工作簿</a>

<p id="a1"><p>
  
### :custard:Apache POI ###

:arrow_double_up:<a href="#t">返回目录</a>

>Apache POI [1]  是用Java编写的免费开源的跨平台的 Java API，Apache POI提供API给Java程式对Microsoft Office格式档案读和写的功能。POI为“Poor Obfuscation Implementation”的首字母缩写，意为“简洁版的模糊实现”。

>Apache POI [1]  是创建和维护操作各种符合Office Open XML（OOXML）标准和微软的OLE 2复合文档格式（OLE2）的Java API。用它可以使用Java读取和创建,修改MS Excel文件.而且,还可以使用Java读取和创建MS Word和MSPowerPoint文件。Apache POI 提供Java操作Excel解决方案（适用于Excel97-2008）。

#### :wind_chime:结构 ####

```
HSSF [1]  － 提供读写Microsoft Excel XLS格式档案的功能。
XSSF [1]  － 提供读写Microsoft Excel OOXML XLSX格式档案的功能。
HWPF [1]  － 提供读写Microsoft Word DOC格式档案的功能。
HSLF [1]  － 提供读写Microsoft PowerPoint格式档案的功能。
HDGF [1]  － 提供读Microsoft Visio格式档案的功能。
HPBF [1]  － 提供读Microsoft Publisher格式档案的功能。
HSMF [1]  － 提供读Microsoft Outlook格式档案的功能。
```

<p id="a2"><p>
  
### :custard:创建工作簿 ###

:arrow_double_up:<a href="#t">返回目录</a>

首先需要添加Apache poi的包，其下载地址 [poi包](https://poi.apache.org/download.html)

然后再添加其引用：

```java
import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.ss.usermodel.Workbook;
```

如下在其同一目录下创建一个Extec.xls工作簿

```java
    public  static void main(String arg[])throws IOException
    {
        Workbook wb = new HSSFWorkbook(); //定义一个新的工作簿
        FileOutputStream output = new FileOutputStream("Extec.xls"); //创建一个输入流
        
        wb.write(output);
        output.close();
    }
```

运行后你就可以在与代码所在统一路径看到这个文件名。要注意的是需要加上文件扩展名。

**创建Sheet页**

Sheet是表的分页数据，Sheet在表格的左下角，是表格其他页面：

需要添加`import org.apache.poi.ss.usermodel.Sheet`引入sheet

```java
    public  static void main(String arg[])throws IOException
    {
        Workbook wb = new HSSFWorkbook(); //定义一个新的工作簿
        wb.createSheet("第一页");
        wb.createSheet("第二页");
        FileOutputStream output = new FileOutputStream("Extec.xls"); //创建一个输入流
        wb.write(output);
        output.close();
    }
```

运行后打开此文件（存在此文件先删除掉），可以看到如下：

![](https://github.com/Lumnca/Java/blob/master/img/3H%25%25__UJXF00TNWMTHKAE7J.png)

这就创建的sheet页，

**创建单元格**

创建单元格可以给表添加数据，通过行列数据来设置表格元素，需要添加行列引入：

```java
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Cell;
```

下面通过单元格来创建数据：

```java
    public  static void main(String arg[])throws IOException
    {
        Workbook wb = new HSSFWorkbook(); //定义一个新的工作簿
        Sheet sheet1 = wb.createSheet("第一页");

        Row row = sheet1.createRow(0);         //获取第一行
        Cell cell = row.createCell(0);         //获取第一行的第一列
        cell.setCellValue("第一个元素");        //为这个表格设置内容


        FileOutputStream output = new FileOutputStream("Extec.xls");
        wb.write(output);
        output.close();
    }
```

运行打开文件可以看到：

![](https://github.com/Lumnca/Java/blob/master/img/W(ESV%5DFPE%5DWPP0JDJ%60K2(L6.png)

可以在行上使用列直接设置属性,如下创建一个表格信息：

```java
    public  static void main(String arg[])throws IOException
    {
        Workbook wb = new HSSFWorkbook(); //定义一个新的工作簿
        Sheet sheet1 = wb.createSheet("第一页");

        Row row1 = sheet1.createRow(0);
        Row row2 = sheet1.createRow(1);
        Row row3 =  sheet1.createRow(2);

        row1.createCell(0).setCellValue("姓名");
        row1.createCell(1).setCellValue("学号");
        row1.createCell(2).setCellValue("年龄");

        row2.createCell(0).setCellValue("小明");
        row2.createCell(1).setCellValue("20170330");
        row2.createCell(2).setCellValue(18);

        row3.createCell(0).setCellValue("小红");
        row3.createCell(1).setCellValue("20170456");
        row3.createCell(2).setCellValue(18);

        FileOutputStream output = new FileOutputStream("Extec.xls"); //创建一个输入流
        wb.write(output);
        output.close();
    }
```

![](https://github.com/Lumnca/Java/blob/master/img/a1.png)


