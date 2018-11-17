# Learn to use AsyncTask
## 基本思路
1. App启动的时候，启动HomePageActivity。
2. HomePageActivity直接启动ExImageActivity。
```java
    startActivity(new Intent(HomePageActivity.this, ExImageActivity.class));
```
3. ExImageActivity启动的时候，直接创建一个异步任务，将内建的图片地址作为参数传入，并执行此异步任务。
```java
        String url = "http://himg2.huanqiu.com/attachment2010/2018/1117/20181117082418674.png";
        new MyAsyncTask1().execute(url);
```
4. 在异步任务的onPreExecute()方法中，将进度条显示出来。
```java
        @Override
        protected void onPreExecute() {
            super.onPreExecute();
            progressBar.setVisibility(View.VISIBLE);
        }
```
5. 在异步任务的doInBackground()方法中，使用传进来的图片地址，读取并下载图片。在这个过程中，注意异常的捕获和处理。
```java
        @Override
        protected Bitmap doInBackground(String... strings) {
            String url = strings[0];
            Bitmap bitmap = null;
            URLConnection connection;
            InputStream is = null;
            BufferedInputStream bufferedInputStream = null;

            try {
                connection = new URL(url).openConnection();
                is = connection.getInputStream();
                bufferedInputStream = new BufferedInputStream(is);
                bitmap = BitmapFactory.decodeStream(bufferedInputStream);

                Thread.sleep(3000);
            } catch (InterruptedException e) {
            } catch (MalformedURLException e) {
            } catch (IOException e) {
            }
            finally {
                if (is != null){
                    try {
                        is.close();
                    } catch (IOException e) {
                    }
                }

                if (bufferedInputStream != null) {
                    try {
                        bufferedInputStream.close();
                    } catch (IOException e) {
                    }
                }
            }
            return bitmap;
        }
```
6. 在异步任务的onPostExecute()方法中，将进度条隐藏。如果成功读取了网络上的图片，则将图片显示出来。
```java
        @Override
        protected void onPostExecute(Bitmap bitmap) {
            super.onPostExecute(bitmap);
            progressBar.setVisibility(View.INVISIBLE);

            if (bitmap != null){
                imageView.setImageBitmap(bitmap);
                imageView.setVisibility(View.VISIBLE);
            }
        }
```

## HomeActivity的全部代码
> activity_home_page.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".activity.HomePageActivity"
    android:gravity="center">

    <Button
        android:id="@+id/btn_loadImage"
        android:text="加载图片"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

</LinearLayout>
```

> HomePageActivity.java
```java
package com.mydemo.demoapplication.activity;

import android.content.Intent;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.Button;

import com.mydemo.demoapplication.R;

public class HomePageActivity extends AppCompatActivity {
    private Button button;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        setContentView(R.layout.activity_home_page);

        button = findViewById(R.id.btn_loadImage);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                startActivity(new Intent(HomePageActivity.this, ExImageActivity.class));
            }
        });
    }
}
```


## ExImageActivity的全部代码
> activity_ex_image.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="vertical"
    android:gravity="center"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".activity.ExImageActivity">

    <ProgressBar
        style="?android:attr/progressBarStyleLarge"
        android:id="@+id/ex_progress"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="8dp"
        android:visibility="gone" />

    <ImageView
        android:id="@+id/ex_imageView"
        android:visibility="gone"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:contentDescription="Test Image" />
</LinearLayout>
```

> ExImageActivity.java
```java
package com.mydemo.demoapplication.activity;

import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.os.AsyncTask;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.ImageView;
import android.widget.ProgressBar;

import com.mydemo.demoapplication.R;

import java.io.BufferedInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.MalformedURLException;
import java.net.URL;
import java.net.URLConnection;

public class ExImageActivity extends AppCompatActivity {
    private ImageView imageView;
    private ProgressBar progressBar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_ex_image);

        imageView = findViewById(R.id.ex_imageView);
        progressBar = findViewById(R.id.ex_progress);

        String url = "http://himg2.huanqiu.com/attachment2010/2018/1117/20181117082418674.png";
        new MyAsyncTask1().execute(url);
    }

    class MyAsyncTask1 extends AsyncTask<String, Void, Bitmap>{
        @Override
        protected void onPreExecute() {
            super.onPreExecute();
            progressBar.setVisibility(View.VISIBLE);
        }

        @Override
        protected void onPostExecute(Bitmap bitmap) {
            super.onPostExecute(bitmap);
            progressBar.setVisibility(View.INVISIBLE);

            if (bitmap != null){
                imageView.setImageBitmap(bitmap);
                imageView.setVisibility(View.VISIBLE);
            }
        }

        @Override
        protected Bitmap doInBackground(String... strings) {
            String url = strings[0];
            Bitmap bitmap = null;
            URLConnection connection;
            InputStream is = null;
            BufferedInputStream bufferedInputStream = null;

            try {
                connection = new URL(url).openConnection();
                is = connection.getInputStream();
                bufferedInputStream = new BufferedInputStream(is);
                bitmap = BitmapFactory.decodeStream(bufferedInputStream);

                Thread.sleep(3000);
            } catch (InterruptedException e) {
            } catch (MalformedURLException e) {
            } catch (IOException e) {
            }
            finally {
                if (is != null){
                    try {
                        is.close();
                    } catch (IOException e) {
                    }
                }

                if (bufferedInputStream != null) {
                    try {
                        bufferedInputStream.close();
                    } catch (IOException e) {
                    }
                }
            }
            return bitmap;
        }
    }
}
```

## 若干收获
1. `android:gravity="center"`定义的是某个元素其【内部】的控件的【重心】在哪里。
2. ImageView的`android:contentDescription`属性，是为视力障碍的人士准备的，需要提前开启相关的功能。在开启相关功能，并设置这个属性后，用户点击这个控件的时候，android系统会自动使用人声朗读为此属性设置的内容。


## References
- [Android必学之AsyncTask](https://www.cnblogs.com/caobotao/p/5020857.html)

## Back to [index](./index.md)
