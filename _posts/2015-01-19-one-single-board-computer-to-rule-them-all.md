---
layout: post
title: "One single-board computer to rule them all!"
categories:
  - software
  - development 
  - internet of things
tags: 
  - dev
  - software
  - development processes
  - internet of things
fullview: false
published: true
comments: true
---

For my first post of the year I wanted to write about a pet project I have been working for a while and finally I could stabilize. It's about a ***Cloud backup DYI***.

**The Problem**

As most of the people, I have plenty of pictures, documents, music, videos and games across a number of devices at home. The problem starts when I want to share my files between the different devices: pc, laptop, tablet, smart phones and a video games console. So, the first attempt to solve my problem was buying a ***NAS***, Network Attached Storage, big enough to store and share all my files across the network. I did not want to spend hundreds of dollars on a NAS, so I bought a cheap but decent one. It was not amazingly fast, but it gets the job done! When I moved all my precious data to one place, I realized that I had created a single point of failure. If something happens to the NAS, I would lose everything!

So I decided to do the most sensible thing in this case, to have an external backup. I started paying for a remote backup which to be fair works ok, but it was a little bit pricey. Additionally, if I needed more space, I would need to pay more, of course. Fortunately, I found a great post about how to back up your personal data using ***Amazon Glacier***, a low-cost storage service to archive data. It is really, really amazing! You can store GBs of data, in a safe and reliable way, replicated all around the world for, literally, cents a month!

However, I had to fix minor issues before I could jump into Glacier. The first problem is that Glacier is meant to be used by applications, not for final users. So, it does not have a friendly user interface. However, you can find some commercial apps that helps you managing your files for some dollars. The second problem, and it's a big one, is that you have to sync your data manually. This is a deal breaker because you do not want to sync your GBs of data manually, period! Luckily for us, Glacier provides a RESTful API that we can use to manage our files.


**A Recipe for Disaster**

So, it became personal! I needed to write a program that will synchronize my files to the remote backup! After all I am an engineer and a software developer, so what could possible go wrong ?!?!

At this point I had the perfect excuse to use ***Node Js***, an open source platform to write backend applications on ***Javascript***. My plan was installing Node Js along with a program to sync the files on the NAS directly. The plan sounds great, but it failed miserably because the NAS Operative System is a proprietary Linux distribution. So, to Install Node I needed to upgrade several Linux packages that this OS did not allowed me to do. In fact, by trying this I damaged the software of the NAS, damn! So, I had to reset the NAS to factory settings. After a few days of painful recovery process, I got everything restored on the NAS. But I almost lost all the data stored on it! Lesson learned, never attempt to install non vendor approved packages on your NAS!

After my first failure, I realised that I needed an extra device on my local network that will play the role of an ***Application Server***. A place where I could install all the automation tasks I wanted. In this case, sync all the data from my local storage to Glacier.


**The Perfect Storm**

For about $60, and a week I got a ***Raspberry Pi***, a credit card-sized single-board computer. where I installed:

- ***s3funnel***, a tool to uploads files to Amazon.
- Node Js
- A Node Js program I wrote that picks up my files and checks if they need to be uploaded to Glacier.
- A cron job that starts the backup every day.

After a few weeks of trial and error, the backup process became stable and reliable. Now, I'm very confident that all my information will be backed up every day on a secure, remote and highly available storage. The best part is that I pay less than one dollar a month for about 200GB of data stored in Glacier! How cool is that!


**Postpartum**

After all the failures I had playing with the NAS, Glacier, Node Js and the Raspberry Pi, I have to say that it was more painful than I expected. However, it was rewarding.

In the first place, I had the perfect excuse to play with all these tools. Secondly, I spent some money on the NAS, Raspberry PI, and Glacier. But at the end of the day my data is more secure than ever for only a few bucks! And last but not least, I have the infrastructure I needed to automate more things at home, like: downloading and stream content automatically, have control of smart devices such as lights, sensors, even my robot vacuum. If you are an automation enthusiast, ore you are interested on ***The Internet Of Things***, The sky is the limit!


**DO's and DONT's**

The question now is: would I do it again? The short answer is: I would!

However, I would recommend you to buy a good NAS like ***Synology***. It costs a few hundreds, but it is faster, more robust and has lots of applications ready to install! In fact, it can back up all your data to Amazon Glacier without all the pain I suffered.

My second advice is do not buy a Raspberry Pi. Don't get me wrong, I love it and it's a really good device. But, there are others single-board computers more powerful than the Raspberry Pi for about the same price, such as: ***BeagleBone***, ***Banana Pi*** and ***Intel Edison***, among many others. For example, if your applications relies heavily on the network. The Raspberry Pi has a 10/100 Ethernet network interface only while its rivals come with Gigabit Ethernet! #fail

My last advice is use Amazon Glacier! You can get a serious cloud based backup for cents a month!

This is how the current infrastructure for the remote backup is setup:

![My backup using Amazon Glacier]({{ site.url }}/images/back-up-to-glacier.jpg)