---
title: "Penpal Finishing Touches"
date: 2019-10-16T23:45:28-06:00
draft: false
categories:
- development
- python
tags:
- cipher
- python
- penpal
---

## Let the labels be selectable
So as we stated in the previous episode, there are a couple of features that we still need to add. We really wanted the user to be able to copy+paste the decoded or encoded message so that they can send it to their penpal. Turns out that this is a very easy thing to do. `Gtk.Label` has a method `set_selectable()` and we just needed to pass true to that method.

{{< highlight python "linenos=table" >}}
self.decoded_label.set_selectable(True)
{{< /highlight >}}

## Shift the alphabet
We needed a way for the user to be able to select how much they want to shift the alphabet. We didn't want them to be able to choose values that didn't wouldn't work. This meant that they could only choose a value between 1 and 25. As I looked for options I found that there is a widget called `Gtk.SpinButton` and that widget has an option to create with a range. We did this because it fit exactly what we wanted. Only integers can be selected and you can't go below 1 or over 25.

{{< highlight python "linenos=table" >}}
self.shift_input = Gtk.SpinButton.new_with_range(1, 25, 1)
{{< /highlight >}}

That last parameter, the `1` is the step. Meaning that this will change values by 1 each time the button is pressed.

## Can we have a standalone app?
Kaleb asked, "Can any of us run this app on our computer?". Initially my answer was, "Yes, you'll need to download the latest code through git or download the source in a zip folder and then you can run the app." Then I realized that will only work on the machine if GTK is installed... bummer.

That got us thinking that it would sure be nice if we could package up the app with some kind of installer or makefile. After doing a little research we found that [pyinstaller](https://www.pyinstaller.org/) would work nicely for our purposes. I installed it on my Mac and ran `pyinstaller penpal.py`. After it completed I had an exectuable file inside a `dist` folder. I was really excited. I zipped it up and made it a release on Github in the repository; [Version 1.0](https://github.com/sumnercreations/penpal/releases/tag/1.0.0).

Feeling pretty good about that, I told Kaleb to download that and run it on his computer. I felt pretty dumb when he did that on his Windows computer and I realized my mistake...it's only going to work on Mac OS. :disappointed:

I double-checked on the pyinstaller site to confirm my suspicions. It's a nice application and it can create executables for all operating systems, it just can't do it from any operating system. Meaning it's not a `cross-compiler`, you have to compile the code on the same system that you want have an executable for. Not a huge deal, it just means that Kaleb or April will need to compile it on their windows machines and we'll need to also do it on a linux machine.

*Reference* - [FAQ](https://github.com/pyinstaller/pyinstaller/wiki/FAQ)

**UPDATE:** I did do a little more digging and found that there are docker machines that can be used to compile python applications for each of the operating systems so that you don't need to have separate machines for each operating system.

https://github.com/cdrx/docker-pyinstaller

We haven't done this yet, maybe I'll get around it sometime. If I do, I'll make a post here about my experience with it.


## Finished
:tada: We did it! We've created our first application using Python together. We were able to learn a lot of cool things including:
- creating a GUI with GTK
- working with Git and Github
- using the project feature of Github to assign tasks to work on
- using the pyinstaller library to **freeze** a version of our application as an executable.

[< Previous Episode - Connecting modules to UI]({{< ref "penpal-connecting-modules-to-ui.md" >}})