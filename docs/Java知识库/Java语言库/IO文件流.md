# 文件对象
文件夹和文件都用 `File` 对象表示
```java
File f = new File("XXX.txt");
//获取文件的绝对路径
f.getAbsolutePath();
//检测文件是否存在
f.exists();
//检测是否为文件夹
f.iisDirectory();
//检测是否为文件
f.isFile();
//获取文件的修改时间
f.lastModified();
//手动设置文件修改时间
f.setLastModified(0);
//文件重命名
f.rename(new File("xxx.txt"));

=====================
//以字符串数组的形式，返回当前文件夹下的所有文件（不包含子文件夹及子文件）
f.list();
//以文件数组的形式，返回当前文件夹下的所有文件不包含子文件夹及子文件）
File[] fs=f.listFiles();
//以字符串形式返回获取所在的文件夹
f.getParent();
//以文件形式返回获取所在的文件夹
f.getParentFile();
//创建文件夹
f.mkdir();//不能创建父级文件夹
f.mkdirs();//包含父级文件夹，一起创建；

//创建一个空文件,父级文件夹必须存在
f.createNewFile();
f.createNewFile().mkdirs();//会相应的创建父级文件夹
//列出所有的磁盘名称
f.listRoots();
==================
//删除文件
f.delete();
//JVM虚拟机结束时，删除文件，
f.deleteOnExit();
```

# 流
当不同的介质之间有数据交互时，Java使用流来实现数据交互
1. 输入流 `InputStream`  读取文件中的数据到程序（JVM）中
2. 输出流 `OutputStream`
## 文件输入流
```
File f = new File("d:/1.txt");
//基于文件的输入流
FileInputStream stream = new FileInputStream(f);

```

