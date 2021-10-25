---
title: TestNG-DataProvider-1
date: 2021-10-25 09:51:15
tags: TestNG
categories: TestNG
---
### 自定义excel格式参数化文件处理工具类-POI库，支持高版本
<!--more-->
#### 工具类实现
```
	public class ExcelDataNew {
    public Workbook workbook;
    public Sheet sheet;
    public Cell cell;
    int rows;
    int columns;
    public String fileName; //区别于ExcelData，该fileName需要带后缀
    public String caseName;
    public ArrayList<String> arrkey = new ArrayList<String>();
    String sourceFile;
 
    /**
     * @param fileName   excel文件名
     * @param caseName   sheet名
     */
    public ExcelDataNew(String fileName, String caseName) {
        super();
        this.fileName = fileName;
        this.caseName = caseName;
        setWorkbook();
    }
 
    /**
     * 判断，设置workbook
     * xls格式的需要使用HSSFWorkbook类来解析，xlsx格式的需要使用XSSFWorkbook格式来解析
     */
    public void setWorkbook(){
        if(this.fileName == null){
            return;
        }
        String extString = fileName.substring(fileName.lastIndexOf("."));
        InputStream is = null;
        try {
            is = new FileInputStream(getPath());
            if(".xls".equals(extString)){
                this.workbook = new HSSFWorkbook(is);
            }else if(".xlsx".equals(extString)){
                this.workbook = new XSSFWorkbook(is);
            }
 
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
 
    /**
     * 获取cell中的值，转换为String类型
     * @param cell
     * @return
     */
    public static String getCellFormatValue(Cell cell) {
        String cellValue = "";
        if (cell != null) {
            //判断cell类型
            switch (cell.getCellType()) {
                case Cell.CELL_TYPE_NUMERIC: {
                    cellValue = String.valueOf(cell.getNumericCellValue());
                    break;
                }
                case Cell.CELL_TYPE_STRING: {
                    cellValue = cell.getRichStringCellValue().getString();
                    break;
                }
                default:
                    cellValue = "";
            }
        }
        return cellValue;
    }
 
    /**
     * 获得excel表中的数据
     */
    public Object[][] getExcelData() throws BiffException, IOException {
        this.sheet = workbook.getSheet(caseName);
        this.rows = sheet.getPhysicalNumberOfRows();
        this.columns = sheet.getRow(0).getPhysicalNumberOfCells();
        // 为了返回值是Object[][],定义一个多行单列的二维数组
        HashMap<String, String>[][] arrmap = new HashMap[rows - 1][1];
        // 对数组中所有元素hashmap进行初始化
        if (rows > 1) {
            for (int i = 0; i < rows - 1; i++) {
                arrmap[i][0] = new HashMap<>();
            }
        } else {
            System.out.println("excel中没有数据");
        }
 
        // 获得首行的列名，作为hashmap的key值
        Row rowHead = sheet.getRow(0);
        for (int c = 0; c < columns; c++) {
            String cellvalue = getCellFormatValue(rowHead.getCell(c));
            arrkey.add(cellvalue);
        }
        // 遍历所有的单元格的值添加到hashmap中
        for (int r = 1; r < rows; r++) {
            Row row = sheet.getRow(r);
            for (int c = 0; c < columns; c++) {
                String cellvalue = getCellFormatValue(row.getCell(c));
                arrmap[r - 1][0].put(arrkey.get(c), cellvalue);
            }
        }
        return arrmap;
    }
 
    /**
     * 获得excel文件的路径
     * @return
     * @throws IOException
     */
    public String getPath() throws IOException {
        File directory = new File(".");
        sourceFile = directory.getCanonicalPath() + "\\src\\main\\resources\\data\\"
                + fileName;
        return sourceFile;
    }
 
}
```
#### 使用
```
	public class ExcelDataproviderTest {
 
    @DataProvider(name = "num")
    public Object[][] Numbers() throws BiffException, IOException {
        ExcelDataNew data = new ExcelDataNew("testexcel.xlsx", "test");
        return data.getExcelData();
    }
 
    @Test(priority = 1, dataProvider = "num")
    public void test(HashMap<String, String> data) {
        System.out.println(data.toString());
        int num1 = Integer.parseInt(data.get("num1"));
        int num2 = Integer.parseInt(data.get("num2"));
        int result = Integer.parseInt(data.get("result"));
        Assert.assertEquals(num1 + num2, result);
    }
}
```