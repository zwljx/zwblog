---
title: TestNG-DataProvider-3
date: 2021-10-25 10:08:37
tags: TestNG
categories: TestNG
---
### 自定义mysql格式参数化文件处理工具类

<!--more-->
#### 工具类实现
```
	public class MysqlData {
    int rows;
    int columns;
    Connection connection;
    Statement statement;
 
    public MysqlData(String ip, String port, String databaseName, String userName, String password) {
        try {
            Class.forName("com.mysql.jdbc.Driver");//加载数据库驱动
            String url = "jdbc:mysql://" + ip + ":" + port + "/" + databaseName;//数据库连接子协议
            System.out.println(url);
            this.connection = DriverManager.getConnection(url, userName, password);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
 
    /**
     * 通过传入的sql语句获取参数
     * @return
     */
    public Object[][] getMysqlDataBySQL(String sql){
        HashMap<String, String>[][] arrmap = null;
        //获取创建语句对象
        try {
            this.statement = connection.createStatement();
            //执行sql语句，获取查询结果集
            ResultSet result = statement.executeQuery(sql);
            //获取总行数
            this.rows = 0;
            while(result.next()) {
                rows++;
            }
            result.beforeFirst();
            //获取总列数
            ResultSetMetaData rd = result.getMetaData();
            this.columns = rd.getColumnCount();
            // 为了返回值是Object[][],定义一个多行单列的二维数组
            arrmap = new HashMap[rows][1];
            if (rows > 1) {
                for (int i = 0; i < rows; i++) {
                    arrmap[i][0] = new HashMap<>();
                }
            } else {
                System.out.println("sql查询结果集合中没有数据");
            }
            //循环每行
            int r = 0;
            while (result.next()) {
                //循环每列，如果不要id，则i设为2
                for (int i = 1; i <= columns; i++) {
                    String key = result.getMetaData().getColumnName(i);
                    String value = result.getString(i);
                    arrmap[r][0].put(key, value);
                }
                r++;
            }
 
        } catch (SQLException e) {
            e.printStackTrace();
        }
 
        return arrmap;
    }
}
```
#### 使用
```
	public class MySQLDataProviderTest {
 
    @DataProvider(name = "num")
    public Object[][] Numbers() throws BiffException, IOException {
        String ip = "";
        String port = "";
        String databaseName = "";
        String user = "";
        String password = "";
        MysqlData mysqlData = new MysqlData(ip,port,databaseName,user,password);
        String sql = "SELECT * FROM PHONE";
        return mysqlData.getMysqlDataBySQL(sql);
    }
 
    @Test(priority = 1, dataProvider = "num")
    public void test(HashMap<String, String> data){
        System.out.println(data.toString());
    }
}
```