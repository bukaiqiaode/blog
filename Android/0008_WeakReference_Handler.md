# Use WeakReference when work with Handler
## 基本思路
1. 将Handler定义在Activity的外部。

2. Handler的通过构造函数，持有Activity的弱引用。

```java
    private WeakReference<ExHandlerActivity> mActivity;
    public ExHandler1(ExHandlerActivity mActivity) {
        this.mActivity = new WeakReference<>(mActivity);
    }
```

3. Activity在自己的内部，初始化一个Handler的实例`handler1 = new ExHandler1(this);`，并使用Handler的实例发送消息。

```java
    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        if (event.getAction() == KeyEvent.ACTION_DOWN){
            switch (keyCode){
                case KeyEvent.KEYCODE_BACK:
                    handler1.sendEmptyMessage(ExHandler1.MSG_Back);
                    break;
            }
        }
        return super.onKeyDown(keyCode, event);
    }
```

4. Handler收到消息后，判断持有的Activity引用是否还存在。

5. 如果存在，则根据消息，反过来调用Activity的方法进行业务逻辑的处理。

```java
    @Override
    public void handleMessage(Message msg) {
        ExHandlerActivity theActivity = mActivity.get();
        if (theActivity == null){
            return;
        }
        switch (msg.what){
            case MSG_Back:
                theActivity.toastBack();
                break;
        }
        super.handleMessage(msg);
    }
```

## Activity的全部代码

> activity_ex_handler.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".activity.HomePageActivity"
    android:gravity="center">

    <TextView
        android:gravity="center"
        android:text="@string/app_name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

</LinearLayout>
```

> ExHandlerActivity.java

```java
package com.mydemo.demoapplication.activity;

import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.KeyEvent;
import android.widget.Toast;

import com.mydemo.demoapplication.R;
import com.mydemo.demoapplication.handlers.ExHandler1;

public class ExHandlerActivity extends AppCompatActivity {
    ExHandler1 handler1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_ex_handler);
        handler1 = new ExHandler1(this);
    }

    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        if (event.getAction() == KeyEvent.ACTION_DOWN){
            switch (keyCode){
                case KeyEvent.KEYCODE_BACK:
                    handler1.sendEmptyMessage(ExHandler1.MSG_Back);
                    break;
            }
        }
        return super.onKeyDown(keyCode, event);
    }

    public void toastBack(){
        Toast.makeText(getApplicationContext(), "Back", Toast.LENGTH_SHORT)
                .show();
    }
}

```


## ExHandler1的全部代码

> ExHandler1.java

```java
package com.mydemo.demoapplication.handlers;

import android.os.Handler;
import android.os.Message;

import com.mydemo.demoapplication.activity.ExHandlerActivity;

import java.lang.ref.WeakReference;

public class ExHandler1 extends Handler {
    public static final int MSG_Back = 1;

    private WeakReference<ExHandlerActivity> mActivity;

    public ExHandler1(ExHandlerActivity mActivity) {
        this.mActivity = new WeakReference<>(mActivity);
    }

    @Override
    public void handleMessage(Message msg) {
        ExHandlerActivity theActivity = mActivity.get();
        if (theActivity == null){
            return;
        }
        switch (msg.what){
            case MSG_Back:
                theActivity.toastBack();
                break;
        }
        super.handleMessage(msg);
    }
}
```

## References
- [This Handler class should be static or leaks might occur](https://www.cnblogs.com/jevan/p/3168828.html)

## Back to [index](./index.md)
