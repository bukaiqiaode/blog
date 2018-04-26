## Step 1 Install and run
> _Estimation: 20 minutes_

Download the war file and run it following documentation: https://jenkins.io/doc/book/installing/#war-file
Save the war file into a single folder.
Follow the document and run the war file from command-line, like `java -jar jenkins.war --httpPort=9090`
Access `localhost:9090` from browser and unlock Jenkins, following the documentation.

## Step 2 Install suggested plugins
> _Estimation: 20 minutes_

In the **Customize Jenkins** page, select 'Install suggested plugins' and wait for the process to be finished. Just ignore the failed-to-install ones.

## Step 3 Create administrator user
> _Estimation: 5 minutes_

Following the [guide](https://jenkins.io/doc/book/installing/#creating-the-first-administrator-user) to create the administrator user.

## Step 4 Create a free-style task
> _Estimation: 10 minutes_

- Open Jenkins portal(`http://localhost:9090`) and select 'New Item'.
- Input a name for the item.
- Select **Freestyle project** and click 'OK'.
- Find section **Build Triggers** and select **Build periodically**. Set value `H/6 * * * *` to build every 6-minutes.
- Find section **Builds** and select **Execute Windows batch command**. Input `dir` as the shell to be used.
- Click 'Apply' and then 'Save'.
- Wait for several minutes, then check that a build is scheduled and executed successfully.

## Step 5 Useful DOS commands to remove read-only attributes and to copy files & folders
```shell
pushd c:\
attrib -r /s /d c:\yyyyy\*

xcopy /S /Y C:\xxxxx\stbModel c:\yyyyy\stbModel\

pushd c:\yyyyy

python runner.py
```

## Back to [index](./index.md)
