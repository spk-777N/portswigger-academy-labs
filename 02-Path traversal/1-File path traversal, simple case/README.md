# Writ-up: File path traversal, simple case

![](img/logo.png)

Lab-Link: **[File path traversal, simple case](https://portswigger.net/web-security/file-path-traversal/lab-simple)**

This write-up for the lab *File path traversal, simple case* is part of my walkthrough series for [PortSwigger's Web Security Academy](https://portswigger.net/web-security).

Learning path: Server-side vulnerabilities >> Path traversal

Difficulty: APPRENTICE

## Summary

__Path traversal__ is also known as __directory traversal__. These vulnerabilities enable an attacker to read arbitrary files on the server that is running an application. This might include:

_Application code and data._

_Credentials for back-end systems._

_Sensitive operating system files._

## Description

This lab contains a **[path traversal](https://portswigger.net/web-security/file-path-traversal)** vulnerability in the display of product images.

To solve the lab, retrieve the contents of the `/etc/passwd` file.

## Impact

an attacker might be able to write to arbitrary files on the server, allowing them to modify application data or behavior, and ultimately take full control of the server.

## what I do

1. From the description of the lab, I realized that I need to access the content of the `passwd` file.

2. I entered the `home` page to take a quick look at its content and found that it only contains images and paragraphs.

![](img/home_page.png)

3. What caught my attention were the images because they are treated as files, so I took a look at how to retrieve these images on the website.

4. Using [developer tools](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Tools_and_setup/What_are_browser_developer_tools), I found that images are being called from an image file via a parameter called `filename`.

![](img/upload_img.png)

5. So I thought about modifying the `filename` value and setting `/../../../../../../../../etc/passwd` as the new value to retrieve the `passwd` file.

![](img/filename_value.png)

6. Indeed, I was able to access the `passwd` file, but the problem is that I couldn't read its content. So, I thought of using [Burp Suite](https://portswigger.net/burp/communitydownload) to read the file's content.

7. I opened the image as a link, modified the filename value, and intercepted the response using [Burp Suite](https://portswigger.net/burp/communitydownload) to see the file's content. Indeed, I was able to read it.

![](img/passwd_file.png)


![](img/using_burp.png)


## Short steps

1. Use [Burp Suite](https://portswigger.net/burp/communitydownload) to intercept and modify a request that fetches a product image.

2. Modify the `filename` parameter, giving it the value: `../../../etc/passwd`

3. Observe that the response contains the contents of the `/etc/passwd` file.

__congratulations!__

## References

*OWASP*: https://portswigger.net/web-security/file-path-traversal

*Medium*: https://medium.com/@Steiner254/directory-path-traversal-288a6188076

*Youtube*: [Intigriti](https://youtu.be/17KYOIf5ZbU) - [z3nsh3ll](https://youtu.be/Wt0gk05MBz0) - [Michael Sommer](https://youtu.be/3CyrBt0E8AA)