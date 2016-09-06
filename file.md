##文件的基本操作

基本操作包括：创建，删除，属性等。

```
            File file = new File("test.txt");
            if(!file.exists()){
                try {
                    file.createNewFile();
                    System.out.println("文件创建成功");
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }else{
            //****************属性**********************//
                System.out.println("文件已经存在");
                System.out.println("文件名:"+file.getName());
                System.out.println("文件绝对路径:"+file.getAbsolutePath());
                System.out.println("文件相对路径:"+file.getPath());
                System.out.println("文件大小："+file.length());//字节
                System.out.println("文件是否可读："+file.canRead());
                System.out.println("文件是否可写："+file.canWrite());
                System.out.println("文件是否隐藏："+file.isHidden());
                //重命名
                File newfile = new File("newfile.txt");
                file.renameTo(newfile);
                System.out.println("文件被重命名了");
                //文件删除
                file.delete();
                System.out.println("文件被删除了");

            }
```
##目录的基本操作

基本等同于文件操作。目录删除的话，得确保目录是空的。
```
//目录创建
    File dir = new File("dir1/dir2");
    if(!dir.exists()){
        dir.mkdirs();
    }
```

###读取assets目录里的文件数据

我们创建assets目录，跟res同级，然后在里面创建一个文本文件。
```
            try {
                InputStream is = getResources().getAssets().open("wenben.txt");//返回的是字节流InputStram流类型
                //我们要读取字符，就要将字节流包装成字符流
                InputStreamReader isr = new InputStreamReader(is,"UTF-8");
                //进一步将字符流包装成buffer流，buffer会自动提供一个缓冲区
                BufferedReader bfr = new BufferedReader(isr);
                //System.out.println(bfr.readLine());//好处是可以直接读取一行数据
                String str = "";
                while((str =bfr.readLine())!=null){
                    System.out.println(str);
                }
            } catch (IOException e) {
                e.printStackTrace();
            }

```

###读取raw目录中的文件数据

我们在res下面，创建raw目录，然后在里面创建一个文本文件。

```
            try {
                InputStream is = getResources().openRawResource(R.raw.test);//自动创建id
                //我们要读取字符，就要将字节流包装成字符流
                InputStreamReader isr = new InputStreamReader(is,"UTF-8");
                //进一步将字符流包装成buffer流，buffer会自动提供一个缓冲区
                BufferedReader bfr = new BufferedReader(isr);
                //System.out.println(bfr.readLine());//好处是可以直接读取一行数据
                String str = "";
                while((str =bfr.readLine())!=null){
                    System.out.println(str);
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
```

###读写内部存储数据

####写入数据

```
            try {
                FileOutputStream fos = openFileOutput("t.txt",Context.MODE_PRIVATE);//返回的是字节流
                OutputStreamWriter osw = new OutputStreamWriter(fos,"UTF-8");//包装类
                osw.write("this is a test");//直接写入string
                osw.flush();
                fos.flush();
                osw.close();
                fos.close();
                Toast.makeText(getApplicationContext(),"写入成功",Toast.LENGTH_LONG).show();
            } catch (IOException e) {
                e.printStackTrace();
            }
```

####读数据

```
            try {
                FileInputStream fis = openFileInput("t.txt");//返回的是字节流
                InputStreamReader isr = new InputStreamReader(fis,"UTF-8");//包装类
                //用字节数组装数据
                char input[] = new char[fis.available()];//获取文件可用长度
                isr.read(input);
                //先打开的后关闭
                isr.close();
                fis.close();
                String str = new String(input);
               System.out.println(str);
            } catch (IOException e) {
                e.printStackTrace();
            }
```

###读写sdcard数据

####写数据

在manifest文件中开放写权限。

```
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```
代码如下：

```
            try {
                File sdcard = Environment.getExternalStorageDirectory();//sdcard目录
                File myfile = new File(sdcard,"test.txt");//目标文件
                if(!sdcard.exists()){
                    //是否配置了sdcard。
                     Toast.makeText(getApplicationContext(),"当前设备没有sdcard",Toast.LENGTH_SHORT).show();
                     return;
                }else{
                    myfile.createNewFile();
                    .....

                }

            } catch (IOException e) {
                e.printStackTrace();
            }

```

###读数据

同上。









