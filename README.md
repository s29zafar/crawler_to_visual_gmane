# crawler_to_visual_gmane
#### Analyzing an EMAIL Archive from gmane and vizualizing the data using the D3 JavaScript library.

### This is a set of tools that allow you to pull down an archive of a gmane repository using the instructions at:

http://gmane.org/export.php

In order not to overwhelm the gmane.org server, I have used a copy of the messages present at: 

http://mbox.dr-chuck.net/

This server will be faster and take a lot of load off the gmane.org server.

## Implementation:

#### The first step is to spider the gmane repository, and obtain the data.
(note:  It may take many hours to pull all the datadown.  So you may need to restart several times.)

#### The second process is running the program gmodel.py. 

#### There is a simple vizualization of the word frequence in the subject lines in the file gword.py.

#### A second visualization is in gline.py.  It visualizes email participation by organizations over time.

# Visualization: first for gword.py and then for gline.py

![Word_cloud](https://user-images.githubusercontent.com/69566994/149310887-55d9019d-366e-4120-9d0e-f650af3c3536.png)

<img width="1066" alt="Screen Shot 2021-10-22 at 4 06 18 PM" src="https://user-images.githubusercontent.com/69566994/149311013-0cd180ac-9bc4-42c7-9dad-5ed0ab288d29.png">


## Please check the poster file in the Repository for more information

### Notes:

##### No future mass commits to main are allowed

##### The program scans content.sqlite from 1 up to the first message number not already spidered and starts spidering at that message.

##### Sometimes gmane.org is missing a message.  Perhaps administrators can delete messages or perhaps they get lost - I don't know. 

##### Each time gmodel.py runs - it completely wipes out and re-builds index.sqlite, allowing you to adjust its parameters and edit the mapping tables in 
##### content.sqlite to tweak the data cleaning process.
