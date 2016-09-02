##什么是Activity？

可以理解成界面。

###状态：

有三种状态：运行状态，停止状态，暂停状态。

###生命周期

1.onCreate() <br />
2.onStart() <br />
3.onResume() <br />
4.activity running....
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

###转场
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
