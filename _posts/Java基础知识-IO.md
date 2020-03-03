---
title: Java基础知识-IO
date: 2019-03-03 21:36:25
author: fuf
notebook: blog
evernote-version: 0
source: 原创/转载
thumbnail: 
tags:
    - 默认
blogexcerpt:
---
 
# Java基础知识-IO

# 简介
Java IO 也称为IO流，IO = 流，它的核心就是对文件的操作，对于 字节 、字符类型的输入和输出流。

Java.io 包几乎包含了所有操作输入、输出需要的类。所有这些流类代表了输入源和输出目标。

Java.io 包中的流支持很多种格式，比如：基本类型、对象、本地化字符集等等。

一个流可以理解**为一个数据的序列。输入流表示从一个源读取数据，输出流表示向一个目标写数据。**
<!-- more -->
Java 为 I/O 提供了强大的而灵活的支持，使其更广泛地应用到文件传输和网络编程中。

# 1， File 类

File类是对文件系统中文件以及文件夹进行封装的对象，可以通过对象的思想来操作文件和文件夹。File类保存文件或目录的各种数据信息，包括文件名、文件长度、最后修改时间、是否可读、获取当前文件的路径名、判断文件是否存在、获取当前目录中的文件列表、创建、删除文件和目录等方法。

```
import java.io.File;
 
public class DirList {
    public static void main(String args[]) {
        String dirname = "/java";
        File f1 = new File(dirname);
        if (f1.isDirectory()) {
            System.out.println("Directory of " + dirname);
            String s[] = f1.list();
            for (int i = 0; i < s.length; i++) {
                File f = new File(dirname + "/" + s[i]);
                if (f.isDirectory()) {
                    System.out.println(s[i] + " is a directory");
                } else {
                    System.out.println(s[i] + " is a file");
                }
            }
        } else {
            System.out.println(dirname + " is not a directory");
        }
    }
}
```

```
  private static String readAll(Reader rd) throws IOException {
        StringBuilder sb = new StringBuilder();
        int cp;
        while ((cp = rd.read()) != -1) {
            sb.append((char) cp);
        }
        return sb.toString();
    }

```


# 2. 字节流(InputStream,OutputStream)

[字节/符流](../../hexo_pages/img/InputOutputStreams.png)

# 3. 字符流(Reader，Writer)

# 4. 转换流(OutputStreamWriter、InputStreamReader)
## InputStreamReader
InputStreamReader 是字符流Reader的子类，是字节流通向字符流的桥梁。你可以在构造器重指定编码的方式，如果不指定的话将采用底层操作系统的默认编码方式，例如 GBK 等。要启用从字节到字符的有效转换，可以提前从底层流读取更多的字节，使其超过满足当前读取操作所需的字节。一次只读一个字符。

- InputStreamReader构造函数
  
```
InputStreamReader(Inputstream  in) //创建一个使用默认字符集的 InputStreamReader。

InputStreamReader(Inputstream  in，Charset cs) //创建使用给定字符集的 InputStreamReader。

InputStreamReader(InputStream in, CharsetDecoder dec) //创建使用给定字符集解码器的 InputStreamReader。

InputStreamReader(InputStream in, String charsetName)  //创建使用指定字符集的 InputStreamReader。
```
- 一般方法

```
void close() //关闭该流并释放与之关联的所有资源

String getEncoding() //返回此流使用的字符编码的名称

int read() //读取单个字符

int read(char[] cbuf,int offset,int length) //将字符读入数组中的某一部分
boolean ready() //判断此流是否已经准备好用于读取。

```

## OutputStreamWriter
OutputStreamWriter 是字符流Writer的子类，是字符流通向字节流的桥梁。每次调用 write()方法都会导致在给定字符（或字符集）上调用编码转换器。在写入底层输出流之前，得到的这些字节将在缓冲区中累积。一次只写一个字符。

## 需要注意的事项
InputStreamReader、OutputStreamWriter实现从字节流到字符流之间的转换，使得流的处理效率得到提升，但是如果我们想要达到最大的效率，我们应该考虑使用缓冲字符流包装转换流的思路来解决问题。比如：
```
BufferedReader in = new BufferedReader(new InputStreamReader(System.in));

```

```
public static void copyFile(String rString, String filename) {

        InputStream inputStream = null;
        InputStreamReader inputStreamReader = null;

        OutputStream outputStream = null;
        OutputStreamWriter outputStreamWriter = null;


        try {

            inputStream = new FileInputStream(rString); //创建输入流
            inputStreamReader = new InputStreamReader(inputStream); //创建转换输入流

            outputStream = new FileOutputStream(filename); //创建输出流
            outputStreamWriter = new OutputStreamWriter(outputStream); //创建转换输出流

            int result = 0;

            while ((result = inputStreamReader.read()) != -1) {  //一次只读一个字符
                outputStreamWriter.write(result); //一次只写一个字符
            }

            outputStreamWriter.flush();  //强制把缓冲写入文件


        } catch (IOException e) {
            e.printStackTrace();

        } finally {
            if (inputStreamReader != null) {
                try {
                    inputStreamReader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if (outputStreamWriter != null) {
                try {
                    outputStreamWriter.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }

```

# 5. 压缩流
在日常的使用中经常会使用到像WinRAR或WinZIP这样的压缩文件，通过这些软件可以把一个很大的文件进行压缩以方便传输。
在JAVA中 为了减少传输时的数据量也提供了专门的压缩流，可以将文件或文件夹压缩成ZIP、JAR、GZIP等文件的格式。