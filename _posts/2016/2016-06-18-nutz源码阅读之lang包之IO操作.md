---
layout: post
category : 学习笔记 
keywords: "nutz,源码学习"
description : "本文主要说说IO处理工具Streams和文件处理工具类Files。"
tags : [nutz,源码学习]
---

本文主要说说IO处理工具Streams和文件处理工具类Files。
这里事先说明下，以前和以后的文章里面，粘贴的代码主要是为了说明实现逻辑登，我会把一些异常处理，等无关逻辑的代码删除掉，避免文章出现大段大段的代码。
 

<!--break-->

{% include JB/setup %}
 
 
## Streams

流操作工具类，之前一般用的第三方流工具类是：apache commons包中的IOUtils；Streams和IOUtils封装的方法基本一致，都是针对流的读操作，写操作，关闭.
另外IOUtils中也封装了copy方法，本质上其实就是从一个流读入内容，然后写到另一个流中。

辣么看看Streams封装流的读写操作。

ps：遥想当年初学编程的时候，流的操作和线程，网络被俺视为三个最难的拦路虎。不过现在好多第三方库，即使对这些底层不是特别熟悉，也能写出不错的程序。
不过作为码农，还是找机会多了解下这些稍微底层的知识了。

### 流的输出

主要包含将一些内容写出到输出流中，这些内容可以是文本，输入流，文本流，数组.总共封装了大概9个方法，但是最基础的还是针对四种不同的输出形定义的四个方法：

- 将一段文本写到输出流中

```java

    public static void write(Writer writer, CharSequence cs) throws IOException {
        if (null != cs && null != writer) {
            writer.write(cs.toString());
            writer.flush();
        }
    }
```
- 将一个输入流：InputStream输出

```java

    public static long write(OutputStream ops, InputStream ins, int bufferSize)
            throws IOException {
        if (null == ops || null == ins)
            return 0;

        byte[] buf = new byte[bufferSize];
        int len;
        long bytesCount = 0;
        while (-1 != (len = ins.read(buf))) {
            bytesCount += len;
            ops.write(buf, 0, len);
        }
        ops.flush();
        return bytesCount;
    }
```
- 将一个文本流：Reader输出

```java

    public static void write(Writer writer, Reader reader) throws IOException {
        if (null == writer || null == reader)
            return;

        char[] cbuf = new char[BUF_SIZE];
        int len;
        while (-1 != (len = reader.read(cbuf))) {
            writer.write(cbuf, 0, len);
        }
    }
```
- 将一个字节数组输出

```java

        public static void write(OutputStream ops, byte[] bytes) throws IOException {
            if (null == ops || null == bytes || bytes.length == 0)
                return;
            ops.write(bytes);
        }
```

嗯嗯，贴着些代码，纯粹是为了复习下IO流的操作。

不过使用nutz的Streams的时候，我们需要注意，这个类提供的方法有关闭流和不关闭流的区别。使用的时候要注意，如果调用的是没有关闭流的方法，需要自己另外关闭流。反之不要去重复关闭流。


### 流的输入

流的输入就不细说了。整体设计跟输出没有大的差别，只是输出变成输入而已。
同样输入的对象，可以是文本（流），字节数组，输入流，


## Files
作为对比，可以参考apche common包中的FileUtils 和google guava库中的Files。
该类主要封装了对文件和目录的，读，些，拷贝，删除等功能。

### 文件读写

该类中具体针对文件的读和写操作，都是调用Streams类的读写操作的，所以这里就不在说了。

### findFile
无论对文件进行什么操作，首先要先找到这个文件，findFile是被其他方法调用次数最多的一个方法。
该方法根据指定的路径等参数，获取一个文件对象File。
该方法首先通过Disks.absolute(path, klassLoader, enc); 获取文件的绝对路径，然后用绝对路径new一个File对象返回。
那么重点是这个absolute方法。

findFile将用户输入的文件路径参数归为三类：

- 绝对路径，比如：/home/zzh/.zdoc
- 相对系统用户目录,比如：~/abc.txt
- 当前应用中的路径，比如：哈哈/abc.txt

绝对路径不用处理，直接返回即可。第二种,通过这种方式获取绝对路径：

    System.getProperty("user.home")+"/abc.txt"
    
这种场景的用户要注意，不要出现，你文件在root用户目录下，运行程序的时候使用其他用户来运行，就找不到文件了。

针对 第三种方式，调用classloader获取文件的绝对路径，不过获取完毕后，要对文件进行解码,路径中的中文字符会被使用UTF8进行编码，所以要用UTF8进行解码。具体代码如下：

    path = klassLoader.getResource(path);
    return URLDecoder.decode(path, enc);

### 后缀名相关操作
针对文件后缀名相关操作，其实不涉及文件内容本身，只是针对文件路径名称进行操作。所以本质就是字符串的操作。这里就不细说了。
后缀名相关操作主要封装了更改后缀名，获取后缀名，获取文件主名称（不包含路径和后缀名）

呃，其实越是这种基础的，越容易看出编码水平的差距。比如获取后缀名这里：

方法1（nutz中写法）：

```java

    int p0 = path.lastIndexOf('.');
    return path.substring(p0 + 1);
```

方法2（我们项目里写法）：

```java

    String[] strArr = path.split("\\.");
    return strArr[strArr.length-1];
```

虽然都是两行代码，但是明显方法1要比方法2好的多。在不应想代码阅读理解的情况下，也兼顾了性能。
 
 
其他还有很多目录相关操作
 
### 其他
 
其他文件（目录）拷贝，删除，等操作主要也是调用java api，这里不细说。
 