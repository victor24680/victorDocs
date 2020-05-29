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
## 字节流
```
File f = new File("d:/1.txt");
//基于文件的输入流
try{
    FileInputStream stream = new FileInputStream(f);
    //创建字节数组，其长度就是文件的长度
    byte[] all = new byte[(int) f.length()];
    stream.read(all);//读出的是ASCII字节，需要自己转换
    stream.write(data);//写入ASCII字节
}catch(IOException e){
}finally{
    stream.close();//关闭流（一般写在finally里)
}
#注意：如果把流定义在try里，那结束之后，会自动关闭(JDK7之后)
```
## 字符流
字符流是基于 `FileReader` 进行文件读取的
```java
File f = new File("d:/file/1.txt");
try(FileReader fr = new FileReader(f)){
    char[] all = new char[(int) f.length];
    fr.read(all);//以字符流的形式读取文件内容
    fr.write(all);//以字符流的形式写入内容
}catch(Exception e){
}
```
备注：字节流，字符流，都是以硬盘为介质读取和写入的，每次读写，都会访问硬盘，性能不佳。

## 缓存流
缓存在读取的时候，一次性读较多的数据到缓存中，以后每一次的读取，都是在缓存中访问，<br />
直到缓存中的数据读取完毕，再到硬盘中读取,从而减少了IO操作(类似于吃饭的操作)。
```java
File f = new File("d:/file/1.txt");
//文件读取
try(FileReader fr = new FileReader(f);BufferedReader br = new BufferedReader(fr);){
    while(true){
        String line = br.readLine();//逐行读取
        if(null == line){
            break;
        }
        System.out.println(line);
    }
}catch(Exception e){
}
//文件写入
try(FileWriter fr = new FileWriter(f);PrintWriter br = new PrintWriter(fr);){
    br.println("OK");
    br.flush();//立即写入的硬盘中，无论缓存是否已满
    br.println("HK");
}catch(Exception e){
}
```
## 数据流(两者写入与读取必须是对应的，否则会出错)
```java
File f = new File("d:/file/1.txt");
//文件读取
try(FileInputStream fr = new FileInputStream(f);DataInputStream br = new DataInputStream(fr);){
    boolean b = br.readBoolean();//去取的到的布尔值
    int i = br.readInt();//去取到的整数
    String str = br.readUTF();//读取的到的字符串
}catch(Exception e){
}
//文件写入
try(FileOutputStream fr = new FileOutputStream(f);DataOutputStream br = new DataOutputStream(fr);){
   br.writeBoolean(true);
   br.writeInt(300);
   br.writeUTF("OKOK");
}catch(Exception e){
}
```
## 对象流
```java
File f =new File("d:/tt.lol");//用于保存一个对象
XXObject xx = new XXObject();//对象必须实现序列化接口
//文件读取
try(FileInputStream fr = new FileInputStream(f);ObjectInputStream  br = new ObjectInputStream (fr);){
    XXObject h = （XXObject）br.readObject();//对象读取
}catch(Exception e){
}
//文件写入
try(FileOutputStream fr = new FileOutputStream(f);ObjectOutputStream br = new ObjectOutputStream(fr);){
   br.writeObject(xx);//对象写入
}catch(Exception e){
}
```
## 控制台中读取数据
```java
    Scanner s = new Scanner(System.in);//从控制台中读取数据
    int a = s.nextInt();//读取整数
```
## IO流关系(总结)
- 流 -> 字节流 -> 数据流
- 流 -> 字节流 -> 对象流
- 流 -> 字符流 -> 缓存流
- 文件输入输出流 `InputStream`,`OutputStream`
- 基于文件的（输入/输出）`FileInputStream`,`FileOutputSream`流的流有：字节流、数据流、对象流
- 基于文件Reader `FileReader`,`FileWrite` 流的流有：字符流、缓存流