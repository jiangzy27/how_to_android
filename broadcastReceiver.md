##什么是Broadcast Receiver？

就是广播监听器的意思，是一种组件间通信的工具，可以监听接收操作系统的某个状态，比如短信来电、操作系统启动完毕、耗电量等。
但是效率比较低，造成程序运行慢，频繁的场合慎用。
我们先看一下如何用Broadcast Receiver传递简单的数据：

>先要创建一个类bc.java

```
import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;

/**
 * Created by 170157 on 2016/9/9.
 */
public class bc extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        System.out.println("onReceive data:"+intent.getStringExtra("name"));
    }
}

```

>然后我们在manifest.xml文件中注册一下

```
<application>
......
    <receiver android:name=".bc"></receiver>
</application>
```

>MainActivity中调用

```
        public void onClick(View view) {
            Intent i = new Intent(MainActivity.this, bc.class);
            i.putExtra("name","hello");
            sendBroadcast(i);


        }
```

只传递了一个name的字符串值，看是否可以接收到。

##动态注册取消广播监听器

我们要做三个按钮：发送广播，注册广播，删除广播。

```
package com.example.myapplication;
import android.app.Activity;
import android.content.ComponentName;
import android.content.Context;
import android.content.Intent;
import android.content.IntentFilter;
import android.content.ServiceConnection;
import android.graphics.Bitmap;
import android.os.AsyncTask;
import android.os.Bundle;
import android.os.Environment;
import android.os.IBinder;

import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.FileReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.io.OutputStreamWriter;


public class MainActivity extends Activity{

    private final bc mybc = new bc();
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        //建立activity，新建进程。
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);//绑定视图
        Button btn =(Button)findViewById(R.id.button);//findViewById返回的是view类型,button是view的子类。
        Button btn2 =(Button)findViewById(R.id.button2);
        Button btn3 =(Button)findViewById(R.id.button3);
        btn.setOnClickListener(new SendBroadcast());
        btn2.setOnClickListener(new Registerroadcast());
        btn3.setOnClickListener(new UnregisterSendBroadcast());
    }
    class SendBroadcast implements View.OnClickListener{
        @Override
        public void onClick(View view) {
            //Intent i = new Intent(MainActivity.this, bc.class);
            //另外一种声明方式
            Intent i = new Intent(bc.ACTION);
            i.putExtra("name","hello");
            sendBroadcast(i);


        }
    }
    class Registerroadcast implements View.OnClickListener{
        @Override
        public void onClick(View view) {
            //需要指定action，在bc类中加。
            registerReceiver(mybc,new IntentFilter(bc.ACTION));


        }
    }
    class UnregisterSendBroadcast implements View.OnClickListener{
        @Override
        public void onClick(View view) {
            unregisterReceiver(mybc);


        }
    }

}


```
然后是receiver类

```
package com.example.myapplication;

import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;

/**
 * Created by 170157 on 2016/9/9.
 */
public class bc extends BroadcastReceiver {
    //规则：包名+自定义名称abc
    public  static final String ACTION = "com.example.myapplication.intent.action.abc";
    @Override
    public void onReceive(Context context, Intent intent) {
        System.out.println("onReceive data:"+intent.getStringExtra("name"));
    }
}

```

只有在注册了receiver的基础上，点击发送才有效果，否则没有。

