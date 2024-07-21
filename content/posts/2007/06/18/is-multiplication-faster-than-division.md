---
title: 'Is multiplication faster than division ?'
date: '2007-06-18T23:02:00+05:30'
status: publish
permalink: /2007/06/18/is-multiplication-faster-than-division
author: Sidhu
excerpt: ''
type: post
id: 23
category:
    - 'Oracle Tips'
tag: []
post_format: []
blogger_blog:
    - amardeepsidhu.blogspot.com
blogger_author:
    - Sidhu
blogger_permalink:
    - /2007/06/is-multiplication-faster-than-division.html
---
An interesing post by Laurent. Check out <http://laurentschneider.com/wordpress/2007/06/to-divide-or-to-multiply.html>

My findings on 10gR2 on Windoze XP

```
<br></br>SQL> var z number<br></br>SQL> var y number<br></br>SQL> exec :z := power(2,102)*2e-31;<br></br><br></br>PL/SQL procedure successfully completed.<br></br><br></br>SQL> exec :y := 1e125;<br></br><br></br>PL/SQL procedure successfully completed.<br></br><br></br>SQL> set timi on<br></br>SQL> exec while (:y>1e-125) loop :y:=:y/:z; end loop<br></br><br></br>PL/SQL procedure successfully completed.<br></br><br></br><b style="color: rgb(255, 0, 0);">Elapsed: 00:00:00.10</b><br></br>SQL> set timi off<br></br>SQL> print y<br></br><br></br>      Y<br></br>----------<br></br>9.988E-126<br></br><br></br><br></br><br></br><br></br><br></br>SQL> exec :z := power(2,-104)*2e31;<br></br><br></br>PL/SQL procedure successfully completed.<br></br><br></br>SQL> exec :y := 1e125;<br></br><br></br>PL/SQL procedure successfully completed.<br></br><br></br>SQL> set timi on<br></br>SQL> exec while (:y>1e-125) loop :y:=:y*:z; end loop<br></br><br></br>PL/SQL procedure successfully completed.<br></br><br></br><b style="color: rgb(255, 0, 0);">Elapsed: 00:00:00.04</b><br></br>SQL> set timi off<br></br>SQL> print y<br></br><br></br>      Y<br></br>----------<br></br>9.988E-126<br></br><br></br>SQL><br></br>
```

Sidhu