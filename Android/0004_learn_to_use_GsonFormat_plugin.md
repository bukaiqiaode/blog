# Steps to install & use GsonFormat plug-in in Android Studio.

## Step 1, install GsonFormat plug-in

In Android Studio, select [File] --> [Settings] to launch the 'Settings' dialog.

![Image](../images/0004_01.png)

Select 'Plugins' and click 'Browse Repositories'

![Image](../images/0004_02.png)

In the 'Browse Repositories' dialog, search for 'GsonFormat' and select to install it.

![Image](../images/0004_03.png)
![Image](../images/0004_04.png)

After installed, re-start the Android-Studio to enable the plug-in.

![Image](../images/0004_05.png)

## Step 2, create one Bean using GsonFormat plug-in

Create one new JAVA class, put focus inside the class body. Select [Code] --> [Generate...]

![Image](../images/0004_06.png)

Select 'GsonFormat' from the pop-up

![Image](../images/0004_07.png)

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
![Image](../images/0004_08.png)

Review the information in the 'Virgo Model' dialog and click 'OK'

![Image](../images/0004_09.png)

## Step 3, review the code generated
Below is the code generated by the plug-in.
```JAVA
        public class mybean  {

            /**
             * name : wang 5
             * gender : man
             * age : 15
             * height : 140cm
             * addr : {"province":"fujian","city":"quanzhou","code":"300000"}
             * hobby : [{"name":"billiards","code":"1"},{"name":"computeGame","code":"2"}]
             */

            private String name;
            private String gender;
            private int age;
            private String height;
            private AddrBean addr;
            private List<HobbyBean> hobby;

            public String getName() {
                return name;
            }

            public void setName(String name) {
                this.name = name;
            }

            public String getGender() {
                return gender;
            }

            public void setGender(String gender) {
                this.gender = gender;
            }

            public int getAge() {
                return age;
            }

            public void setAge(int age) {
                this.age = age;
            }

            public String getHeight() {
                return height;
            }

            public void setHeight(String height) {
                this.height = height;
            }

            public AddrBean getAddr() {
                return addr;
            }

            public void setAddr(AddrBean addr) {
                this.addr = addr;
            }

            public List<HobbyBean> getHobby() {
                return hobby;
            }

            public void setHobby(List<HobbyBean> hobby) {
                this.hobby = hobby;
            }

            public static class AddrBean {
                /**
                 * province : fujian
                 * city : quanzhou
                 * code : 300000
                 */

                private String province;
                private String city;
                private String code;

                public String getProvince() {
                    return province;
                }

                public void setProvince(String province) {
                    this.province = province;
                }

                public String getCity() {
                    return city;
                }

                public void setCity(String city) {
                    this.city = city;
                }

                public String getCode() {
                    return code;
                }

                public void setCode(String code) {
                    this.code = code;
                }
            }

            public static class HobbyBean {
                /**
                 * name : billiards
                 * code : 1
                 */

                private String name;
                private String code;

                public String getName() {
                    return name;
                }

                public void setName(String name) {
                    this.name = name;
                }

                public String getCode() {
                    return code;
                }

                public void setCode(String code) {
                    this.code = code;
                }
            }
        }
```

## References:
- http://blog.csdn.net/dakaring/article/details/46300963

## Back to [index](./index.md)
