---
date: "2024-01-24T00:00:00Z"
title: Diagnostic HTB CTF Write-up
tags : ["Forensics"]

---

# ##

- This is a good one to start off the weak and understanding the Forensic based challenges and how to go about it

## Diagnostic

This challenge tests the knowledge of forensics,,the challenge is available in Hack the Box in forensic category,

**Initiate the challenge step 1**

Download the file by pasting the IP on the browser, open the file (diagnostic.doc) 
you can analyze the file by using *oleid* and *oleobj* tool, This tool is used to analyze such documents to check what they contains, and the relationship between the two.
![image](https://github.com/f4taal/f4taal.github.io/blob/main/static/img/Diagnostic/diagnostic3.png)

**Step 2**

Paste the link on the webbrowser, and the page was empty so i decided to inspect,I found a script content,

![image](https://github.com/f4taal/f4taal.github.io/blob/main/static/img/Diagnostic/diagnostic4.png)
   it's so interesting'
Now copy the content and paste it to the text editor of your choice, i used SCITE,
![Image](https://github.com/f4taal/f4taal.github.io/blob/main/static/img/Diagnostic/diagnostic%205.png)


**step 3**

Now convert the Characters to Ascii, in this case we going to refer to Ascii table by visiTING ascii.com, and after converting, copy the part of the content to and convert it to *base 64 string* using the online tools of your choice, i used the *Cyberchef* and the other part too convert it to base 64 string. 
![webbrowser](static/img/angstrom/diagnostic7.png)

**Step 4**

After converting we can see that they the first part is quite interesting, the content of the first part is 
![webbrowser](static/img/angstrom/diagnostic6.png)
    .

you can see that the content has arrays and string values, as the name of the first file, this might contain the flag, to concatinate this i used power shell,

**Flag**
  .


<--View More-->


## Tools

- cyberchef
- oleid
- powershell


## References 

