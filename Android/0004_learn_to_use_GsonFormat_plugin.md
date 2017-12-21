# Steps to install & use GsonFormat plug-in in Android Studio.

## Step 1, install GsonFormat plug-in

In Android Studio, select [File] --> [Settings] to launch the 'Settings' dialog.

Select 'Plugins' and click 'Browse Repositories'

In the 'Browse Repositories' dialog, search for 'GsonFormat' and select to install it.

After installed, re-start the Android-Studio to enable the plug-in.

## Step 2, create one Bean using GsonFormat plug-in

Create one new JAVA class, put focus inside the class body.

Select [Code] --> [Generate...]

Select 'GsonFormat' from the pop-up

Input the JSon string into the 'GsonFormat' dialog. You can click the 'Format' button to format the string input. Then click 'OK'.
```Javascript
    {
        "name": "wang 5",
        "gender": "man",
        "age": 15,
        "height": "140cm",
        "addr": {
            "province": "fujian",
            "city": "quanzhou",
            "code": "300000"
        },
        "hobby": [
            {
                "name": "billiards",
                "code": "1"
            },
            {
                "name": "computeGame",
                "code": "2"
            }
        ]
    }
```
Review the information in the 'Virgo Model' dialog and click 'OK'

## Step 3, review the code generated


## Back to [index](./index.md)
