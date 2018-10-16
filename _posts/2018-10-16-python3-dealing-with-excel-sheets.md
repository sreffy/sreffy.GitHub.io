---
layout: post
title: "Python3 dealing with Excel Sheets"
category: Python
tags: [Python]
---
I have to deal with Excel data from different companies. These data have different header or even different Sheet Name,so they puzzle me a lot for aggregating.
I found some methods to handle this kind of situation with Python3. They are all very easy and simple algorithms but I just want to share them with you and 
make a note for myself. Hope it can helps you with Excel DATA!

# Search and aggregate
 
<head>
    <title>Rouge</title>
    <link media="all" rel="stylesheet" type="text/css" href="../assets/rouge/rouge.css" />
</head>

<body>
	{% highlight python %}
import os
import xlrd
import pandas as pd
	{% endhighlight %}
</body>

Search the sheet name contains keyword of our data. And move this sheet ONLY to a new folder to aggregate data. (If the Excel Sheet that you want don't have similar 
word with another, you have to rename it yourself).

For example, I have to Excel files contain different Sheets, and I want to search the data that contains calve data which means the Sheet Name of that data contains "calve".

<body>
	{% highlight python %}
path='F:\\kaggle\\kernels\\Excel aggregating'
excels=os.listdir(path) #see how many files in the folder

a = list(excels)
for i in range(len(excels)):
    a[i] = path + excels[i] #add each Excel Sheet in the list 'a'

newfloder='F:\\Python\\' # new folder to save data

	{% endhighlight %}
</body>

<body>
	{% highlight python %}
sheet_names=list(excels) # new list
for i in range(len(excels)):
    sheet_names[i]=xlrd.open_workbook(excels[i]).sheet_names()
    for j in range(len(sheet_names[i])):
        if 'calve' in sheet_names[i][j]:
            df=pd.read_excel(excels[i],sheet_name=sheet_names[i][j],encoding='utf8')
            df.to_csv(newfile+excels[i]+sheet_names[i][j]+'.csv',encoding="utf_8_sig",index=False)
	
	{% endhighlight %}
</body>

By doing this we can move the sheets we want to a new folder and save them as .csv.

# Merge all selected data

Next step, we have to deal with different headers. If you just want data that exisit in every files, we can define which header we want and aggregate all the data 
columns(if you want some header that do not exisit in many files, it will be empty columns in the aggregate data).

<body>
	{% highlight python %}
files=os.listdir(newfolder) #the csv data we transformed
SaveFile_Name='agg.csv'
SaveFile_path="F:\\Python\\agg"
	{% endhighlight %}
</body>

<body>
	{% highlight python %}
df1 = pd.read_csv(newfolder+ files[0])   
#read the first file and save it in new path
df1['name']=calvefiles[0] # define a column to indicate which file the data from
df2=df1.loc[:,["ID","parity","date","afc","name"]] #the header you want
df2.to_csv(SaveFile_path+'\\'+SaveFile_Name,encoding="utf_8_sig",index=False)

#iterations for files in the folder
try:
    for i in range(1,len(files)):
        df1 = pd.read_csv(newfolder+ files[i])
        df1['name']=calvefiles[1]
        df2=df1.loc[:,["ID","parity","date","afc","name"]]
        df2.to_csv(SaveFile_path+'\\'+SaveFile_Name,encoding="utf_8_sig",index=False, header=False, mode='a+')
except Exception as err:
    print(err)
    print(files[i])     #print the filename that has problem
	{% endhighlight %}
</body>

As you can see. The second step I used csv files. And it is the same structure if we use "read_excel" of pandas.

<body>
	{% highlight python %}
df1 = pd.read_excel(newfolder+ files[0],sheet_name="calve") #you can even choose the sheet  
#read the first file and save it in new path
df1['name']=calvefiles[0] # define a column to indicate which file the data from
df2=df1.loc[:,["ID","parity","date","afc","name"]] #the header you want
df2.to_csv(SaveFile_path+'\\'+SaveFile_Name,encoding="utf_8_sig",index=False)

#iterations for files in the folder
try:
    for i in range(1,len(files)):
        df1 = pd.read_excel(newfolder+ files[i],sheet_name="calve")
        df1['name']=calvefiles[1]
        df2=df1.loc[:,["ID","parity","date","afc","name"]]
        df2.to_csv(SaveFile_path+'\\'+SaveFile_Name,encoding="utf_8_sig",index=False, header=False, mode='a+')
except Exception as err:
    print(err)
    print(files[i])     #print the filename that has problem
	
	{% endhighlight %}
</body>

__This is my way to cope with annoying excel sheets!!__