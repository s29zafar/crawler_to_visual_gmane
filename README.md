# crawler_to_visual_gmane
Analyzing an EMAIL Archive from gmane and vizualizing the data
using the D3 JavaScript library.

This is a set of tools that allow you to pull down an archive
of a gmane repository using the instructions at:

http://gmane.org/export.php

In order not to overwhelm the gmane.org server, I have used a copy of the messages present at: 

http://mbox.dr-chuck.net/

This server will be faster and take a lot of load off the 
gmane.org server.

You should install the SQLite browser to view and modify the databases from:

http://sqlitebrowser.org/

The first step is to spider the gmane repository.  The base URL 
is hard-coded in the gmane.py and is hard-coded to the Sakai
developer list.  You can spider another repository by changing that
base url.   Make sure to delete the content.sqlite file if you 
switch the base url.  The gmane.py file operates as a spider in 
that it runs slowly and retrieves one mail message per second so 
as to avoid getting throttled by gmane.org.   It stores all of
its data in a database and can be interrupted and re-started 
as often as needed.   It may take many hours to pull all the data
down.  So you may need to restart several times.

The program scans content.sqlite from 1 up to the first message number not
already spidered and starts spidering at that message.  It continues spidering
until it has spidered the desired number of messages or it reaches a page
that does not appear to be a properly formatted message.

Sometimes gmane.org is missing a message.  Perhaps administrators can delete messages
or perhaps they get lost - I don't know.   If your spider stops, and it seems it has hit
a missing message, go into the SQLite Manager and add a row with the missing id - leave
all the other fields blank - and then restart gmane.py.   This will unstick the 
spidering process and allow it to continue.  These empty messages will be ignored in the 
next phase of the process.

# IMPORTANT

One nice thing is that once you have spidered all of the messages and have them in 
content.sqlite, you can run gmane.py again to get new messages as they get sent to the
list.  gmane.py will quickly scan to the end of the already-spidered pages and check 
if there are new messages and then quickly retrieve those messages and add them 
to content.sqlite.

The content.sqlite data is pretty raw, with an innefficient data model, and not compressed.
This is intentional as it allows you to look at content.sqlite to debug the process.
It would be a bad idea to run any queries against this database as they would be 
slow.

The second process is running the program gmodel.py.  gmodel.py reads the rough/raw 
data from content.sqlite and produces a cleaned-up and well-modeled version of the 
data in the file index.sqlite.  The file index.sqlite will be much smaller (often 10X
smaller) than content.sqlite because it also compresses the header and body text.

Each time gmodel.py runs - it completely wipes out and re-builds index.sqlite, allowing
you to adjust its parameters and edit the mapping tables in content.sqlite to tweak the 
data cleaning process.

You can re-run the gmodel.py over and over as you look at the data, and add mappings
to make the data cleaner and cleaner.   When you are done, you will have a nicely
indexed version of the email in index.sqlite.   This is the file to use to do data
analysis.   With this file, data analysis will be really quick.

You can look at the data in index.sqlite and if you find a problem, you 
can update the Mapping table and DNSMapping table in content.sqlite and
re-run gmodel.py.

1) The first, simplest data analysis is to do a "who does the most" and "which 
   organzation does the most"?  This is done using gbasic.py.

2) There is a simple vizualization of the word frequence in the subject lines
   in the file gword.py.
   
3) A second visualization is in gline.py.  It visualizes email participation by 
   organizations over time.
