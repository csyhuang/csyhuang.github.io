---
layout: post
title: Installation of Jekyll on Windows 10
tags: ['setup']
comments: true
---

Below are the procedures I used to install Jekyll with problem solvers:

Main reference sites:

[Jekyll on Windows](https://jekyllrb.com/docs/windows/)  
[Easily install Jekyll on Windows with 3 command prompt entries and Chocolatey](https://davidburela.wordpress.com/2015/11/28/easily-install-jekyll-on-windows-with-3-command-prompt-entries-and-chocolatey/)  
<!--more-->
Procedures:
1. Open Powershell  
2. Run Powershell as administrator: [[Reference](http://stackoverflow.com/questions/7690994/powershell-running-a-command-as-administrator)]  
   > Start-Process powershell -Verb runAs  
3. Change execution policy to enable installation of Chocolatey (a package manager): [[Reference](http://stackoverflow.com/questions/4037939/powershell-says-execution-of-scripts-is-disabled-on-this-system)]  
   > Set-ExecutionPolicy RemoteSigned  
4. Installing Chocolatey: [[Reference](https://chocolatey.org/install)]  
   > iwr https://chocolatey.org/install.ps1 -UseBasicParsing \| iex  
5. Update the certificate to install ruby: [[Reference](http://guides.rubygems.org/ssl-certificate-update/)]  
   [http://guides.rubygems.org/ssl-certificate-update/](http://guides.rubygems.org/ssl-certificate-update/)
6. Install ruby:  
   > choco install ruby -y  
7. Close the window and open a new command prompt with Administrator access (i.e. step 2)  
8. Install gem bundler  
   > gem install bundler  
9. Install Jekyll  
   > gem install jekyll  
10. Done :)  
