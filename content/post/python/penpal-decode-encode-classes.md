---
title: "Penpal Decode & Encode Modules"
date: 2019-10-07T23:41:35-06:00
draft: false
categories:
- development
- python
tags:
- cipher
- python
- penpal
---

# Creating the Decode & Encode Modules

Life got really busy for a bit and so it took some time for us to get things rolling again. Thankfully April was able to get some work done on the decoder module and I, Ammon, was able to get a little work on the UI done.

## Decoder Module

April selected the task of creating the decoder class. Her and Kaleb were able to create a file that would shift an alphabet and then find the correct letter by shifting the index of the encoded letter to output the correct letter. It works well in jupyter, but it's not yet able to receive any user input, nor is it part of the UI.

`decoder.py`
{{< highlight python "linenos=table">}}
alpha_dict={'a':0,'b':1,'c':2,'d':3,'e':4,'f':5,'g':6,'h':7,'i':8,'j':9,'k':10,'l':11,'m':12,'n':13,'o':14,'p':15,'q':16,'r':17,'s':18,'t':19,'u':20,'v':21,'w':22,'x':23,'y':24,'z':25}
alpha_num={0:'a',1:'b',2:'c',3:'d',4:'e',5:'f',6:'g',7:'h',8:'i',9:'j',10:'k',11:'l',12:'m',13:'n',14:'o',15:'p',16:'q',17:'r',18:'s',19:'t',20:'u',21:'v',22:'w',23:'x',24:'y',25:'z'}
alphaset=['a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','w','x','y','z']
def decoder(message,keyword):
    key = len(keyword)
    decoded=''
    for letter in message.lower():
        if letter in alphaset:
            #print(letter)
            x=alpha_dict[letter]
            decode=alpha_num[(x-key)%26]
            decoded+=decode
        else:
            decoded+=letter
    print(decoded)
{{< /highlight >}}

## Adding user input

As we started to implement a way for the user to input information from a terminal window we recognized it was going to work, but it wasn't much different from setting the variable like we had done in Jypter. We decided it would be better to connect the user input to the UI using some kind of text field. Something we'll cover next.

## Encoder Module

April recognized that to create the encoder module is very similar to the decoder module. A big difference that we had decided on was that we didn't want to make the user select what the shift in the alphabet was. Instead we wanted to have the encoder add a random string of characters the length of the shift to the front of the encoded message. Meaning, if the shift the user selected was 7 then we would generate a random string 7 characters in length. Our decoder of course, is then going to take the length of that first string as the shift. Probably not necessary, but it was a fun idea we decided to run with.

`encoder.py`
{{< highlight python "linenos=table">}}
import random
import string

def encode(message, shift):
    alpha_dict={'a':0,'b':1,'c':2,'d':3,'e':4,'f':5,'g':6,'h':7,'i':8,'j':9,'k':10,'l':11,'m':12,'n':13,'o':14,'p':15,'q':16,'r':17,'s':18,'t':19,'u':20,'v':21,'w':22,'x':23,'y':24,'z':25}
    alpha_num={0:'a',1:'b',2:'c',3:'d',4:'e',5:'f',6:'g',7:'h',8:'i',9:'j',10:'k',11:'l',12:'m',13:'n',14:'o',15:'p',16:'q',17:'r',18:'s',19:'t',20:'u',21:'v',22:'w',23:'x',24:'y',25:'z'}
    alphaset=['a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','w','x','y','z']
    encoded = ''
    for letter in message:
        if letter.lower() in alphaset:
            x=alpha_dict[letter.lower()]
            encode = alpha_num[(x + shift) % 26]
            if letter.isupper():
                encoded += encode.upper()
            else:
                encoded+=encode
        else:
            encoded+=letter
    return randomString(shift) + ' ' + encoded

def randomString(stringLength):
    letters = string.ascii_lowercase
    return ''.join(random.choice(letters) for i in range(stringLength))
{{< /highlight >}}

## On the Next Episode
We finished today with the very rough UI that we started to put together knowing we would need to clean it up over the next couple of days in order to have a finished product we could be proud of. I'm very excited about where we are at. It's always fun to build something you can use.

[< Previous Episode - Penpal Cipher Project]({{< ref "penpal-cipher-project-guis.md" >}})

[Next Episode - Connecting modules to UI >]({{< ref "penpal-connecting-modules-to-ui.md" >}})