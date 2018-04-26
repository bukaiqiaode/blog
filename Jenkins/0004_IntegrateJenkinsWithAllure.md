Documentation at https://docs.qameta.io/allure/#_reporting
## Step 1 Install Allure plugin
Open Jenkins portal(http://localhost:9090), then go to [Manage Jenkins] - [Manage Plugins]
Click the **Available** tab, search for **Allure**
Select it and select to 'Install without restart'

## Step 2 Install Allure-Commandline
Open Jenkins portal(http://localhost:9090), then go to [Manage Jenkins] - [Global Tool Configuration].
Focus on **Allure Commandline**, and click on 'Add Allure Commandline'.
Input a name, and keep the defaults.
Click on 'Apply', and then 'Save'.

## Step 3 Configure the build item to generate Allure report after build
Back to Jenkins portal, click on the build item, and select 'Configure' to configure the build.
Focus on **Post-build Actions** and click on **Add post-build action**, select 'Allure Report'.
Specify the path to Allure result.
Click 'Apply' and then 'Save'.

## Step 4 Practice
Run the build again, and Allure report will be generated.

## Back to [index](./index.md)
