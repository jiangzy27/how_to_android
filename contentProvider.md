##什么是contentProvider？
就是应用程序之间共享数据的工具。
我们以获取联系人列表为例，联系人属于系统自带的一个应用程序。

>添加读取联系人列表的权限

```
<manifest>
    <application>
        .....
    </application>
    <uses-permission android:name="android.permission.READ_CONTACTS" />
</manifest>
```

注意这个位置跟之前不一样了，放在application里面无效。

>MainActivity.java

```
        ....
        public void onClick(View view) {
            //查询联系人,不指定查询条件，很多行，返回的是cursor指针，指向第一行。
            Cursor c = getContentResolver().query(ContactsContract.Contacts.CONTENT_URI,null,null,null,null);
            while(c.moveToNext()){
                //打印联系人名称
                System.out.println(c.getString(c.getColumnIndex(ContactsContract.Contacts.DISPLAY_NAME)));
            }


        }
```