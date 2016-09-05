##如何读取文件

要读取文件，首先要在配置文件加一个授权：

```
 <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```

然后，我们可以设置一个textView，然后将内容读到textView里面显示出来。代码如下：

```
            try {
                File file = new File("/data/test1.txt");
                FileInputStream fis = new FileInputStream(file);
                byte[] b = new byte[1024];
                fis.read(b);
                String result = new String(b);
                System.out.println("success:"+result);
                TextView tv = (TextView)findViewById(R.id.tv);
                tv.setText(result);

            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            }
```

##如何写入文件

同样，我们首先要在配置里面加一下权限：

```
  <!-- SDCard中创建与删除文件权限 -->
  <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS"/>
 <!-- 向SDCard写入数据权限 -->
 <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```
代码如下：

```
try {
        File file = new File("/data/test1.txt");
        FileOutputStream fos = new FileOutputStream(file);
        String info = "I am a student";
        fos.write(info.getBytes());
        fos.close();
        System.out.println("写入成功!");
} catch (Exception e) {
    e.printStackTrace();
}
```

##sdcard
这个的读写代码同上，测试尽量用实体机，虚拟机搞不定。

```
System.out.println(Environment.getExternalStorageState());//状态：mounted已加载
    if (Environment.getExternalStorageState().equals(Environment.MEDIA_MOUNTED)) {
       //检验sdcard是否被挂载
           String sdpath = Environment.getExternalStorageDirectory().getAbsolutePath();//获取绝对路径/mnt/sdcard
           System.out.println(sdpath);

      }

```



