# AWAE-OSWE Review

In 2019, Offensive Security finally released their Advanced Web Attacks and Exploitation (AWAE) course and its associated Offensive Security Web Expert (OSWE) certification in an online format. 

This is a course focused on web applications and, more notably, on what is called "White Box" testing. That means, a security test (or assessment) in which we have access to the source code, database, and everything that is involved, so that we can assess and search for any vulnerabilities.  

As this is a course that deals directly and heavily with source code analysis, I believe it is a great experience not only for penetration testers, but also for web application developers. Note that it includes examples with different programming languages and frameworks, including PHP, Javascript/NodeJS, C#, Java, and more.   

## Taking the course
For those who are familiar with Offensive Security courses, for AWAE (or WEB-300 as it's currentlly officially called) the student will receive a PDF file, accompanying video lessons, and VPN access to servers for exercises and practice. 

The PDF's Table of Contents (or what they call the "Syllabus") is actually [publicly available](https://www.offensive-security.com/documentation/awae-syllabus.pdf) and does give a good insight of the topics covered. It goes from the basics of using Burp Suite, to many techniques on how to bypass authentication and get to Remote Code Execution (RCE). We get to play with XSS (Cross-Site Scripting) vulnerabilities, type juggling, vulnerabilities in templates, and more. One of the things I personally liked the most is that the student is also exposed to different programming languages, which, I believe, helps to generate a more well-rounded professional. While, as previously mentioned, most of the book is actually focused on white box testing, the last chapter takes the student through a black-box assessment, which may be very valuable in the student's professional career. 

Full disclosure: While I do personally appreciate the effort put into the videos and even like the idea, I have been working on a time crunch, so I stayed away from the videos this time. I focused on following the text and doing all the exercises.

Differently from my experience with PWK and CTP, the exercises for AWAE have "extra miles". These are questions that are intended to make the student think about problems. Some of them are kind of a thought exercise, others are more practical challenges. Again, due to a very limited availability in terms of time, I enforced a 2 hours limit on all extra miles for myself. So, I did not complete all of them 100%, but I made sure to put some effort and, at the very least, have a written plan on how I'd answer each one of them (even if I had not time to test it out). I believe it was really paramount in preparing for the exams because, as their students have learned, Offensive Security likes to throw curve balls. 

Besides the exercises from the material, the course offers 3 virtual machines (namely Answers, DocEdit, and Sqeakr) for the students to practice what was learned. These are accessed through the VPN that connects the student to the course. By the way, differently from my experience during PWK, the VMs  are exclusive for each student. So, we don't need to be fighting for resources with other students, having a machine rebooted by someone else in the middle of an exercise.  

Of the lab machines, Answers and DocEdit are for white-box testing, while Sqeakr is a black box. But, if possible, I strongly recommend students to go through all of them. If time is limited, a student should try do complete at least Answers and DocEdit, in that sequence. 

By the way, the student also have access to a Wiki with code from the exercises and information for the machines. At the end of the course, we tend (myself included) that the first module even provide a "/etc/hosts" sample for the students. That includes IP address of Sqeakr. One of the most repeated questions I saw in the course Discord channel (a must-use resource) was "what is the IP address of Sqeakr? It's not in the Wiki". Yes, it is. I guess OffSec wanted you to "Try Harder" to find it. ;-)  

If you do have time between the course and the exam, I'd also suggest you to try VulnHub's [SecureCode1](https://www.vulnhub.com/entry/securecode-1,651/) and [bdmyy's vulnerable applications](https://github.com/bmdyy/). They will definitely help preparing you for the exam, but they may also provide great insights for any security assessment. 

A great resource for this course is actually Offensive Security's Discord Server. When you enroll in the course, you can request access to a private channel for WEB-300 Students. This channel is frequented by peers, people who have already taken the course, and for "community companions". In general, everyone is willing to help you the best way possible so that you learn the topics. Spoilers are not welcome, though. 


## Taking the exam

I'll keep all (or most) of the details away, due to Offensive Security's Academic Policy. But here go some lessons I can share. 

The exam is about 72 hours long. 47:45 hours for testing and 24 hours for reporting. You'd need to scroe 85 out of 100 possible points. One tricky thing here is that we assume that if you complete all the tasks, you have 100 points. Remember: You'll need to provide a report. It's unknown how much the report can affect your results, so pay attention to that part as well.  
 
Now, your commitment actually starts 15 minutes earlier your assigned time (so, you can really spend a total up to 72 hours in this thing). You need to connect to the proctoring software, provide proof of identification, and go through a check up with your proctor. 

The check up includes sharing screen (or screens), show your identification document,  and the room where you're doing the exam.  In my case, I had two screens connected using an external monitor connected to the HDMI and the webcam on my laptop. It was a pain to move it and show the room (particularly under the desk) because of the short HDMI cable. If you have a similar setup, get a big cable. 

As usual, when the time for your exam starts, you receive a list of tasks to complete, with the points assigned to them, and your clock starts ticking. At this point, it's a pretty well known fact that the tasks consist of getting authentication bypass and RCE. You'll also need to write an automated exploit.

Best way to succeed is to have a plan on how to attack each one. Keep notes of what you've done and keep track on what is missing. 

For one of the machines, I found myself out of ideas. I looked at a checklist quickly and that was all that I needed to find my way in.  

Also, take breaks! I took breaks for Lunch, coffee, more cofee, and snacks.... and more coffee. But the important is that your mind does need a break. For OSCP, I went almost the 24 hours up. For a 48 hours exam, that is not feasible (or wise). 

By the end of the first day, I had a good idea of how to complete 50% of the objetives. I went to bed at 1am, with no alarm set. But  I woke up about 4 hours later, eager to see if I was on the right track. Spent a couple of hours trying to get it working and by 7am, I had a complete working exploit (tested on Debugger machine). 

Again, talking about breaks, at this point I had a nice breakfast with family, before attacking the next challenge. 

At 9am fo the second day, I actually started working on the second half of the exam. Before lunch, I had a good idea on how to get the last portion of the last machine (RCE). After lunch (and the natural laziness after it, cured with yet another cup of coffee), it took me a few hours looking at the code. Again, having a checklist to remind you of things you may forget helped a lot. I finished the final machine right at around 7pm. I had over 14 hours to the end of the exam. 

I started working on the report, and I could have worked longer on it, but family also required attention. And I was frankly exhaust. So, I took the rest of the night off and started on the report in the next day. I was confident that I had all evidence I needed. 

In the next day, I resumed the report. Struggled a bit with formatting, but it was mostly the "easy" part of this entire process, as I had actually recorded plenty of evidence. 

That said, while writing it, I noticed two things:
1. There were parts of the report that, when I was describing the vulnerability, I wished I had more evidence/screenshots. As the saying goes, an image sometimes can tell a story much better than words. 
2. I realized that one of my exploits could, potentially, be much better if I had done things slightly differently, but I couldn't test it anymore.  

So, I finished it with doubts still polluting my mind. 

A couple of days later, I did receive the exams results and was I glad! 
![[Pasted image 20211011185308.png]]

## Lessons Learned
1. **During training, do as much "Extra Miles" as possible, including the "Lab" boxes (Answers, DocEdit, and Sqeakr).** Even if what you learn from them is not directly applied to the Exam, it will help you. And it will also help you in your professional life. Guaranteed!
2. **Keep a checklist of things to be tested and stick with it.** ApexPredator, from the OffSec Discord, created [this one](https://github.com/ApexPredator-InfoSec/AWAE-OSWE), which I found very useful. If I had really followed the checklist, I wouldn't have wasted time wondering and completed it even quickier.
3. **Use your time wisely.** Take screenshots as you go. If you have spare time, use it to start the report. Things are fresher on your mind and if you notice something is missing in terms of evidence, you can still gather it. Also, hindsight is 20/20. As it happened to me, during the report, you may realize that there could be better/easier/smarter ways, and you wouldn't need to wonder if you could have done things differently.  
4. **Be social!** Join the discord channel and have nice conversations about security with people all over the world. It's a fun way to learn. 

## Final thoughts

Overall, AWAE was a great experience, one that I do find very valuable for penetration testers, security professional, and any web developer.. heck, any developer would find value in such a course.  

My favorite parts was the variety of environments, programming languages, and techniques discussed during the course, and the discussions that happened on the Discord Channel. 
