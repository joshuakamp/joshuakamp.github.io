---
title: "Retrospect and future plans - Pt. 2"
tags:
  - random
---
## A new job, graduation internship, a shift of focus towards Android malware analysis, and more...
It's been about 6 months since I posted one of these, so I guess it's about time that I do a bit of a reflection and talk about my future plans again :)

These past couple of months have been quite busy. I've learned a ton and some really cool things have happened. 

## Retrospect: the past six months

### Software Reversing & Exploitation minor
On August 29th 2022 I started the "Software Reversing & Exploitation" minor that I briefly mentioned over at [Retrospect and future plans - Pt. 1](https://malwarepenguin.com/retrospect-future-plans/), which I'm now nearly finished with. The minor consists of 3 main parts that we are graded on: presentation, portfolio on write-ups and tooling review, and portfolio on learning track.

**(1) Presentation**<br>
For this part, we had to give a group presentation (max. 5 students) with a duration of 90 minutes. The presentation had to at least contain a discussion on one of the provided articles. I was part of the first group to give a presentation. We chose the article "Smashing The Stack For Fun And Profit" by Aleph One. In our presentation we talked about the article, explained the stack, basic x86 assembly instructions, buffer overflow basics, and shellcode. Near the end of our presentation we gave the students a little buffer overflow challenge. 

During the presentation, we were assessed on:

1) transferable skills<br>
2) quality of supporting material (such as slides, demo's)<br>
3) technical know-how<br>
4) ability to involve the audience<br>
5) ability to answer questions<br>

We received a passing grade for our presentation!

**(2) Portfolio on write-ups and tooling review**<br>
Every 6 weeks we had to hand in an individual report (max. 16 pages) containing the write-ups of pwning 2 (new) Linux binaries and 2 (new) Windows binaries AND a review on tooling used during the 6 weeks.

The 1st 6 weeks were labeled as the "beginning era", and we had to do a tooling review on debuggers.<br>
The 2nd 6 weeks were labeled as the "cat & mouse era", and we had to do a tooling review on disassemblers and decompilers.<br>
The 3rd 6 weeks were labeled as the "modern era", and we had to do a tooling review on automated vulnerability research & exploitation tools. After the Christmas holiday we have about 2 weeks left in the minor. During the past 4 weeks I mainly focused on learning about [angr](https://angr.io/) for this part of the minor. I want to learn more about [AFL++](https://aflplus.plus/) in the remaining weeks.

The collection of my write-ups and reviews will be graded by the end of the semester. I'm not allowed to share my write-ups here, as the challenge binaries may get re-used in the next edition of the minor.

**(3) Portfolio on learning track**<br>
Something that's very unique (and awesome) about this minor is that we were provided with the opportunity to spend 16 hours a week on an individual learning track. For this learning track, we were allowed to do a deep dive and learn as much as possible about a topic related to the minor. I chose malware analysis as my topic of interest, as I got really excited about this topic just before the minor started. As part of my learning plan, I read the [Practical Malware Analysis](https://nostarch.com/malware) book and worked on the included lab assignments. I've posted the write-ups for these labs on my blog. Once I finished Lab 12 of the PMA book, I changed my entire learning plan to focus more on Android malware analysis. You'll find the explanation for this in the "Graduation internship" part of this blog ;)

Every 2 weeks we had to hand in a report (max. 2 pages) on our individual learning progress. The report had to use the cycle of key events from the [Hero's Journey](https://www.youtube.com/watch?v=Hhk4N9A0oCA) as structure. The collection of my journeys will be graded by the end of the semester.

### New job!
While searching for a graduation internship, I came across a job posting for a part-time SOC Analyst at a cybersecurity company. The job ad was aimed at students, and I saw this as a great opportunity to gain experience and learn more from professionals already working in the field. Sharing knowledge is listed as one of their top priorities, and I consider myself a lifelong learner. Anyhow, I applied and got invited for an interview! The interview procedure was a lot different from what I was used to. All candidates were invited for an interview on the same night. We were brought to a training room where we received an introduction and a tech talk on Log4Shell and how they reacted to it, which was really interesting to see. After the presentation, we were given a tour around the SOC. Then, we got to the break room where we received pizza's and drinks. Yum!<br>
After chatting with some of the employees in the break room, we were brought back into the training room. Here we had to do an online assessment. This assessment contained theoretical and practical questions regarding the following topics:<br>
- General security; Attacks & Trends
- Networking; Detection, Analysis & Response
- SIEM; Detection, Analysis & Response
- File analysis; Approach & Mentality<br>

During the assessment the candidates were called in turns for a short interview. Both the assessment and the interview went very well for me, and I really enjoyed this selection procedure. Even though all candidates were invited at once, it did not feel like a competition at all. They were actually considering multiple people for the job.

A few weeks later I received the news: I've been hired!<br>
Oh, and an awesome bonus: my girlfriend (who is following the same study as I) also applied for this job and she got hired as well! We will be starting our part-time SOC analyst jobs in January 2023. 

### Graduation internship
I've contacted a total of 4 cybersecurity companies for a possible graduation internship. Each company ended up offering me a position with an interesting project to work on. The projects to chose from were:

1. Reverse engineer a (Android) mobile malware sample and create a technical blog post on the analysis of this sample.
2. Reverse engineer a LockBit ransomware sample and research their anti-analysis techniques.
3. Forensic analysis on (legitimate) Remote Administration Tools to improve detection: investigate what traces they leave behind (and other matters in the context of (anti-)forensics).
4. Researching methods that malware uses to evade detection in order to improve endpoint detection<br>

I ended up choosing project #1: Reverse engineer a (Android) mobile malware sample and create a technical blog post on the analysis of this sample. This project has been offered to me by the company that also hired me as a part-time SOC analyst. But besides this being a great way to combine my graduation internship with my job and gaining more experience within the company, I'm also super excited about the project itself and the Threat Intelligence team which I'll join for my internship. The members of this team are also very enthusiastic about reversing malware and I feel like I can learn a lot from them. <br>

In order to prepare myself for the graduation internship, I'm following this learning plan:

- (1) Maddie Stone's "Android App Reverse Engineering 101" workshop [https://www.ragingrock.com/AndroidAppRE/](https://www.ragingrock.com/AndroidAppRE/)
  - DONE!
- (2) ISTS and Google's 3-day Android Malware Analysis course [https://www.youtube.com/watch?v=CwCOGf4Uunk](https://www.youtube.com/watch?v=CwCOGf4Uunk)
  - DONE!
- (3) Android related learning material of The Frida Handbook [https://learnfrida.info/](https://learnfrida.info/)
  - Currently working on this!
- (4) Reverse engineer 3 Zanubis LATAM banking malware samples
  - Currently working on this!
- (5) Android malware analysis workshop @ Navaja Negra Conference 2022
- (6) ARM assembly basics tutorial series [https://azeria-labs.com/writing-arm-assembly-part-1/](https://azeria-labs.com/writing-arm-assembly-part-1/)

I will start my graduation internship on February 6, 2023.

### Responsible disclosure
During the Christmas holiday break, I did some website related bug bounty hunting for the first time. In 4 days I submitted a total of 3 responsible disclosure reports. I can't go into detail about the vulnerabilities yet. So stay tuned for new blog posts once the vulnerabilities have been fixed! For now, I guess I've got a new hobby...

## Future plans

### Road to Malware Analyst
2023 is going to be an exciting year for me. I can't wait to start my first cybersecurity job and I'm really looking forward to the graduation internship related to Android malware analysis. They promised to reserve a position for me as a Malware Analyst if the internship goes well. That would be a dream come true!

### Books
Humble Bundle had an amazing deal for a bunch of No Starch Press books, including [The IDA Pro Book](https://nostarch.com/idapro2.htm), [The Ghidra Book](https://nostarch.com/GhidraBook), and [Black Hat Python](https://nostarch.com/black-hat-python2E). I plan on reading these books in the new year.

### Android bug hunting
My shift of focus towards Android malware analysis and my new interest in bug bounty hunting makes me wonder what kind of bugs I can find in Android apps. So I'd like to learn more about this as well! <br>
This seems like a good resource to get started:[https://github.com/B3nac/Android-Reports-and-Resources](https://github.com/B3nac/Android-Reports-and-Resources) and this seems like a good resource to learn about ARM exploitation: [https://github.com/bkerler/exploit_me](https://github.com/bkerler/exploit_me).

## Cat
This blog post has been full of text so far. Let's close it off with this picture of my cat helping me RE. See you next year!<br>
![](/assets/images/REcat.png) <br>