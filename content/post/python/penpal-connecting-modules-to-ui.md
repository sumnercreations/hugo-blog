---
title: "Penpal Connecting Modules to UI"
date: 2019-10-09T23:45:11-06:00
draft: false
categories:
- development
- python
tags:
- cipher
- python
- penpal
---

## Give it a GUI

If you've been following along, you know that we are using GTK for our GUI. It took us a bit of time to get comfortable with it. I come from a web background and so I wanted to be able to create a `<textarea>` for the user to enter the message that they wanted to encode or decode. All that I could find was the the `Gtk.Entry`. It was an input, and it would work, but I wanted a larger space for text. I didn't like that lots of text would get cut off in the `Gtk.Entry` widget. I found `Gtk.TextView` and I thought, "This is it! I think this will do what I want.". However, it was not as easy to do what I wanted. I thought I would be able to just add the TextView to the flowbox and be done. I did this and it did work, however it was small and didn't really look like the `<textarea>` that I'm familiar with. I spent a lot of time on Google trying to find an example of how to change the size. I must have been really off my game because I wasn't having any luck finding an example. Sadly, I don't recall where I finally found the answer. I feel like it was on the official documentation that I found `set_size_request` on the `Gtk.TextView`. I tried it out and was happy that it worked nicely for me.

{{< highlight python "linenos=table" >}}
self.message_to_decode = Gtk.TextView()
self.message_to_decode.set_size_request(300,50)
{{< /highlight >}}

That line did the trick and I was able to get a large area for text. I wasn't entirely satisfied with the experience. I kept thinking it would make more sense to the user if there was text pre-populated that helped to make it clear what the widget was for. So I did that with this:

{{< highlight python "linenos=table" >}}
self.textbuffer = self.message_to_decode.get_buffer()       self.textbuffer.set_text("awecvgy Dlsjvtl av aol WluWhs Jpwoly Hww!")
{{< /highlight >}}

Now that I'm happy with the way this looks it was time to set up a way to get the text the user enters into the TextView.


## Gotta get that text

We needed the input so we could pass it to the encoder or decoder module. In order to get the text in the `TextArea` we needed to have some action from the user that said "Ok, I'm done. Do the thing." *the thing*, in this case, was either to encode or decode. Since April had the decoder module ready, we started there. We added a decode button to the flowbox with a label that said **Decode Message**.

{{< highlight python "linenos=table, linenostart=35" >}}
decode_button = Gtk.Button.new_with_label("Decode Message")
{{< /highlight >}}

Next we needed to create a handler that would listen for when the button was clicked and run the appropriate function.

{{< highlight python "linenos=table" >}}
decode_button.connect("clicked", self.on_decode_clicked)
{{< /highlight >}}

This made it so that we were able to use the `textBuffer` to get the input, send it to the decoder and then we decided the best way to show the decoded message was in another label below the `TextView`.

{{< highlight python "linenos=table" >}}
def on_decode_clicked(self, button):
    buffer = self.message_to_decode.get_buffer()
    (start, stop) = buffer.get_bounds()
    message_to_decode = buffer.get_text(start, stop, 0)
    decoded_message = decode(message_to_decode)

    # display the decoded message
    self.decoded_label.set_text(decoded_message)
{{< /highlight >}}

We did the same thing for the encode button. Now we have two sections in the app. One for decoding and one for encoding.

## Nice, but it's a bit messy

This works and it has what we need, but it's messy. The widgets don't fit really nicely into our window and there isn't a clear separation of the two features. In order to fix this we used `Gtk.FlowBox`. I've mentioned that we added the widgets to the `FlowBox` throughout this post, but didn't really explain what it is. The `FlowBox` is the layout widget that allows us to add all the other widgets so they all appear in the window. It's nice, and we chose it, because it would allow for the elements to flow nicely one after the other based on the order that they were added to it. I'm sure we could clean this up, and there is probably a better layout widget that we could have used and it may have been better, but this works and we're happy with it.

## On the Next Episode

We have a functioning app, for the most part. We are aware that we are missing a feature though. The user needs to have the ability to choose the how many characters we will shift the alphabet. Right now we have it hard-coded to always shift 7 characters. We know we want to change that and we'll take care of that next. We know that we don't want to allow the user to enter a number that is larger than 26 since there are only 16 letters in the alphabet. Really it doesn't make sense to go over 25 because shifting 26 is the same as not shifting at all. We have talked about using modular math in order to do it so that if the user chooses a value higher than 25 it won't matter because we'll just take the remainder and use that and it will always be between 1 and 25.

We also noticed that our labels where we are putting the endoded or decoded messages aren't selectable. That's not what we want. We expect that the user will want to copy their message and send it over to their penpal so we'll need to fix that too.
[< Previous Episode - Encode & Decode Modules]({{< ref "penpal-decode-encode-classes.md" >}})

[Next Episode - Finishing Touches >]({{< ref "penpal-finishing-touches.md" >}})