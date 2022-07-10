---
title: "Retrospect and future plans - Pt. 1"
tags:
  - random
---
## It's been a while...
Since I haven't been active on my blog in the past two months: this is going to be more of a random post, describing things that have happened recently and what my plans are for the future. If you're mainly here for educational content, feel free to skip this one!<br>

## Retrospect: the past two months 
### School deadlines
I've just finished the 3rd year of my Information Security Management bachelor's degree. The past semester was focused on 'Cyber Security Technology', with subjects such as: Software, System and Network Security, Pentesting, Digital Forensics, Security Monitoring and Detection. I've had quite a few deadlines during the past two months. I had to: 
- Submit a report on our findings and mitigation steps for a vulnerable CentOS VM that we analyzed (in groups of two members) during the pentesting labs;
- Pitch about a subject related to cybersecurity. I chose to pitch about CTF events and why cybersecurity students should attend them;
- Hand in a report in which I reflected on the entire semester, my individual progress and the progress of my project group;
- Deliver the final report and give a final presentation about our vulnerability research project;
- Study for two exams.

<br>I've met all of the deadlines and have received very satisfactory grades. I'm happy.

### Fuzzing Apache Log4j
As briefly mentioned before, I've worked on a vulnerability research project during this semester. For this project we researched, reproduced (reproduction steps can be found in my previous blog post over [here](/reproducing-log4shell/)) and rediscovered the Log4Shell vulnerability in Apache Log4j. As a bonus, the time we had left was spent on finding new vulnerabilities in Apache Log4j. This is exactly what we focused on during the last weeks of this semester: fuzz testing Log4j to find new vulnerabilities.<br> 

Big thanks to my teammate Pascal Koeiman for helping me grasp the concept of fuzz testing and teaching me how to properly set up Jazzer [(https://github.com/CodeIntelligenceTesting/jazzer)](https://github.com/CodeIntelligenceTesting/jazzer).<br>

On June 9th, I decided to fuzz test the [StrSubstitutor Class](https://logging.apache.org/log4j/2.x/log4j-core/apidocs/org/apache/logging/log4j/core/lookup/StrSubstitutor.html) by using the  target code below. The reason for looking at this specific Class, is that it was one of the classes that popped up when we rediscovered the Log4Shell vulnerability by fuzz testing.<br>
![log4j001.png](/assets/images/log4j001.png)<br>*Figure 1: Screenshot of the fuzz target code.*<br>

When I compiled the fuzz target and ran it through Jazzer, it quickly resulted in the following crash:<br>![log4j002.png](/assets/images/log4j002.png)<br>*Figure 2: A string substitution resulted in an infinite recursion, which caused the application to crash.* <br> 
<br>After doing some research, this looks like the exact same issue as described at [https://issues.apache.org/jira/browse/LOG4J2-3230](https://issues.apache.org/jira/browse/LOG4J2-3230). Developers stated that this bug was fixed in version 2.17.0, but we managed to find the exact same bug on version 2.17.2 with a different input string!<br> 

Thus, we submitted our own Log4j issue ticket over at [https://issues.apache.org/jira/browse/LOG4J2-3535](https://issues.apache.org/jira/browse/LOG4J2-3535). The developers have accepted our ticket and have requested sharing the sources of either a JUnit test or single-class Java application. At the time of writing, we are still figuring out how to reproduce this crash outside of fuzzing tools in order to provide the developers with a code snippet. We have contacted one of Jazzer's developers, Fabian Henneke, for assistance in this.<br>

### Job interview
In the past few months I've developed a strong interest in malware analysis. Whenever I become very interested in a topic, I tend to browse relevant job postings in order to get a better understanding of the skills that are required. In the beginning of May I looked for 'Malware Analyst' job postings on Indeed, and I came across a vacancy that excited me. It was a vacancy for a parttime student position that involved malware analysis to improve detection capabilities. 
Even though I was (and still am) at the very beginning of my malware analysis learning journey, I applied. I figured that, since it was a student position, they might not require someone to be super experienced in this. <br>

Nearly four weeks after my application I received an e-mail. They invited me for a job interview! I was very surprised by this as I hadn't given it much thought anymore. I was focused on school deadlines and it had been a while since I sent out my application.<br>

The job interview went okay. The people that I spoke with were super friendly and we talked for a whole 70 minutes. It was clear however that they were looking for someone more experienced, so I knew my chances of getting a job were slim. Two days after the interview I received an e-mail in which they stated their decision not to continue to a second interview for the parttime position. The good news is, that they do want to have another meeting with me in September in order to discuss a graduation internship at their company.<br>

This was another learning experience for me, as it was my first "real" job interview for a cybersecurity position. I was already eager to learn more about malware analysis, but after this interview I'm motivated even more and I look forward to acquiring the necessary knowledge. I also have a great first impression of the company and I hope that I will be able to hold my graduation internship there.<br>

### Domain
I've purchased a pretty cool domain name recently, and when the time is right I will transfer this blog over to my newly acquired domain. More information on this will be out soonTM.<br>
**Update:** The blog has been transferred over to [https://malwarepenguin.com](https://malwarepenguin.com) on July 10th, 2022.

## Future plans
### Malware analysis
I will soon start a minor in reverse engineering and software exploitation. During this minor, we are able to work on an individual learning plan for 16 hours a week. My learning plan will be heavily focused on malware analysis. When I was talking to teachers about my plan, my favorite teacher granted me an access voucher to the [Ultimate Malware Reverse Engineering bundle by Zero2Automated](https://courses.zero2auto.com/beginner-bundle)! I've already started working on the beginner course that is included in this bundle.<br>
At the time of writing, I have a list of the following material in my learning plan:
- Zero2Automated - Beginner course;
- Zero2Automated - Advanced course;
- Book: 'Reversing: Secrets of Reverse Engineering', by Eldad Eilam;
- Book: 'Practical Malware Analysis', by Michael Sikorski and Andrew Honig;
	- Lab assignments that are included in this book;
- OALabs exclusive reverse engineering tutorials (Patreon);
- TCM Security: Practical Malware Analysis & Triage;
- Sam Bowne's Practical Malware Analysis class (CNIT 126);
- MalwareUnicorn Workshops;
- eLearnSecurity Certified Malware Analysis Professional (eCMAP) certification.

### Music
I'm currently writing music with a friend of mine for a blackened death metal project. I will be playing the guitar in this project. I know this has nothing to do with InfoSec (unless I start writing death metal lyrics about how great fuzz testing is), but I did want to briefly mention it here as I'm looking forward to putting out my own music.