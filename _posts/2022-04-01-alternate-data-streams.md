---
title: "Alternate Data Streams"
tags:
  - Alternate Data Streams
  - ADS
  - NTFS
  - Windows
  - Malware
---
## What are Alternate Data Streams?
An Alternate Data Stream (ADS) is a file attribute in NTFS (the main file system format in Windows). Files and folders within NTFS have different attributes, such as $DATA. Text within a .txt file gets stored in the **default** $DATA stream. The name of this default stream is empty and is for this reason often referred to as the unnamed data stream. Any **additional** $DATA streams must be named and are typically referred to as alternate data streams.

The ADS file attribute was introduced into NTFS starting in Windows NT 3.1. This was done in order to allow compatibility with Macintosh computers that use the Hierarchical File System (HFS). A file within HFS uses two 'forks': one for data and one for resource information. The data fork contains the actual data. The resource fork is used for metadata (for recording the file type and other relevant details about the file) . Windows does a similar thing through extensions (such as .txt and .exe). These extensions tell the operating system how to use the data found in the files. Because of ADS, Macintosh users could copy files to a Windows system and then back to a Macintosh computer without losing the resource fork of their files.
<br>

## Malicious use of Alternate Data Streams
Threat actors have used ADS to store malicious data. This is done in order to evade detection. One example is the [BitPaymer](https://attack.mitre.org/software/S0570/) ransomware variant. BitPaymer starts off as a regular .exe file, but when it is running the malware copies itself into two ADS: 
1. The first ADS is created to run the 'net view' command to obtain a list of network shares.
2. The second ADS is created to scramble the data on the disk and network shares. <br>

After copying itself into these ADS, it deletes the more obvious .exe file in which it arrived. Because the running copies of the original malware end up in two ADS, they are less noticeable than usual.
<br>

## How to hide data in Alternate Data Streams
Let's take a look at the following simple text file:
![A simple text file.](/assets/images/simpletextfile.png)

If we open a new PowerShell window and run the <code>Get-Item simple.txt -Stream *</code> command, we can see that there is only one stream called :$DATA. 
![Default :$DATA stream](/assets/images/psstream.png)
As mentioned before, the name of this default stream is empty and it contains the main content of the file. 

Now what if we wanted to store some secret text in our simple text file? We could do that by creating a new ADS with the following PowerShell command: <br>
<code>Set-Content .\simple.txt -Value "42 is the answer to the Ultimate Question of Life, the Universe, and Everything." -Stream MyHiddenStream</code><br>
This command will create a new ADS named "MyHiddenStream" in our simple text file. Let's display the streams again with the command that we used earlier:
![Display ADS in PowerShell](/assets/images/psstream2.png)

We can also view ADS in a Windows command prompt by using the <code>dir /r</code> command:
![dir /r](/assets/images/dir.png)

If we open our simple text file again in notepad, the content is still the same:
![Content of simple text file](/assets/images/simpletextfile2.png)

So how do we view the secret text that we just entered? Well, we'd have to call the ADS to view this text. We can do this by executing the following command: <code>notepad.exe simple.txt:MyHiddenStream</code>
![Secret text](/assets/images/42.png)


### What about storing executables in Alternate Data Streams?
Let's create a new ADS for our simple text file and store calc.exe in there. We will use the following PowerShell command: <br>
<code>Set-Content simple.txt -Value $(Get-Content $(Get-Command calc.exe).Path -readcount 0 -encoding byte) -encoding byte -stream exestream</code><br>
To read the file in one read operation, we have set a readcount of 0. The Get-Command part is only used so we don't have to type out the full path.

In order to launch the hidden executable, we can launch it via wmic as follows:<br><code>wmic process call create C:\Users\Joshua\Desktop\0\adsdemo\simple.txt:exestream</code><br>
And, as we can see, the calculator pops up:
![Running calc.exe from ADS](/assets/images/calc.png)
<br>

## How to find and remove Alternate Data Streams
There are many tools that give you the ability to scan for ADS, such as [Sysinternals Streams](https://docs.microsoft.com/en-us/sysinternals/downloads/streams). You can also use built-in commands like <code>dir /r</code> in Windows command prompt. The following Powershell command can also be run in the root of the target that you want to scan for ADS:<br>
`gci -recurse | % { gi $_.FullName -stream * } | where stream -ne ':$Data'`<br>

Removing ADS is not always adviseable. Some of them are needed for the proper use of the software that created the ADS, so make sure you have done your research before removing them. To remove an ADS you can use the following Powershell command:<br>
`Remove-Item –path <PATH TO THE FILE> –Stream <NAME OF THE STREAM>`
<br>
<br>
<br>

*Sources:*
1. *[https://attack.mitre.org/techniques/T1564/004/](https://attack.mitre.org/techniques/T1564/004/)*
2. *[https://www.bleepingcomputer.com/tutorials/windows-alternate-data-streams/](https://www.bleepingcomputer.com/tutorials/windows-alternate-data-streams/)*
3. *[https://blog.malwarebytes.com/101/2015/07/introduction-to-alternate-data-streams/](https://blog.malwarebytes.com/101/2015/07/introduction-to-alternate-data-streams/)*
4. *[https://stealthbits.com/blog/ntfs-file-streams/](https://stealthbits.com/blog/ntfs-file-streams/)*
5. *[https://nakedsecurity.sophos.com/2017/09/21/how-bitpaymer-ransomware-covers-its-tracks/](https://nakedsecurity.sophos.com/2017/09/21/how-bitpaymer-ransomware-covers-its-tracks/)*
6. *[http://powershellcookbook.com/recipe/XilI/interact-with-alternate-data-streams](http://powershellcookbook.com/recipe/XilI/interact-with-alternate-data-streams)*
7. *[https://davidhamann.de/2019/02/23/hidden-in-plain-sight-alternate-data-streams/](https://davidhamann.de/2019/02/23/hidden-in-plain-sight-alternate-data-streams/)*
8. *[https://docs.microsoft.com/en-us/windows/win32/wmisdk/wmic](https://docs.microsoft.com/en-us/windows/win32/wmisdk/wmic)*
