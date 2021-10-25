---
title: TestNG-DataProvider-1
date: 2021-10-25 09:56:06
tags: TestNG
categories: TestNG
---
### 自定义txt格式参数化文件处理工具类
<!--more-->
#### 工具类实现
```
public class TxtData {
    int rows;
    int columns;
    public String fileName;
    public String delimiter;
    public ArrayList<String> arrkey = new ArrayList<String>();
    String sourceFile;
 
    public TxtData(String fileName, String delimiter){
        this.fileName = fileName;
        this.delimiter = delimiter;
    }
 
    /**
     * 通过分隔符获取参数
     * @return
     */
    public Object[][] getTxtDataByDelimiter(){
        List<String> dataList = null;
        try {
            dataList = FileUtil.readFile(getPath());
        } catch (IOException e) {
            e.printStackTrace();
        }
        rows = dataList.size();
        HashMap<String, String>[][] arrmap = new HashMap[rows - 1][1];
        if (rows > 1) {
            for (int i = 0; i < rows - 1; i++) {
                arrmap[i][0] = new HashMap<>();
            }
        } else {
            System.out.println("txt中没有数据");
        }
 
        columns = (dataList.get(0).split(delimiter)).length;
        for (int c = 0; c < columns; c++) {
            for (String key : dataList.get(0).split(delimiter)){
                arrkey.add(key);
            }
        }
 
        for (int r = 1; r < rows; r++) {
            String[] values = dataList.get(r).split(delimiter);
            for (int c = 0; c < columns; c++) {
                String value = values[c];
                arrmap[r - 1][0].put(arrkey.get(c), value);
            }
        }
 
        return arrmap;
    }
 
 
    /**
     * 获得txt文件的路径
     * @return
     * @throws IOException
     */
    public String getPath() throws IOException {
        File directory = new File(".");
        sourceFile = directory.getCanonicalPath() + "\\src\\main\\resources\\data\\"
                + fileName + ".txt";
        return sourceFile;
    }
 
}
```
#### 使用
```
	public class TxtDataproviderTest {
 
    @DataProvider(name = "num")
    public Object[][] Numbers() throws BiffException, IOException {
        TxtData data = new TxtData("testtxt", ",");
        return data.getTxtDataByDelimiter();
    }
 
    @Test(priority = 1,dataProvider = "num")
    public void test(HashMap<String, String> data){
        System.out.println(data.toString());
        int num1 = Integer.parseInt(data.get("num1"));
        int num2 = Integer.parseInt(data.get("num2"));
        int result = Integer.parseInt(data.get("result"));
        Assert.assertEquals(num1+num2 , result);
    }
 
}
```