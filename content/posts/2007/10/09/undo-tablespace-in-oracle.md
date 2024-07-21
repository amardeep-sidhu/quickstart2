---
title: 'UNDO tablespace in Oracle&#8230;'
date: '2007-10-09T19:44:00+05:30'
status: publish
permalink: /2007/10/09/undo-tablespace-in-oracle
author: Sidhu
excerpt: ''
type: post
id: 40
category:
    - 'Oracle Basics'
tag: []
post_format: []
blogger_blog:
    - amardeepsidhu.blogspot.com
blogger_author:
    - Sidhu
blogger_permalink:
    - /2007/10/undo-tablespace-in-oracle.html
---
Today, I was following [a thread](http://forums.oracle.com/forums/thread.jspa?threadID=571079) on [Oracle Forums](http://forums.oracle.com). Someone asked a question about UNDO tablespace wrt to a scenario. The question was:  
<span style="font-style: italic;">There is a database and its hot backup is taken on Friday. Now for Saturday, Sunday and Monday there are archive logs but no backups. Suppose the machine crashes on Monday. After we restore the database to Friday (from backup), recovery will happen. As UNDO tablespace is of Friday so it has no information related to transactions that happened on Saturday, Sunday and Monday. So in the end of recovery process when we need to rollback some transactions from where that required information will come ?</span>

[Howard](http://www.dizwell.com) did 3 beautiful follow ups of this post, explaining how UNDO works. Just saving it here for quick reference. Hope its no copyright mess 🙂

<span style="font-weight: bold;">Follow up 1: </span>  
<span style="font-style: italic;">Yes, it can certainly be confusing, especially when you get told completely incorrect information! As has already (and thankfully!) been pointed out, redo logs contain redo change records from both committed and uncommitted transactions. </span>

<span style="font-style: italic;">The answer to your question is that as we re-perform transactions by applying redo in a recovery session, we redo exactly what would have been done when the transactions were first performed. That is, we’d see a redo change record that says (in effect) update EMP set sal=900 where ename=’Bob’, so we’d find the Bob record in the restored Friday copy of the data file, and we’d lock that record. Then we’d store details of Bob’s existing salary in an undo block. Then we’d store the new and old salaries in redo (yup, recovery generates redo!). Then we’d change the Bob record itself. </span>

<span style="font-style: italic;">If that’s all that’s in the archives and online redo logs, that’s all that happens: Bob’s record is left locked and changed. At the end of the recovery process, we realise that a lot of re-performed transactions need rolling back, so SMON does that…. and it knows what to roll the stuff back to because in re-performing the transactions, we generated fresh undo.</span>

<span style="font-style: italic;">So your question says, “how does the uncommited transactions rolled back with the Friday undo tablespace since they do \[not\] have latest uncommited trnsactions”, but that’s not right. The undo tablespace certainly STARTS at the state it was in on Friday. But as recovery proceeds, it gets ‘freshened up’, because the new transactions generate fresh undo.</span>

<span style="font-style: italic;">(A slightly more accurate description would be to point out that when you generated undo on Saturday and Sunday, those changes to the undo blocks would themselves have generated redo. Therefore, your redo stream, archives and online alike, have the necessary information to recover the undo tablespace. Personally, I don’t find that any more informative than thinking that applying redo generates fresh undo, but it’s up to you which mental model you prefer to work with).</span>

<span style="font-weight: bold;">Follow up 2:  
</span>*&gt;does “recovery generates redo” mean that during recovery we regenerate the same amount &gt;of redo that was generated since the last backup*

<span style="font-style: italic;">No, it’s not the same. If you do a simple test, you’ll see that. Update EMP, commit, check with Log Miner that the redo is in the logs in analyzable form. The blow up your database, restore it and recover it. Your log sequence number will have moved on, redo will have been generated… but you can mine the logs till Christmas and you won’t find a second ‘update EMP’ set of redo records.</span>

<span style="font-style: italic;">That’s why I mentioned the ‘more accurately’ bit further on in my original reply. Recovery is a bit more subtle than just sitting there issuing a lot of insert/update/delete statements as if from the keyboard of an incredibly fast typist! Metaphorically you can say, “We repeat transactions during recovery”. But actually, it’s “we apply redo change vectors”… and that doesn’t generate a one-for-one amount of redo as the original transactions did. </span>

*&gt;Will there be duplicate archive logs then?*

<span style="font-style: italic;">No. Do the Log Mining test to see this for yourself. The updates you did before the blow-up will not be visible in the logs from after (or during) the recovery.</span>

*&gt;Can we say that the undo tablespace starts from scratch with no undo at all*

<span style="font-style: italic;">I’m not sure what you’re getting at, but an undo segment is just a special sort of table, and it’s got data stored in it, even if the transactions that placed them there are long-since finished. So when you restore that file on Monday, it comes back in the state it was in when it was backed up -with data in it. Leaving aside undo\_retention for a moment, it follows from the fact that you’re doing a database recovery that none of the stuff inside the rollback segments is related to “live” transactions, therefore all of it is over-writeable. So in that sense, yes, you could say the undo tablespace, just after restore and just before recovery, is ‘clean’.</span>

<span style="font-weight: bold;">Follow up 3:  
</span><span style="font-style: italic;">I didn’t give “2 ideas”. I gave just one. I just happened to give you the option of thinking of it in two different ways. </span>

<span style="font-style: italic;">Applying redo causes transactions to be re-performed. That is the only, solo, number 1, all on its own, lonely idea being conveyed here.</span>

<span style="font-style: italic;">Now you can describe the re-performance of transactions either as “re-doing the transactions” as if some virtual user were sitting there typing insert, update and delete statements maniacally fast. That’s the way I usually think of it, largely because that’s what it looks like when you use Log Miner to peer inside the logs. Or you can think of it in a slightly more technical and accurate way, of taking the description of changes to byte-data which the redo change vectors represent and re-applying those changes.</span>

<span style="font-style: italic;">They’re both descriptions of exactly the same process with the same outcome. They just happen to use different words to describe them because some people’s mental models of what is happening work better with one than the other. It doesn’t help, however, to start thinking that somehow I’ve described two different processes. </span>

<span style="font-style: italic;">By way of a rather stretched analogy. Suppose you are a photographer. You take a lovely colour photograph of a landscape. You make a couple of copies of this photograph. The copy you framed and took especial care of one day gets damaged. Now, you can either repair that photo by going back to the scene, setting up the camera in the exact spot as before, waiting until the light conditions are exactly as they were, and then re-taking the shot. Or you can take the existing photo, compare it with another copy of the photo, and where you are lacking red/green/blue information in the damaged picture, you ‘paint in’ the corresponding red/blue/green values determined from the other print. Either way, you end up with a new photograph that looks like the old one did, and that’s the important thing: you end up with a restored print to hang back on your wall. </span>

<span style="font-style: italic;">Technically, Oracle recoveries apply deltas to existing data, rather than re-performing transactions as if done by a human being with very fast fingers. So that’s the “truth” if you want to think in those terms. But the core fact is, recovery restores data and it doesn’t really matter precisely HOW that’s done: whatever description we give for the process will be “wrong”, in either case, because only people who have seen the source code know \*actually\* how it’s done. So, I just as happily thing of “very fast fingers” as “applying deltas”, and in fact, I prefer that mental model. I just gave you the choice of models, in other words. But don’t, please, confuse that with their being two mechanisms.</span>

<span style="font-style: italic;">I have no idea why on Earth anyone participating in this thread is so hung up about the undo tablespace. The central question appears to be, ‘How can an undo tablespace from Friday help in the recovery of a database the following Monday’. But I explained that the first time and 20 posts ago! Very simply, during a recovery, the undo tablespace gets rolled forward like any other part of the database. It gets redo applied to it just as much as the USERS tablespace does. At the end of the rolled forward process, your undo tablespace is effectively a Monday tablespace. It’s fresh. It’s got all the undo generated by the weekend’s transactions in it. And there’s a bunch of transactions which were uncomitted at the time the database blew up, so although recovery blindly replayed those transactions and therefore re-performed them, they are left there in an uncommitted state and SMON goes ahead and rolls them back (users also help, but SMON does the bulk of the work). </span>

<span style="font-style: italic;">And SMON knows what to roll the uncommitted transactions back to because all the undo needed to do that was FRESHLY created by the recovery’s roll forward phase.</span>

<span style="font-style: italic;">You don’t need to read anything into the sentence in bold that isn’t there in plain English. You have to stop looking for deep and secret meanings and just read the words that are there: Your redo, whether it comes from the online logs or the archives, contains all the information necessary to recover the undo tablespace. Just as it has all the information necessary to recover ANY tablespace, in fact. </span>

<span style="font-style: italic;">Think of an undo segment just as if it were the EMP segment, or the DEPT segment. You don’t seem to be bothered about EMP or DEPT being restored from Friday and yet managing to be recovered to the state they were in on Monday. Neither should anyone be surprised that a Friday undo segment can be transformed into a Monday undo segment. That is, after all, what recovery </span>*does*<span style="font-style: italic;">.</span>

<span style="font-style: italic;">Recovery is the application of redo to a datafile to make it and the segments it contains -be they “ordinary” segments like tables and indexes, or more “exotic” segments like undo segments- more up–to-date. That means recovery is the “rolling forward in time of a datafile”. </span>

<span style="font-style: italic;">But when we start rolling forward, we can’t predict the future. So when I see “update EMP set sal=900” in the redo stream, I do not yet know whether you managed to commit that or not. I can’t see ahead. So I just blindly re-play that update and keep my fingers crossed. And I do that for every transaction recorded in the redo stream. And in the process of replaying that transaction, I also re-generate the fact that the original salaries in the EMP table were 800… which means I’ve just re-generated the undo for the EMP transaction.</span>

<span style="font-style: italic;">Only at the end of the roll forward phase can I look around and see, ‘Ah, that one wasn’t committed; neither was that one; and this one wasn’t even finished when the database blew up’. I therefore set SMON to work rolling those uncommitted transactions back… and it is at THAT point that the undo tablespace, storing that freshly-recreated undo, becomes vital for completing the recovery process.</span>

<span style="font-style: italic;">In words of few syllables: every recovery requires a roll forward and a roll back phase. Redo lets us roll things forward. Undo allows us to do the roll back. The undo tablespace is vital to recovering a database, therefore, because without it, half the job couldn’t be done.</span>

Sidhu<span style="font-style: italic;">  
</span>