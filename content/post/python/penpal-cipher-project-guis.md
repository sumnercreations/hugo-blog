---
title: "Penpal Cipher Project - GUIs"
date: 2019-09-11T23:41:35-06:00
draft: false
categories:
- development
- python
tags:
- cipher
- python
---

# Project GUIs with GTK3+

We are working on the final project to complete our [Complete Python Bootcamp: Go from zero to hero in Python 3 at Udemy](https://www.udemy.com/share/100058A0cdcVZbR34=/). We have decided that we will build a simple Caesar Cipher application that we are calling [Penpal](https://github.com/sumnercreations/penpal). During our conversation we decided that we would really like to try to make a GUI. We've taken inspiration from this online [cipher tool](https://cryptii.com/pipes/caesar-cipher).

## Research
We started with Google, you know, like you do, and found a lot of options for GUIs with python. 2 of them stood out.

- [Kivy](https://www.kivy.org)
- [GTK 3+](https://www.gtk.org/)

### Kivy
#### What we like
- It's new. 
  - It's fun to work with the latest and greatest, cutting edge technology. Kivy has been around since 2011, about 8 years. Not really young, however, certainly younger than GTK.
- It's cross-platform. 
  - We aren't working on anything else at the moment, however, we know we will be. The idea is for us to continue to learn more and I'm positive we'll do something on mobile at some point.
- It looks nice.
  - We want it to look nice and at least somewhat native. We know that's a hard ask when you're building something that works across so many environments.

### GTK 3+
#### What we like
- It's been around for a while.
  - Newer isn't always better. In some cases the old tried and true way is the best way. Since it's been around a while, we can feel safe that someone has run into any problem we'll come across in this project.
- It's stable
  - This isn't to say that Kivy isn't stable, just that GTK is well-known and used by lots of major projects.
- It's cross-platform
  - This is a big deal since some of us are using Mac OS X and others Windows. We want to make sure we can easily work together on this project.

## Conclusion
In the end we decided to go with GTK3+ because it was easy enough to install and it's a recognizable interface. In otherwords, we know what we are going to get. Since this is our first project with a GUI and python it seems wise to go with what will most likely have more resources online for us to learn and solve problems as they come up.

## Today we...
- Installed GTK3+ on mine and Kaleb's computers using this [tutorial](https://python-gtk-3-tutorial.readthedocs.io/en/latest/install.html)
- Created a very simple `app.py` to make sure everything was installed correctly on our machines
- Discussed how to architect the `Decoder` class
- Got comfortable with the GTK docs and the widgets that are available. This next week should be a lot of fun!

## On the Next Episode >
Now that we have GTK intalled on mine and Kaleb's computer we're going to be able to spend this week working on building an interface. We'll keep it very simple so that we don't have a lot to worry about when we start to plugin in the work from Amy and April.