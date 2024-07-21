---
title: 'Small things&#8230;'
date: '2006-09-27T16:18:00+05:30'
status: publish
permalink: /2006/09/27/small-things
author: Sidhu
excerpt: ''
type: post
id: 4
category:
    - 'Technology  General'
tag: []
post_format: []
blogger_blog:
    - amardeepsidhu.blogspot.com
blogger_author:
    - Sidhu
blogger_permalink:
    - /2006/09/sometimes-even-very-small-things-mess.html
---
<span style="font-family:verdana;"><span style="font-size:85%;">sometimes even very small things mess up everything in your head…something similar happened with me also…had two lists of numbers…some numbers were missing in one…wanted to find out thoes..instead of inserting values in tables then using SQL or using diff on Unix…thought about using Excel for the same…much heard about function VLOOKUP is there…which we can use for such scenarios…so I also tried the same….had heard a lot abt vlookup but was generally heard of it…as when people used to write “=vlookup(bla bla bla bla…)”…i just used to count my heartbeat…and look around…so this was the time when i really needed to learn vlookup so as a normal computer geek wud do…i opened google.com and started searching the same…what i got was really disappointing….everywhere i almost found the same language….(oops….i also checked microsoft help even dat techie lingo didnt help as when you have messed up the things…nothing really gets into your head….)…some of that text…m pasting here with a hope that it wont create copyright issues….i really got nothing out of thi</span></span><span style="font-family:verdana;"><span style="font-size:85%;">s….  
<span style="font-style: italic;font-family:verdana;font-size:78%;">“</span></span></span><span style="font-style: italic;font-family:verdana;font-size:78%;">Searches for a value in the leftmost column of a table, and then returns a value in the same row from a column you specify in the table. Use VLOOKUP instead of HLOOKUP when your comparison values are located in a column to the left of the data you want to find.</span>

<span style="font-size:78%;">The V in VLOOKUP stands for “Vertical.”</span>

<span style="font-size:78%;">**Syntax**</span>

<span style="font-size:78%;">**VLOOKUP**(**lookup\_value**,**table\_array**,**col\_index\_num**,range\_lookup)</span>

<span style="font-size:78%;">Lookup\_value is the value to be found in the first column of the [array](http://www.blogger.com/post-edit.g?blogID=31843123&postID=115935508615336437#). Lookup\_value can be a value, a reference, or a text string.</span>

<span style="font-size:78%;">Table\_array is the table of information in which data is looked up. Use a reference to a range or a range name, such as Database or List.”</span>

 <span style="font-size:85%;"><span style="font-family:verdana;">so when everything said no…started looking around….and found wat the heck vlookup is and how to handle it….wud like to write here…for people like me…..anything you dont undestand…do leave a comment…..it wud make me happy in 2 ways…one thing that my post helped you to learn vlookup n another that people are reading my post….so here goes the story abt vlookup…</span></span>

“basically vlookup is a function which we use to compare two lists of values…two in the sense….like i have first list as “1 2 3 4 5” and second list as “3 4 5″…now i want to pick up values from 2nd list one by one and search in first list and let me know that whether it is there in the first list or not…note that it is one way only….<span style="font-size:85%;"><span style="font-family:verdana;">values in first list will not be searched in second list…you need to write another vlookup in reverse manner to accomplish this…so lets come to the point…  
the syntax of vlookup is  
</span></span><span style="font-style: italic; font-weight: bold;font-family:verdana;font-size:85%;">VLOOKUP(lookup\_value,table\_array,col\_index\_num,range\_lookup)</span>

<span style=";font-family:verdana;font-size:85%;">lookup\_value is the value we want to look for….as per above example…first value from 2nd list….table\_array is the list of values in which we will look for lookup\_value…i mean the first list….(! note one thing that the first list(the list of values in which we will look for a value,always has to be the first column in the excel sheet)….necassary kinda thing…</span>

as we write general functions in Excel..this is also written in the same manner..  
like =VLOOKUP(B2,$A$2:$A$6,1,0)

B2 value we want to search

<span style=";font-family:verdana;font-size:85%;">$A$2:$A$6 the list in whic</span><span style=";font-family:verdana;font-size:85%;">h we will look the above said value</span>

1 basically denotes what will be written if the value is there in the first column…here i will give 1 that it will print the same value from first column itself…

0 this field<span style="font-size:85%;"><span style="font-family:verdana;"> is a logical value that specifies whether you want VLOOKUP to find an exact match or an approximate match. If TRUE or omitted, an approximate match is returned. In other words, if an exact match is not found, the next largest value that is less than lookup\_value is returned. If FALSE, VLOOKUP will find an exact match. If one is not found, the error value #N/A is returned(here 0 means <span style="font-weight: bold;">false</span>)</span></span>

now why the range of values in which i am searching my value is written as  
<span style=";font-family:verdana;font-size:85%;">$A$2:$A$6 not as A2:A6…there is a logic behind this also…if i write as A2:A6…there will be one problem….suppose it picks up the first value and searches for it in the first column….and the value is found at 4th place….(place here means 4th row…)…then when it will search 2nd value…it will start from 4th row only</span><span style=";font-family:verdana;font-size:85%;"> not 1st…thatz not wat i want….so with </span><span style=";font-family:verdana;font-size:85%;">$A$2:$A$6 i have fixed the range that every values is to be looked in this range only ie. first row to last row of first column…</span>  
<span style=";font-family:verdana;font-size:85%;">  
</span><span style="font-size:85%;"><span style="font-family:verdana;">here is the screen shot of everything</span></span>

[![](http://photos1.blogger.com/blogger/2517/3470/320/1.jpg)](http://photos1.blogger.com/blogger/2517/3470/1600/1.jpg)

<div style="text-align: left;">hope it makes using vlookup beautiful… <span style="font-size:85%;"><span style="font-family:verdana;">thanks for your time</span>  
<span style="font-family:verdana;">….Sidhu</span>  
</span>

</div>