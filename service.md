##什么是service？

有时，我们不需要界面呈现，只是后台运行一些东西，比如qq的实时监听是否有好友发信息给你，
有时我们即使关闭了qq，也是可以接收到好友发的信息的，起码接收到消息提醒，这就是service。
<br />
添加service的方法跟Activity差不多。我们先在工程新建一个myservice.java文件。

```
import android.app.Service;
import android.content.Intent;
import android.os.IBinder;

public class myservice extends Service {
    public IBinder onBind(Intent i){
        return null;
    }
//两个生命周期函数：启动和销毁
    @Override
    public void onCreate() {
        super.onCreate();
        System.out.println("service onCreate");
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        System.out.println("service onDestroy");
    }
}

```
然后打开manifest配置文件，添加上service。

```
        <activity android:name=".activity1">

        </activity>
        <service android:name=".myservice">

        </service>
```
然后，我们在MainActivity.java文件里面设计两个按钮，分别是启动和销毁service。

```

import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;


public class MainActivity extends Activity {
    private Intent i;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button startBtn = (Button) findViewById(R.id.button);
        Button stopBtn = (Button) findViewById(R.id.button2);

        i = new Intent(this,myservice.class);//这里是关键，意图打开service
        //两个监听
        startBtn.setOnClickListener(new startBtnListener());
        stopBtn.setOnClickListener(new stopBtnListener());

    }
    class startBtnListener implements View.OnClickListener{

        public void   onClick(View vv){
            startService(i);//开启service
        }

    }
    class stopBtnListener implements View.OnClickListener{

        public void   onClick(View vv){
            stopService(i);//关闭service
        }

    }


}


```
##两种启动service的方式

前面startService的方式启动服务，我们销毁了Activity是无法停止服务的。而另外一种bundle的方式启动就可以。
MainActivity.java:

```
import android.app.Activity;
import android.content.ComponentName;
import android.content.Context;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.Bundle;
import android.os.IBinder;
import android.view.View;
import android.widget.Button;

//继承更多的类
public class MainActivity extends Activity implements View.OnClickListener, ServiceConnection{
    private Intent i;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button startBtn = (Button) findViewById(R.id.button);
        Button stopBtn = (Button) findViewById(R.id.button2);
        Button bindBtn = (Button) findViewById(R.id.button3);
        Button unbindBtn = (Button) findViewById(R.id.button4);
        i = new Intent(this,myservice.class);//意图打开service
        startBtn.setOnClickListener(this);
        stopBtn.setOnClickListener(this);
        bindBtn.setOnClickListener(this);
        unbindBtn.setOnClickListener(this);

    }

    @Override
    public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
        //服务成功后触发
        System.out.println("onServiceConnected");
    }

    @Override
    public void onServiceDisconnected(ComponentName componentName) {
    //service崩溃时触发
        System.out.println("onServiceDisconnected");
    }

    @Override
    public void onClick(View v) {
        switch(v.getId()){
            case R.id.button:
                startService(i);
                break;
            case R.id.button2:
                stopService(i);
                break;
            case R.id.button3:
                bindService(i,this, Context.BIND_AUTO_CREATE);
                break;
            case R.id.button4:
                unbindService(this);
                break;
        }
    }


}


```

myservce.java:

```
import android.app.Service;
import android.content.Intent;
import android.os.Binder;
import android.os.IBinder;

public class myservice extends Service {
    private final myserviceBinder mb = new myserviceBinder();
    public IBinder onBind(Intent i){
        System.out.println("onBind");
        return mb;//这里如果返回是空，前面MainActivity就无法调用到onServiceConnected函数了。
    }

    public class myserviceBinder extends Binder {

    }

    @Override
    public void onCreate() {
        super.onCreate();
        System.out.println("service onCreate");
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        System.out.println("service onDestroy");
    }
}

```
而且，startService这种方式启动服务，是无法跟服务进行通信的，只有与服务进行绑定，才可以进行通信。
<br />

##在Activity中跟service通信

MainActivity.java:

```
import android.app.Activity;
import android.content.ComponentName;
import android.content.Context;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.Bundle;
import android.os.IBinder;
import android.view.View;
import android.widget.Button;


public class MainActivity extends Activity implements View.OnClickListener, ServiceConnection{
    private Intent i;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button startBtn = (Button) findViewById(R.id.button);
        Button stopBtn = (Button) findViewById(R.id.button2);
        Button bindBtn = (Button) findViewById(R.id.button3);
        Button unbindBtn = (Button) findViewById(R.id.button4);
        i = new Intent(this,myservice.class);//意图打开service
        startBtn.setOnClickListener(this);
        stopBtn.setOnClickListener(this);
        bindBtn.setOnClickListener(this);
        unbindBtn.setOnClickListener(this);

    }

    @Override
    public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
        //服务成功后触发

        System.out.println("onServiceConnected");
        //最后一个参数是获取的servce实例。
        //获取service实例
        myservice serv = ((myservice.myserviceBinder)iBinder).getMyService();
        System.out.println("从服务中获取的数字："+serv.getNum());//myservice里面的方法哦！
    }

    @Override
    public void onServiceDisconnected(ComponentName componentName) {
    //service崩溃时触发
        System.out.println("onServiceDisconnected");
    }

    @Override
    public void onClick(View v) {
        switch(v.getId()){
            case R.id.button:
                startService(i);
                break;
            case R.id.button2:
                stopService(i);
                break;
            case R.id.button3:
                bindService(i,this, Context.BIND_AUTO_CREATE);
                break;
            case R.id.button4:
                unbindService(this);
                break;
        }
    }


}


```

myservce.java:

```
import android.app.Service;
import android.content.Intent;
import android.os.Binder;
import android.os.IBinder;

import java.util.TimerTask;

public class myservice extends Service {
    private final myserviceBinder mb = new myserviceBinder();
    public IBinder onBind(Intent i){
        System.out.println("onBind");
        return mb;//这里如果返回是空，前面MainActivity就无法调用到onServiceConnected函数了。
    }
    //可以传递到前面Activity里面的对象。
    public class myserviceBinder extends Binder {
        public myservice getMyService(){
            return myservice.this;//获取服务的实例方法
        }

    }
    //返回一个数字
    public int getNum(){
        return 100;
    }
    @Override
    public void onCreate() {
        super.onCreate();
        System.out.println("service onCreate");
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        System.out.println("service onDestroy");
    }

}

```


