# Configure VS-Code for PHP development
## Enable remote debug in PHP settings
Add switch to enable remote debug in `php.ini`
```
xdebug.remote_enable = on 
xdebug.remote_autostart = on
```

## Install extension `PHP Debug` for VS-Code
## Install extension `PHP IntelliSense` for VS-Code
## Configure VS-Code to let it know location of PHP binary
- Open `File --> Preferences --> Settings`
- Search for `php.validate.executablePath` and set its value to be path-of-php-binary. Example: `"php.validate.executablePath": "E:\\Downloads\\tools\\php71\\php.exe"`
## Done

## References
- [使用vs code写php及调试](https://www.cnblogs.com/ashidamana/p/5459188.html)

## Back to [index](./index.md)