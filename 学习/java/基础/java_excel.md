##java 用POI 读取excel数据（行，列，总行，总列.....） 
```
            FileInputStream inp = new FileInputStream("E:\\WEIAN.xls"); 
            HSSFWorkbook wb = new HSSFWorkbook(inp);
            HSSFSheet sheet = wb.getSheetAt(2); // 获得第三个工作薄(2008工作薄)
            // 填充上面的表格,数据需要从数据库查询
            HSSFRow row5 = sheet.getRow(4); // 获得工作薄的第五行
            HSSFCell cell54 = row5.getCell(3);// 获得第五行的第四个单元格
            cell54.setCellValue("测试纳税人名称");// 给单元格赋值
            int coloumNum=sheet.getRow(0).getPhysicalNumberOfCells();//获得总列数
            int rowNum=sheet.getLastRowNum();//获得总行数




int coloumNum = sheet.getRow(0).getPhysicalNumberOfCells();// 获得总列数
int rowNum = sheet.getLastRowNum();// 获得总行数
System.err.println(k + "表" + coloumNum + "列，" + rowNum + "行");
``` ##java : 解析ftp上的Excel文件
```
//远程ftp读取读取     
URL u=new URL(url);//这个是ftp文件完整地址      
URLConnection urlconn=u.openConnection();      
urlconn.getInputStream();//获得InputStream流。     
下面可以针对不同的文件格式，对其进行不同的包装。     
    
//如excel文件     
//xlsx     
OPCPackage opc = OPCPackage.open(urlconn.getInputStream());     
XSSFWorkbook      xwb = new XSSFWorkbook(opc);     
    
//xls     
POIFSFileSystem pf = new POIFSFileSystem(urlconn.getInputStream());     
HSSFWorkbook      HSSFWorkbook wb = new HSSFWorkbook(pf);    
```