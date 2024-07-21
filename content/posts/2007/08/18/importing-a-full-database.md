---
title: 'Importing a full database&#8230;'
date: '2007-08-18T18:51:00+05:30'
status: publish
permalink: /2007/08/18/importing-a-full-database
author: Sidhu
excerpt: ''
type: post
id: 35
category:
    - 'Oracle Tips'
tag:
    - import/export
    - ora-01435
post_format: []
blogger_blog:
    - amardeepsidhu.blogspot.com
blogger_author:
    - Sidhu
blogger_permalink:
    - /2007/08/importing-full-database.html
aktt_tweeted:
    - '1'
---
Many times, we are required to restore a database from an export dmp file. Its a simple task but sometimes there are some issues left like invalid objects or some objects missing, in the newly created database. Following steps, followed in order can help in creating an error free database:

1. <span style="font-weight: bold;">Create a blank database:</span><span style="font-weight: bold;"> </span>The very first step is to create a blank database which is to be used as the target database. That can be done using <span style="font-style: italic;">Database Configuration Assistant. </span><span>(In last step of the DBCA, change redo log file sizes to 500 MB each (or some appropriate values depdening upon the size of the databaes), as during import, lot of redo will be generated, so large redo size helps in that scenario)  
  </span><span style="font-style: italic;">  
  </span>
2. <span style="font-weight: bold;">Extract DDLs and create tablespaces: </span><span>Now run the import with <span style="font-style: italic;">show=Y </span>option and create a log of all DDL statements. The main things to be looked for in the log are DDLs to create tablespaces and DB links. You may need to change the create tablespace statements according to the version of the Oracle you are using. If you have the export taken in an older version, where dictionary tablespaces were being used, you will need to change the statements accordingly, to create locally managed tablespaces.  
  (If you have the dmp file in compressed (.Z) format check [here](http://amardeepsidhu.com/blog/2007/06/06/import-export-fromto-compressed-files-directly), to run the import directly from compressed file) </span>
3. <span><span style="font-weight: bold;">Adjust the size of SYSTEM, TEMP, USERS and UNDO: </span>As SYSTEM, TEMP, USERS and UNDO tablespaces will get created with the database itself, so you can alter the sizes as per the sizes in the old database. </span>
4. <span><span style="font-weight: bold;">Edit tnsnames.ora and create dblinks: </span><span>Now edit tnsnames.ora to include all the databases used in the db links and create db links using the statements from DDL log. </span></span>
5. <span><span><span style="font-weight: bold;">Run the import: </span><span>Finally, run the import with <span style="font-style: italic;">FULL=Y</span> and *IGNORE=Y* options and after the import finishes, look for any errors in the log. At last, compile all the invalid objects in the database ([Here](http://philly.astroboy.fr/index.php?page=oracle/misc) is the link to a script to compile all the invalid objects). (If the import terminates with ORA-01435, then have a look at [this post](http://amardeepsidhu.com/blog/2007/07/14/import-ora-01435-user-does-not-exist).)</span></span></span>

To read about all the options with <span style="font-style: italic;">imp</span> have a look at [Original Import &amp; Export](http://download.oracle.com/docs/cd/B19306_01/server.102/b14215/exp_imp.htm#i1023560)[ Utilities](http://download.oracle.com/docs/cd/B19306_01/server.102/b14215/exp_imp.htm#i1023560) chapter of [Oracle Utilities guide](http://download.oracle.com/docs/cd/B19306_01/server.102/b14215/toc.htm).

Sidhu