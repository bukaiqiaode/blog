## Step 1 Import used libraries
```python
    # -*- coding: UTF-8 -*-
    import os
    import subprocess
    import time
```

## Step 2 Generate a time-stamp
```python
    t_timestamp = str(int(time.time() * 100))
```

## Step 3 Create a folder, use the time-stamp
```python
    folderName = os.path.join(r"Log\xx_backups\\", t_timestamp)
    os.mkdir(folderName)
```

## Step 4 Copy some files into the backup folder
```python
    str_command = r"xcopy /s /y allure-results {0}".format(folderName)
    process = subprocess.Popen(str_command.strip(), shell=True)
    process.wait()
```

## Back to [index](./index.md)
