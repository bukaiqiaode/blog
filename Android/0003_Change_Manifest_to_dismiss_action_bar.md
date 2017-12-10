# Change the Manifest.xml to dismiss the Action-Bar

## Step 1
Create a new project, select to use 'Empty Activity'.

## Step 2
Delete the system-generated 'TextView', from 'activity_main.xml'
    ![Image](../images/dismiss_actionbar001.png)

## Step 3
Change the layout to be 'Framelayout'
    ![Image](../images/dismiss_actionbar002.png)

## Step 4
Delete the un-used name-space declaration.
    ![Image](../images/dismiss_actionbar003.png)

## Step 5
Click 'preview' to see what it looks like now.
    ![Image](../images/dismiss_actionbar004.png)
    ![Image](../images/dismiss_actionbar005.png)

## Step 6
Open AndroidManifest.xml
    ![Image](../images/dismiss_actionbar006.png)
    
## Step 7
Change the property of the MainActivity to dismiss the action-bar, by adding 'theme'.
    ![Image](../images/dismiss_actionbar007.png)

## Step 8
Launch the emulator and check that there is no action-bar now.
    ![Image](../images/dismiss_actionbar008.png)

## Step 9
Remove the 'theme' property in AndroidManifest.xml and run again. The emulator will show the action-bar.
    ![Image](../images/dismiss_actionbar009.png)

## Back to [index](./index.md)
