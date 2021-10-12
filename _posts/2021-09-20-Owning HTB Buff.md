---
layout: post
title: Owning HTB Buff
categories: [informational]
tags: [easy,htb]
fullview: false
comments: false
---

This will be the first of a series of posts about vulnerabilities found in [Hack-the-box](https://hackthebox.eu)(HTB) machines, and also associated recommendations for mitigating those vulnerabilities. In all posts of this series, you'll find the following disclaimer.  

> Disclaimer: This is not a walkthrough on how to own machines. Instead, I'll be using **retired** [Hack-the-box](https://hackthebox.eu)(HTB) machines as examples to discuss vulnerabilities, exploitation, and mitigation. If you have access to the retired machine, you would have to work through the "hints" found here. 

Let's start this series of discussions with one of the first boxes I owned from HTB: Buff. 

The reason we'll start with this one is that, as the name hints to, this box introduces one of the most basic concepts we need to understand: **buffer overflows**. It also is affected by an issue that is often overseen by system administrators: **applications running with excessive privileges**. 

### The Target 

The target Buff is a Windows machine running  an open source application called "Gym Management System 1.0", which is known for having an unauthenticated Remote Code Execution (RCE)

#### Foothold 
The apweb application allows any unauthenticated user to upload arbitrary code. The initial website shows links to two sites and also indication that FTP is used to distribute nightly built releases. 

With that, an attacker can quickly get RCE and even a reverse shell, which will be running at the same privileges as the user that runs the server, in this case `shaun`. 

#### Privilege Escalation
After initial access, the attacker will try to enumerate what applications and services are running on the server. That will include a service running on port 8888/TCP. The attacker will also find a program on the `Downloads` folder called `CloudMe_1112.exe`. 

A quick search on the internet will tell the attacker that this application is vulnerable to buffer overflow. An exploit for this application is well known and can be found on ExploitDB. 

Successfully using the exploit with a reverse shell payload (obtained from metasploit) would result in a shell as an "administrator". At that moment, the attacker would have complete control over the machine. 

### The issues

There are three main issues that are worth discussion with this box.

#### Broken Access Control
The web application suffers from the most prevalent vulnerability, listed under the [2021  OWASP Top 10 list](https://owasp.org/Top10/A01_2021-Broken_Access_Control/). 

The [Gym Management System 1.0](https://projectworlds.in/free-projects/php-projects/gym-management-system-project-in-php/) (GMS) has a file upload feature that fails to authenticate user information. 

A quick look at the code in `upload.php` would show the following: 
```php
<?php
include_once 'include/db_connect.php';
include_once 'include/functions.php';
$user = $_GET['id'];
$allowedExts = array("jpg", "jpeg", "gif", "png","JPG");
$extension = @end(explode(".", $_FILES["file"]["name"]));
if(isset($_POST['pupload'])){
if ((($_FILES["file"]["type"] == "image/gif")
|| ($_FILES["file"]["type"] == "image/jpeg")
|| ($_FILES["file"]["type"] == "image/JPG")
|| ($_FILES["file"]["type"] == "image/png")
|| ($_FILES["file"]["type"] == "image/pjpeg"))
&& ($_FILES["file"]["size"] < 20000000000000)
&& in_array($extension, $allowedExts))
  {
...
```
First, an attacker will notice that application uses the GET parameter `id` and stores that in a variable called `$user`. There's no verification whether that the user is logged in the application or even if that id is valid (i.e., that the user exists in the system.) 

The application only tests: (1) if file type is one of some acceptable image formats; (2) if the file extension is also of one used for image; and (3) if the file size is less than 20 TeraBytes(!?). 

The file type is actually informed by the browser and an attacker that knows that the file type is being tested can easily modify that information. 

The file extension sounds tricky. But let's look at the code in the file `upload.php`. 

```php
$pic=$_FILES["file"]["name"];
            $conv=explode(".",$pic);
            $ext=$conv['1'];

move_uploaded_file($_FILES["file"]["tmp_name"],
      "upload/". $user.".".$ext);
```
The first statement would split the name of the file using periods (".") as separators. This would return an array. It stores in the variable `$ext` the second element of the array (remember: array indexes usually start from 0). 

This code actually makes sense if we think that a username is usually in the form `filename.extension` (for example: `cmd.exe` or `index.php`). 

The code continues to use the values stored in  `$user` and `$ext` to save the file in a folder called `upload`.  

What the attacker can do to bypass this test is simply create a file with "2 extensions". For example, a file called `test.php.png` would have an acceptable image extension and would be saved as `{$userid}.php`. 

I will not even comment on the maximum file size!

#### Buffer Overflow

The general idea for a [buffer overflow](https://en.wikipedia.org/wiki/Buffer_overflow) is that a program would reserve an area of memory to store some data and the attacker enters more data than that the program can accepts. 

At the very minimum, the program would crash (and potentially result in Denial of Service). Some buffer overflow vulnerabilities can also lead to remote code execution (such as in the case of CloudMe 1112). 

I intend to come back to the topic of Buffer Overflows in a later post. 

#### Excessive Privileges for Applications

Besides the fact that the CloudMe application had a buffer overflow vulnerability and had not been properly patched, the service was also found running as a privileged user, or `administrator` in this case. 

In the case of a Windows machine, users such as `Administrator` or `SYSTEM` are usually considered "super users", and, as such, are commonly targeted.  On a Linux machine, you'd find processes runing as `root`. 

The clear consequence of having such processes running as a super user is that if an attacker can successfully modify the control flow (in other words, inject code in the application), the attacker will be able to assume the privileges of the user that is executing that process. **In other words, whatever code the attacker is able to execute, it will be executed as super user.** 

### Mitigations
To mitigate the issues above, there are two general approaches that should be implemented by the developers and/or system administrators that would make an attack a lot more difficult. 

#### Input Validation
Both the issue of broken access control and buffer overflow can be dealt with by good programming practices. 

In particular, developers should always implement **input validation** (also known as [data validation](https://en.wikipedia.org/wiki/Data_validation). That means that a program should not trust what the user enters. Instead, all data must be validated. 

In this case, good input validation would verify if the user id is valid and if the current session belongs to that user. It would also verify whether the file being uploaded is in the right format and the filename is also in an expected format (or deal with it accordingly). It sincerely baffles me why, after splitting the filename, they would consider the extension as the second item in the array rather than the last one. 

Input Validation would also make sure that the data entered by the user is under the limits that the program expects (therefore defeating attempts of buffer overflow attacks). 

#### Proper service configuration
One issue that many would oversee in this box was the fact that the CloudMe service was running as `Administrator`.  In general, best practices adopted by the industry include having  any server program running as a limited user. 

The CloudMe was running as an unprivileged user, the attacker would not have been able to escalate privilege to super user (administrator). 
