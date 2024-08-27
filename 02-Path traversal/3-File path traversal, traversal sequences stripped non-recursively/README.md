# Writ-up: File path traversal, traversal sequences stripped non-recursively

![](img/logo.png)

Lab-Link: **[File path traversal, traversal sequences stripped non-recursively](https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively)**

This write-up for the lab *File path traversal, traversal sequences stripped non-recursively* is part of my walkthrough series for [PortSwigger's Web Security Academy](https://portswigger.net/web-security).

Learning path: Server-side vulnerabilities >> Path traversal

Difficulty: APPRENTICE

## Summary

__Path traversal__ is also known as __directory traversal__. These vulnerabilities enable an attacker to read arbitrary files on the server that is running an application. This might include:

_Application code and data._

_Credentials for back-end systems._

_Sensitive operating system files._

## Description

This lab contains a path traversal vulnerability in the display of product images.

The application strips path traversal sequences from the user-supplied filename before using it.

To solve the lab, retrieve the contents of the `/etc/passwd` file.

## Impact

an attacker might be able to write to arbitrary files on the server, allowing them to modify application data or behavior, and ultimately take full control of the server.

## what I do

1. From the description of the lab, I realized that I need to access the content of the `passwd` file.

2. I entered the `home` page to take a quick look at its content and found that it only contains images and paragraphs.

![](img/home-page.png)

3. What caught my attention were the images because they are treated as files, so I took a look at how to retrieve these images on the website.

4. Using [developer tools](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Tools_and_setup/What_are_browser_developer_tools), I found that images are being called from an image file via a parameter called `filename`.

![](img/filename-parameter.png)

5. Before I modify the filename list, I remembered from the description that the application strips path traversal sequences from the user-supplied filename before using it.

1. So I thought about modifying the `filename` value and setting `....//....//....//....//etc//passwd` as the new value, Because the application will only remove one `../` segment, I doubled it to `....//` to retrieve the `passwd` file.

![](img/passwd-file.png)

![](img/congratualtions.png)

7. Indeed, I was able to access the `passwd` file, but the problem is that I couldn't read its content. So, I thought of using [Burp Suite](https://portswigger.net/burp/communitydownload) to read the file's content.

8. I opened the image as a link, modified the filename value, and intercepted the response using [Burp Suite](https://portswigger.net/burp/communitydownload) to see the file's content. Indeed, I was able to read it.

![](img/get-passwd.png)

![](img/passwd-content.png)

__congratulations!__

## Short steps

1. Use Burp Suite to intercept and modify a request that fetches a product image.

2. Modify the filename parameter, giving it the value:`....//....//....//etc/passwd`

3. Observe that the response contains the contents of the /etc/passwd file.

## References

*OWASP*: https://portswigger.net/web-security/file-path-traversal

*Medium*: https://medium.com/@Steiner254/directory-path-traversal-288a6188076

*Youtube*: [Intigriti](https://youtu.be/17KYOIf5ZbU) - [z3nsh3ll](https://youtu.be/n0M-nOEB6a8) - [Michael Sommer](https://youtu.be/bydjunJhZaE)