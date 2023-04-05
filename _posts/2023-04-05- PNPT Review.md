---
title: PNPT Certification Review
date: 2023-04-04
categories: [Reviews]
tags: [review, pnpt, tcm, certification, cybersecurity, exam]
---

![Pnpt-badge](/assets/img/pnpt_badge.png)

Recently I have passed the PNPT from the first attempt and today I wanna share my experience with you

**Content:**
- A brief introduction about the PNPT
- Sharing my thoughts about the course material
- Sharing my experience regarding the exam 
- Recommendations for passing the exam




## What is the PNPT? 

The Practical Network Penetration Tester (PNPT), created by TCM Security (TCMS), is ethical hacking certification exam that assesses a pentester’s ability to perform an external and internal network penetration test. To complete the exam, pentesters must: 

-   Perform reconnaissance to gather OSINT 
-   Leverage common web server vulnerabilities to breach the perimeter  
-   Leverage Active Directory (AD) vulnerabilities to move laterally and vertically within the network 
-   Compromise the Domain Controller (DC) 
-   Produce a professionally written pentest report 
-   Deliver a live report debrief to TCMS assessors

### The 3 stages of the exam:

- Full 5 days to complete the assessment
- 2 days to write a professional report
- 15 minutes debrief in front of one of the TCM's team

### Sugessted trainings to pass the exam:
Trainings provided and suggested by TCM 

- Practical Ethical Hacking (25 hours)
- Linux Privilege Escalation (6.5 hours)
- Windows Privilege Escalation (7 hours)
- Open Source Intelligence (OSINT) (9 hours)
- External Pentest Playbook (3.5 hours)


## My Review

### The Courses Material

Regarding the content of the 5 trainings all are made by the same guy "Heath Adams",
I consider his content and teaching method is very well-made.
He is known for his ability to simply explain topics in such a way that anyone can understand.

The course does very well explaining a high level view of the topic and give a practical example.
But sometimes you have to dig deep and search the topics to really understand the topics and how it really works from low level prespective.


### The Exam

The exam is made to stimulate a real life scenario so It wan't like a ctf game where you see unexpected and unrealistic environment and vulnerabilities plus the whole environemt was serving one purpose which is comprimising the DC meanig you don't have to compromise different machines that has nothing to do with each others like OSCP type of exam.

As well as writing a professional report of your findings and recommended mitigations followed by a video call debrief and talk about your assessment just like a real life penetration test.

The exam experience was very amusing, I didn't face any problems or instability in the exam envronement

You can start/reset the exam environment via the exam platform so you don't have to contact the support, you can do it yoursel in matter of a click.

I only had to contact the support only one time prior taking my exam and they responded almost immediatley!.


Once you start the exam you get:
- Rule of Engagement 

PDF file for the target information and scope ..etc
- VPN config file

For connectiong to the exam network
- Wordlists

You get wordlists to use for all your bruteforcing and password cracknig




Based on the target information you get, You must use OSINT techniques and skills to gather further info to aid you to get a foothold into the target network.

Once you are inside the internal network you must use the skills you learnt to exploit and gain access and pivot into the network as well as move laterally and vertically and use various attack methoed until you reach the enc goal which is compromising the Domain Controller.  

After comprimising the DC, Report writing part comes in.

TCM already provides their report template example to use as well as a tutorial on how to write the report.
You can use any report template or write your own so you don't have to use their report template.

The only part is left is doing a 15 minute debrief with one the TCM's team.
Where you present and discuss your findings and recommended remidiations using either your report or you can make your own presentation slides.

Finally you get your certification!


![Pnpt-cert](/assets/img/PNPT-cert.png)


For me, I really enjoyed the exam, the whole experience for me was so much fun.
Having the benefit of the time window of 5 days, It was really easy going process, I took so many breaks and was able to complete the exam at very slow pace as well as enjoying the experience even with all rabbit holes I went through.



## My Recommendations To Get Ready For The Exam


- I really suggest going over all courses recommended by TCM 

    By doing that make sure you understand what is taught in the courses

- Making sure that you understand all the topics very well and practised it 

    It's benefecial to search for those topics and dig deep to fully understand.

- Take good notes

    Nothing can emphasis how important is this

- Practice

    The key here is to practice what you have learnt and you can do that by solving as many labs as you can

    1. Solve all labs mentioned in all 5 courses.
     
    The course materials suggests many machines that you can practice with, make sure you solve those.
    (HackTheBox, Tryhackme, Self-hosted labs)
    2. Solve [Active Directory 101 HTB](https://app.hackthebox.com/tracks/Active-Directory-101)
    
    3. Solve Tryhackme Network Labs ([Holo](https://tryhackme.com/room/hololive), [Wreath](https://tryhackme.com/room/wreath)).

    Make sure you can use all skills learnt to hack whole network

- One last note is to keep it simple!

    And remember, all you need to pass the exam is in the course material.


**Done all the above?**

Now I can tell you you're ready to take not just the PNPT but the OSCP as well and hopefully passing them too.
