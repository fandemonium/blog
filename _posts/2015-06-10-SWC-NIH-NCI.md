---
layout: post
title: Software Carpentry Workshop at NIH (NCI)
categories: teaching
tags: SWC
---

This is really fresh! I just finished my two day [software carpentry workshop](http://guyer.github.io/2015-06-09-nih/) held at 
National Cancer Institute (NCI), Rockville, MD. The course went for 2 days, from June 
9th to June 10th and taught by [Jonathan Guyer](http://www.nist.gov/mml/msed/mechanical_performance/jonathanguyer.cfm), [Adina Howe](http://adina.github.io), and I. I was in charge 
of the unix shell and version control (git) sessions. Personally, I think the course
went really well but there are definitely things I could adjust and improve in the future. Below
is the run downs.

<!--more-->


_**Audience**_

Majority of the audience are research scientists and several are computer scientists! This
created a big range of computational skills among studends. The organizer did an awesome
job on collecting the pre-class information, including expectations and self-defined
computertational skills (Many many thanks to [Michelle Dunn](https://www.linkedin.com/pub/michelle-dunn/48/428/6a) and [Lisa Federer](http://nihlibrary.campusguides.com/lisafederer)!). For unix shell, 40% of the students identified
themselves as advanced beginners, 20% as novice, 20% has never used shell, and the
rest were either proficient or competent. This was particularly challenging for me in
adjusting the teaching speed and contents. Out of all of the materials I wished to cover,
I went over pipe and `grep` really quick and barely had time to introduce `find`. While some students clearly had a firm crasp of
the shell materials, I was also glad that the students with limited shell experience
seemed to be able to follow without too much frustration. 


_**Setups**_

Like audience experience differences, computer setup is another challenge I faced. Many
of you may nod to yourselves and say "like always!". I do have to say it's much less 
of a trouble than what I have experienced previously. Some of the technical troubles
we ran into were NIH (or maybe government...) specific, such as log-ins, what could be
installed on a government computer, etc. The course was held at a NCI computer lab with
windows operating systems. Although the necessary programs were pre-installed before 
the class, some students still had trouble running the SWC windows installer. Two major
things we couldn't resolve was: nano wasn't accessable from all windows computers and 
we don't know how to copy text within gitbash (in case you wonder how to paste, use 
`insert` key).


_**Course Materials**_

I like the SWC V5 course materials. I think seperated sections are great for students to
learn on their own. However, I find it not easy for me or students to 
navigate from different pages to pages during the course. There were many different web
sites both the students and I had to manage during the course. As an instructor, I would
like to minimize the switching among my windows so that students don't get frustrated
from seeing my windows flashing around, especially during unix shell.
With multiple seperate instruction guides, I
would inevidably either keep going backwards or keep mulitple windows open and remind
students where they are at. As a previous SWC student, I have to say it's very hard to
focus on what the instructors were saying when trying to take notes and experimenting the
course materials at the same time. Therefore, I chosed the old one continuous page shell tutorial
during the course. This way, students can easily scroll back and forth and use search to find
the information they need without the need to understanding how things are categorized. 

As for git, I taught it with modified slides devloped by [Nelle Varoquaux](http://cbio.ensmp.fr/~nvaroquaux/). I chose to use
Nelle's slides not only because it has a nice continuous page version, but also because
it is simple, clear, and covers the most powerful features of git. It also made a very
clear distinction between git and Github, which many students thought was the same thing. 
Unlike how I live coded throughout the shell session, I rarely live coded for git.
Some students thought I should have also live coded while teaching git so that they could
see what the outputs looked like. But again as a previous SWC student, I found a clear slides
and a white board are much more helpful than live coding when learning git. For beginners,
especially people who are new to command lines, it is very difficult to distinguish the
things could be changed and things should not be changed in commands. This becomes impossible when one's instinct is to copy down
as much as possible of what's happening on the screen. In addition, git output displays
message differently from repo to repo. I believe showing repos with my personalized outputs would cause more confusions. 


_**Some Important Lessons Learned**_

1. Our audience loved the stickie system! 

2. If teaching at a government facility, try to have the students bring their own computer,
especially if they use windows system.

3. Make sure the facility you are going to is the only facility with that name in the area.
With that being said, there were 2 NCI in Maryland. There were some panicing moments, but 
that's a story for another time :). 

4. Uber (and your smart phone with data access...) is your best friend in a metro area!

4. Timing is always challenging. While it is important to build the basics, students may be
able to battle through the easy commands on their own with a breif introduction. As I mentioned
before, I barely had time to teach `grep`, `find` and couldn't get to automation at all
during unix shell session. As a scientist,
the automation, `find`, and `grep` were what I used the most often. Many students also
wished more time was spent on the later part of the shell course. The later part of the course
materials  were also clearly
what motivate students to learn simply because the likelihood of potential daily usages.
So next time, I'm going to introduce the shell navigation (up to redirection) more swiftly
and make sure the students get to see the core values of unix shell. Some students also thinks
short cuts, especially tab completion, should be introduced earlier. I think it's a great idea
and both students and instructor could benefit from it. 

5. Students love git! Many expressed the surprise on how useful it is :).

6. Git is confusing and it will always be. So be honest about concepts and commands that you are not familiar. I requested help from
Jon several times during the git session just to make sure that I explained some concepts,
like `ORIG_HEAD` and fast-forward, clearly. It could be difficult for students who are
new to the materials knowing something not quite clear but do not know how to phrase
the questions. When I asked Jon for help, he would explain and I would ask questions to
make sure to clarify any potential confusions. And again, the white board was crutial during
this process. 

7. I should teach more :). One thing I always admire about Adina's teaching is that she
frequently reviews materials just taught, how they are useful, and how they relate to her
personally. These are very important messages to engage students and remind them that they
could also be very efficient with these computational skills. These are important points
to keep in mind. And with more experience, I believe the transitions between materials
would become easier and easier.

8. Learn something new while you are at it, even when you are an instructor! I was a humble
student when Jon was teaching sqlite3. I have learned so much! One thing to point out, sqlite3
was not intuitive to me at years ago when I just started learning command lines. Simply with
experience, it seems so much more clear to me now. Thanks Jon! Adina has also brought up
the idea that maybe sqlite3 should be taught first to build students' confidence.
It's the most language like and the closest for scientists to relate to  compare to shell, git,
and python. It could be something to think about in the future during lessons planning.


_**A Successful Course?**_

I believe so! Well, I tried my best for this round. There are always room for improval but I
didn't scare any students away (I hope not...). That's all I have for now until next time!
