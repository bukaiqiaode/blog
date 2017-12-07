## Step 1
Install 'git' on Ubuntu using the command
```
  sudo apt-get install git
```

## Step 2
Open Jenkins portal(http://localhost:8081) and navigate to `[Manage Jenkins]--[Manage Plugins]`

## Step 3
Click on the 'Available' tab.
Input 'GitHub' as the filter, on the upper-right corner of the page.
In my version, there is one **GitHub Plugin**, whose description below it is `This plugin integrates GitHub to Jenkins'

## Step 4
Select to install the plug-in and wait it to be finished.

## Step 5
- Back to Jenkins portal(http://localhost:8081)
- Click on 'New Item'
- Input the any name and select 'Freestyle project'

## Step 6
- Find section 'Source Code Management' and select to use 'Git'
- Specify your 'https://github.com/xxxx/xxxx.git' for the 'Repository URL'
- Input the credential using your username/password for GitHub

## Step 7
- Find section 'Build Triggers'
- Select 'Build periodically' and set the value to be `H/6 * * * *`, which means to do the build every 6 minutes

## Step 8
- Find section 'Build'
- Click on 'Add build step' and select 'Execute shell'
- Input some easy command like `ls -l`

## Step 9
Click on 'Apply'

## Step 10
Done. <br/>
Now back to the portal of the newly created item, and wait for it to be build~

## Back to [index](./index.md)
