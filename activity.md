##什么是Activity？

可以理解成界面。

###状态：

有三种状态：运行状态，停止状态，暂停状态。

###生命周期

1.onCreate() <br />
2.onStart() <br />
3.onResume() <br />
4.activity running....<br />
5.onPause() <br />
6.onStop() <br />
7.onDestory()<br />

测试代码：<br />

```
package com.example.myapplication;
import android.app.Activity;
import android.os.Bundle;

public class MainActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        //建立activity，新建进程。
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        System.out.println("onCreate");
    }
    @Override
    protected  void onStart(){
        //正式进入activity前，先resume做的动作。
        super.onStart();
        System.out.println("onStart");
    }
    @Override
    protected  void onResume(){
        //正式进入activity前，必须要进行的动作。
        super.onResume();
        System.out.println("onResume");
    }
    @Override
    protected  void onPause(){
        super.onPause();
        System.out.println("onPause");
    }
    @Override
    protected  void onStop(){
        //activity停止后，只要不销毁，还可以restart回去，不必重新create进程。
        //对应的操作：点击中间的菜单键。
        super.onStop();
        System.out.println("onStop");
    }
    @Override
    protected  void onDestroy(){
        //activity被销毁后，进程被杀死，如果再进去，就只能create重建了。
        //对应的操作：点击回退键，回退到菜单。
        super.onDestroy();
        System.out.println("onDestroy");
    }
    @Override
    protected  void onRestart(){
        super.onRestart();
        System.out.println("onRestart");
    }
}


```

###切换Activity
我们新建一个activity1.java文件，也仿照MainActivity.java写一个xml布局文件丢在layout文件夹里面，
然后绑定它，我们在MainActivity.java里面加一个按钮，点击这个按钮，跳转到activity1去。

```
    protected void onCreate(Bundle savedInstanceState) {
        //建立activity，新建进程。
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);//绑定视图
       //在布局文件声明一个按钮，然后再此用findViewById找到它。
        Button btn =(Button)findViewById(R.id.btn);//findViewById返回的是view类型,button是view的子类。
        //绑定一个事件。
        btn.setOnClickListener(new aa());



    }
    class aa implements View.OnClickListener{

        public void   onClick(View vv){
            Intent i = new Intent(MainActivity.this,activity1.class);
            startActivity(i);
        }

    }
```
新建完成之后呢，要注意在manifest配置文件中添加上这个新的activity1。

```
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name"
            android:theme="@style/AppTheme.NoActionBar">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity android:name=".activity1">

        </activity>
```

###关闭Activity
如何关闭一个Activity呢？把Activity1.java的代码贴上来.

```
public class activity1  extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.act1);//绑定静态布局文件
        Button btnClose = (Button) findViewById(R.id.btnclose);
        btnClose.setOnClickListener(new cc());
    }
    class cc implements View.OnClickListener{
        //关闭activity1
        public void   onClick(View vv){
            finish();//调用此方法关闭当前Activity
        }

    }
}
```

###传递字符串数据

MainActivity.java:

```
//intent是打算，意图做某事的意思。参数是意图要去的对象实例，意图的类。
Intent i = new Intent(MainActivity.this,activity1.class);
i.putExtra("myName","张三");
startActivity(i);
```
activity1.java：

```
public class activity1  extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.act1);//绑定静态文件
        Button btn = (Button) findViewById(R.id.btn);
        btn.setOnClickListener(new cc());


    }
    class cc implements View.OnClickListener{
        //接受上一个Activity传递的参数，并展示出来。
        public void   onClick(View vv){
            //文本信息
            TextView txt = (TextView) findViewById(R.id.yourname);
            //取得上个Activity传递的字符串数据
            String str = getIntent().getStringExtra("myName");
            txt.setText(str);
        }

    }
}
```

###传递复杂数据

MainActivity.java:

```
Intent i = new Intent(MainActivity.this,activity1.class);
//bundle比较强大，不仅可以传递基本类型，对象也可以传递，不过需要序列化才行。
Bundle data = new Bundle();
data.putString("name","xiaoming");
data.putInt("id",11111);
i.putExtras(data);
startActivity(i);

```

activity1.java：

```
TextView txt = (TextView) findViewById(R.id.yourname);
//取得上个Activity传递的字符串数据
Bundle getdata = getIntent().getExtras();
String str = getdata.getString("name");
Integer id = getdata.getInt("id");
txt.setText(id+"==="+str);
```

###接收子视图的数据：
MainActivity.java：
```
    class aa implements View.OnClickListener{

        public void   onClick(View vv){
            Intent i = new Intent(MainActivity.this,activity1.class);
            //不能用startActivity了。与下面的监听函数配套使用
            startActivityForResult(i,0);
        }

    }
    //监听发送和接受两种状态
    protected void onActivityResult(int requestCode,int resultCode,Intent data){

        super.onActivityResult(requestCode,resultCode,data);
        if(resultCode == 200){
            String res = data.getStringExtra("res");
            TextView txt2 = (TextView) findViewById(R.id.txt2);
            txt2.setText(res);
        }
    }
```
activity1.java

```
    class cc implements View.OnClickListener{
        //接受上一个Activity传递的参数，并展示出来。
        public void   onClick(View vv){
            Intent i = new Intent();
            i.putExtra("res","i am from child view");//添加一个参数
            setResult(200,i);//返回到上一个视图的信息
            finish();//关闭视图
        }

    }
```




