# There are 3 ways to handle click event for a Button
- Use anonymous class
- Use a new class that implements OnClickListener interface
- Use onClick property in the xml

## Use anonymous class
```
    btn1.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            Toast.makeText(getApplicationContext(), "hello", Toast.LENGTH_LONG).show();
        }
    });
```


## Use a new class that implements OnClickListener interface
### The new class to create
```
    btn1.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            Toast.makeText(getApplicationContext(), "hello", Toast.LENGTH_LONG).show();
        }
    });
```

### In the main activity
```
    btn1.setOnClickListener(new ButtonClickListener(this));
```

## Use onClick property in the xml
### The xml
```
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button"
        android:id="@+id/button1"
        android:onClick="btnClick"/>
```

### In the main activity
```
    public void btnClick(View view) {
        Toast.makeText(this, "From xml", Toast.LENGTH_LONG).show();
    }
```

Reference
- https://www.cnblogs.com/liangshijie/p/6073582.html

# Back to [index](./index.md)
