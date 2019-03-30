# ———————————操作Extecl—————————— #

<p id="t"></p>

## :book:操作Extecl ##

:arrow_double_down:<a href="#a1">Apache POI</a>

:arrow_double_down:<a href="#a2">创建工作簿</a>

:arrow_double_down:<a href="#a3">单元格读取</a>

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

**创建一个时间格式的单元格**

```java
    public  static void main(String arg[])throws IOException
    {
        Workbook wb = new HSSFWorkbook(); //定义一个新的工作簿
        Sheet sheet1 = wb.createSheet("第一页");

        Row row1 = sheet1.createRow(0);
        Cell cell1 = row1.createCell(0);
        Cell cell2 = row1.createCell(1);

        cell1.setCellValue(new Date());  //添加日期
        cell2.setCellValue(Calendar.getInstance());  //日历

        //单元格样式类
        CellStyle cellStyle = wb.createCellStyle();
        CreationHelper creationHelper = wb.getCreationHelper();
        //设定日期格式
        cellStyle.setDataFormat(creationHelper.createDataFormat().getFormat("yyyy-mm-dd hh:mm:ss"));

        //添加样式
        cell1.setCellStyle(cellStyle);
        cell2.setCellStyle(cellStyle);

        FileOutputStream output = new FileOutputStream("Extec.xls"); //创建一个输入流
        wb.write(output);
        output.close();
    }
```

这样就可以在单元格上显示日期格式，由于单元格初始长度不够，所以需要扩展单元格长度，才能显示其内容，当然也可以设置单元格长度来自适应。

<p id="a3"><p>
  
### :custard:单元格读取 ###

:arrow_double_up:<a href="#t">返回目录</a>

前面所说的都是创建一个表，下面介绍如何读取一个表：

首先这是我创建的Extec表的内容：

![](https://github.com/Lumnca/Java/blob/master/img/a2.png)


```java
    public  static void main(String arg[])throws IOException
    {
        InputStream in = new FileInputStream("Extec.xls");   //创建一个输入流
        POIFSFileSystem fs = new POIFSFileSystem(in);        

        HSSFWorkbook wb = new HSSFWorkbook(fs);            //创建一个工作簿
        HSSFSheet hssf = wb.getSheetAt(0);                  //获取sheet第一页

        if(hssf==null){
            System.out.println("该文件没有存在");
        }
        else{
        
          //循环遍历输出
        
            for(int rowNum = 0; rowNum <= hssf.getLastRowNum();rowNum++){          //getLastRowNum()获取最后一行行标
                HSSFRow hssfRow = hssf.getRow(rowNum);               //获取当前行
                if(hssfRow==null){
                    continue;
                }
                for(int cellNum = 0;cellNum<=hssfRow.getLastCellNum();cellNum++){    //获取列
                    HSSFCell cell = hssfRow.getCell(cellNum);
                    if(cell==null){
                        continue;
                    }
                    System.out.print(" "+getValue(cell));  
                }
                System.out.println();
            }
        }



    }
    //类型处理，以相对应的类型输出
    private  static String getValue(HSSFCell cell){
        
        if(cell.getCellType()==HSSFCell.CELL_TYPE_BOOLEAN){
            return  String.valueOf(cell.getBooleanCellValue());
        }
        
        else if(cell.getCellType()==HSSFCell.CELL_TYPE_NUMERIC){
            return  String.valueOf(cell.getNumericCellValue());
        }
        
        else {
            return  String.valueOf(cell.getStringCellValue());
        }
    }
```

点击运行在控制台输出：

```
 姓名 学号 年龄
 小明 201703.0 19.0
 小红 201702.0 18.0
 小刚 201801.0 17.0
```

与表格数据一致，只是类有些不同，


上面是作为表格形式输出，下面可以直接作为文本提取输出：

```java
    public  static void main(String arg[])throws Exception
    {
        InputStream in = new FileInputStream("Extec.xls");
        POIFSFileSystem fs = new POIFSFileSystem(in);

        HSSFWorkbook wb = new HSSFWorkbook(fs);

        //作为文本直接输出

        try {
            ExcelExtractor exec = new org.apache.poi.hssf.extractor.ExcelExtractor(wb);
            System.out.println(exec.getText());
        }
        catch (Exception e){
            e.printStackTrace();
        }
        
    }
```

控制台输出：

```
第一页
姓名	学号	年龄
小明	201703	19
小红	201702	18
小刚	201801	17
```

可以看到这样输出会把sheet的界面内容也显示出来。这些做比用循环迭代方便的多，但是这样就不能对单元格内容做处理，这种方式适用于只用于读取文件。




