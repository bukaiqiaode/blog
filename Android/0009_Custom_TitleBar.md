# Define custom title-bar for Activity
## Steps

1. 在 `res\layout` 下，新建一个布局文件

> my_title_bar.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="50dp"
    android:background="#434343">

    <TextView
        android:id="@+id/tv_my_title_bar_title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"

        android:layout_alignParentStart="true"
        android:layout_centerVertical="true"
        android:layout_marginStart="5dp"

        android:text="微信(4)"
        android:textColor="#FFFFFF"
        android:textSize="20sp" />

    <ImageView
        android:id="@+id/iv_my_title_bar_add"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"

        android:layout_alignParentEnd="true"
        android:layout_centerVertical="true"
        android:layout_marginEnd="10dp"
        android:src="@android:drawable/ic_menu_add" />
</RelativeLayout>
```

2. 在 `controls` package下，新建一个MyTitleLayout的java类

> MyTitleLayout.java

```java
package com.mydemo.demoapplication.controls;

import android.content.Context;
import android.util.AttributeSet;
import android.view.LayoutInflater;
import android.widget.ImageView;
import android.widget.LinearLayout;
import android.widget.TextView;

import com.mydemo.demoapplication.R;

public class MyTitleLayout extends LinearLayout {
    private TextView title;
    private ImageView iv_add;

    public MyTitleLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
        //Assign this 'LinearLayout' to be parent of the 'Relative' layout
        LayoutInflater.from(context).inflate(R.layout.my_title_bar, this);

        title = findViewById(R.id.tv_my_title_bar_title);
        iv_add = findViewById(R.id.iv_my_title_bar_add);
    }
}
```

3. 在需要用到此标题栏的Activity中的布局文件中引入此标题栏即可

```xml
    <com.mydemo.demoapplication.controls.MyTitleLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
    </com.mydemo.demoapplication.controls.MyTitleLayout>
```

## References
- [Android自定义实现微信标题栏](https://www.cnblogs.com/cxyc/p/5377873.html)
- [Android 自定义控件 自定义标题栏](https://blog.csdn.net/plain_maple/article/details/52651171)

## Back to [index](./index.md)
