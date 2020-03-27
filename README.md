# Downloading data from JGI
### Modified from code provided by Joe Vineis
Github link for jvineis: https://github.com/jvineis/get-jgi-data
Also see tutorial from JGI here: https://genome.jgi.doe.gov/portal/help/download.jsf#/api

_Last updated: By ANB, March 26, 2020_

:::warning
As of March 26, 2020, this code is still prelimiary and will be formalized once all data are downloaded
:::

## Step 1: Navigate to JGI and find your project
This is where the data live currently. The dataset I am working with currently is:
https://genome.jgi.doe.gov/portal/pages/dynamicOrganismDownload.jsf?organism=Comhiguestration

After logging in, you should be able to find your project (make sure it has the appropriate PI and project information) for each sample, including taxonomy, raw and filtered reads, and annotated functions. 

## Step 2: Navigate to the DOWNLOAD button on the top left of the web page. 
Click on the botton labeled "Open Downloads as XML" and save this file as a .txt file named _"all-jgi-dat.xml"_

## Step 3: Create a cookie so JGI can validate your credentials 
Use the following code and save as "make-a-cookie.shx" in the directory you will be downloading your files into. Make sure to replace the ENTER-USERNAME and ENTER-PASSWORD with your own personal info. Instructions on how to download using the API: https://genome.jgi.doe.gov/portal/help/download.jsf#/api

:::warning
Keep in mind that JGI may change these commands so make sure to check back into ensure the details have not changed!
:::

```
curl 'https://signon.jgi.doe.gov/signon/create' --data-urlencode 'login=ENTER-USERNAME' --data-urlencode 'password=ENTER-PASSWORD' -c cookies > /dev/null
```
This should produce a folder called "cookies." 

## Step 4: Setup the parameters you want to look for and download 
Explore the all-jgi-dat.xml to identify the files that you want to download and look for defining key words that indicate the files you want. For example. The COG functions for a metagenomic data set will usually contain the text "assembled.COG". This text can be used to generate a bash script that will generate a curl command to download each of the COG annotations for your assembled metagenomic data. Run the script like so, and be sure to inspect the resulting bash scritp to make sure that it will download only the files that you expect. 

You can read the help menu of the gen-curl-for-jgi.py command by typing python gen-curl-for-jgi.py -h. Make sure to call "python" before trying these commands.

```
python gen-curl-for-jgi.py -html all-jgi-dat.xml -o curl-command-for-cog-functions.shx -keyword assembled.COG
```

## Step 5: Run the curl command
Note: You can look at the first few lines of the code by using "head" command and running separately. This should be adding files to your folder. You can also copy/paste the link in the curl command and this should show you what is there/being downloaded. 

#### Pro-tip
JGI does not allow large downloads during certain hours of the day and your download may time-out. Try to initiate your large downloads late at night. 

You can also run using screen

```
screen INSERT_COMMAND_HERE
```
To start your run, while holding down "ctrl", type "a" then "d", then release "ctrl".Note, if you type "exit" it will kill your screen command. This should allow you to run in the background. See here for more information on using screen: http://www.kinnetica.com/2011/05/29/using-screen-on-mac-os-x/
