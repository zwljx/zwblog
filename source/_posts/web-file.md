---
title: web-文件上传
date: 2021-10-25 09:32:48
tags: web
categories: web
---
### web开发-文件上传功能

<!--more-->

#### 前端
```
<div style={{marginTop: 30, marginBottom: 20,width:'50%',marginLeft: 20}}>
                        <span style={{display: 'inline-block', width: 150}}> <Icon type="to-top"/>上传单个文件</span>
                        <Upload
                            name="file"
                            action="http://localhost:8080/upload"
                            /*onChange = {this.handleChange}
                             onRemove = {this.remove}*/>
                            <Button>
                                <Icon type="upload" /> Click to Upload
                            </Button>
                        </Upload>
                    </div>

                    <div style={{marginTop: 30, marginBottom: 20,width:'50%',marginLeft: 20}}>
                        <span style={{display: 'inline-block', width: 150}}> <Icon type="to-top"/>上传多个文件</span>
                        <Upload
                            name="file"
                            action="http://localhost:8080/multiUpload"
                            multiple={true}
                            /*onChange = {this.handleChange}
                             onRemove = {this.remove}*/>
                            <Button>
                                <Icon type="upload" /> Click to Upload
                            </Button>
                        </Upload>
                    </div>
```

#### 后端

##### 单文件上传
```
@PostMapping("/upload")
    @ResponseBody
    public String upload(@RequestParam("file") MultipartFile file) {
        if (file.isEmpty()) {
            return "上传失败，请选择文件";
        }

        try {
            String fileName = file.getOriginalFilename();
            File directory = new File(".");
            String filePath = directory.getCanonicalPath() + "\\src\\main\\resources\\files\\";
            File dest = new File(filePath + fileName);
            file.transferTo(dest);
            System.out.println("success");
            return "上传成功";
        } catch (IOException e) {
            System.out.println("fail");
        }
        return "上传失败！";
    }
```

##### 多文件上传
```
@PostMapping("/multiUpload")
    @ResponseBody
    public String multiUpload(HttpServletRequest request) {
        List<MultipartFile> files = ((MultipartHttpServletRequest) request).getFiles("file");
        File directory = new File(".");
        String filePath = null;
        try {
            filePath = directory.getCanonicalPath() + "\\src\\main\\resources\\files\\";
        } catch (IOException e) {
            e.printStackTrace();
            return "error";
        }
        for (int i = 0; i < files.size(); i++) {
            MultipartFile file = files.get(i);
            if (file.isEmpty()) {
                return "上传第" + (i++) + "个文件失败";
            }
            String fileName = file.getOriginalFilename();

            File dest = new File(filePath + fileName);
            try {
                file.transferTo(dest);
                System.out.println("第" + (i + 1) + "个文件上传成功");
            } catch (IOException e) {
                System.out.println(e.toString());
                return "上传第" + (i++) + "个文件失败";
            }
        }

        return "上传成功";
    }
```

#### 文件操作工具类
```
import org.apache.commons.lang3.StringUtils;

import java.io.*;
import java.util.ArrayList;
import java.util.List;


public class FileUtil {
    /**
     * 按行读取文件，一行为一个String
     * @param path filePath
     * @return
     */
    public static List<String> readFile(String path){
        List<String> result = new ArrayList<String>();
        FileReader reader = null;
        try {
            reader = new FileReader(path);

            BufferedReader br = new BufferedReader(reader);
            String str = null;
            while ((str = br.readLine()) != null) {
                if(StringUtils.isNotEmpty(str)) {
                    result.add(str);
                }
            }
            br.close();
            reader.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return result;
    }

    /**
     * 读文件结果保存为一个字符串中
     */
    public static String readFileToString (String path) {
        File file02 = new File(path);
        FileInputStream is = null;
        StringBuilder stringBuilder = null;
        try {
            if (file02.length() != 0) {
                /**
                 * 文件有内容才去读文件
                 */
                is = new FileInputStream(file02);
                InputStreamReader streamReader = new InputStreamReader(is);
                BufferedReader reader = new BufferedReader(streamReader);
                String line;
                stringBuilder = new StringBuilder();
                while ((line = reader.readLine()) != null) {
                    // stringBuilder.append(line);
                    stringBuilder.append(line);
                }
                reader.close();
                is.close();
            } else {
                return "";
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
        return String.valueOf(stringBuilder);

    }
    /**
     * 写入文件
     * @param path
     * @param
     */
    public static void writeToFile(String path ,String content) {
        FileWriter fw = null;
        try {
            //如果文件存在，则追加内容；如果文件不存在，则创建文件
            File f=new File(path);
            fw = new FileWriter(f, true);
        } catch (IOException e) {
            e.printStackTrace();
        }
        PrintWriter pw = new PrintWriter(fw);
        pw.println(content);
        pw.flush();
        try {
            fw.flush();
            pw.close();
            fw.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void deleteFile(String path) {
        File f = new File(path);
        if (f.exists()){
            f.delete();
        }
    }
}
```