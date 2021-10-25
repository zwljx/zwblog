---
title: selenium-编码前准备
date: 2021-10-22 14:13:52
tags: selenium
categories: selenium
---
selenium Java编码准备
<!--more-->

### selenium Java maven依赖
```
<dependency>
	<groupId>org.seleniumhq.selenium</groupId>
	<artifactId>selenium-java</artifactId>
	<version>2.44.0</version>
</dependency>
```
### selenum 启动firefox浏览器注意事项

selenium启动Firefox，不需要额外的driver
如果没有安装到默认路径C盘，需要设置路径
```
System.setProperty("webdriver.firefox.bin","D:/……/Mozilla Firefox/firefox.exe");
WebDriver driver = new FirefoxDriver();
```

### firefox的版本与selenium的版本对应关系
 【Selenium】 -> 【FireFox】
2.25.0 -> 18
2.30.0 -> 19
2.31.0 -> 20
2.42.2 -> 29
2.44.0 -> 33 (不支持31)
2.53.0 -> 43,46(不支持47)
2.41.0 -> 26(绿色版本)
2.44 -> 32.0-35.0


#### chrome版本与chromedriver版本对应关系

|ChromeDriver Version	|Chrome Version|
|  ----  | ----  |
|83.0.4103.39|	83|
|83.0.4103.14|	83
|81.0.4044.138|	81
|81.0.4044.69	|81
|81.0.4044.20	|81
|80.0.3987.106	|80
|80.0.3987.16	|80
|79.0.3945.36	|79
|79.0.3945.16	|79
|78.0.3904.105	|78
|78.0.3904.70	|78
|78.0.3904.11	|78
|77.0.3865.40	|77
|77.0.3865.10	|77
|76.0.3809.126	|76
|76.0.3809.68	|76
|76.0.3809.25	|76
|76.0.3809.12	|76
|75.0.3770.90	|75
|75.0.3770.8	|75
|74.0.3729.6	|74
|73.0.3683.68	|73
|72.0.3626.69	|72
|2.46	|71-73
|2.46	|71-73
|2.45	|70-72
|2.44	|69-71
|2.43	|69-71
|2.42	|68-70
|2.41	|67-69
|2.40	|66-68
|2.39	|66-68
|2.38	|65-67
|2.37	|64-66
|2.36	|63-65
|2.35	|62-64