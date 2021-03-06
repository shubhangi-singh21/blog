---
title: How sharp is your Memory? Check it with this Memory Game
author: Sanskriti Khare
description: A fun-to-play memory checker made with Python's Speech Recognition API
categories: [speech recognition, python, pyttsx3]
sticky_rank: 1
toc: false
layout: post
comments: true
image: images/blog_1/memory_game.jpg
---
## Introduction

Hey! This is Sanskriti. Hope you all are doing well in this lockdown and using your time well to do productive things. So one of these days, my younger cousin wanted to play something with me, we were on a call. I remembered the memory game I used to play with my mom when I was a kid. My mom used to say the name of an object, I had to add one more object to the list, likewise we had to remember all the old objects in the list in the order and also add the new one. I started playing this game with my cousin but after some time, we both started forgetting the list and neither of us couldn't point out the mistakes of each other, therefore we didn't have a winner. So that day I decided to computerise this game with speech integration. This could solve two problems:

- Computer's memory is definitely better than human memory, so you won't have to care about the right order to tally your responses with.

- If you don't have a partner to play along with you, you could play it with the computer. So making the traditional memory game that we have been playing since childhood more interactive, fun and easy to play!

![](/images/blog_1/memory_game_flowchart.jpeg "Flowchart")


## Step 1: Generate random numbers and append them in a list

To develop this game, the first step was to automate the generation of random numbers that were supposed to be memorised by the player. So I wrote a script in python and used an inbuilt function randint() of the random module(available in the latest versions of python i.e, python 3 onwards). Here, I have given the code snippet to print the whole sequence of numbers, but in the actual game, I have printed only the last number of the list. Rest of the numbers stay in list for verification with the user's input but are not printed.
```python
import random
while(f==0):
    #generate random number
    digit=random.randint(0,9)
    sequence.append(digit)
    print(sequence)
```
### How are random integers generated?

Even the most random data generated with Python is not fully random in the scientific sense. It is pseudorandom, generated with a pseudorandom number generator (PRNG). It starts with a random number, known as the seed, and generates a pseudo-random sequence by performing some operation on that value. Each seed value will correspond to a sequence of generated values for a given random number generator. That is, if you provide the same seed twice, you get the same sequence of numbers twice.

The randint() function uses the [Mersenne Twister](https://github.com/python/cpython/blob/master/Modules/_randommodule.c) PRNG algorithm and takes system current time as the seed so the value of seed keeps changing at every execution of the program. The randint() function can take two integer arguments , a start number and an end number, both inclusive. The function then generates numbers only in this range.

## Step 2: Clear the output from screen

Next step was to clear the output (randomly-generated numbers) from the console after a few seconds for which I used time.stay() function from time library, again an in-built library in python and clear_output() function from IPython.display library. I added these two functions inside the while loop so that the old output gets cleared in every new level.
```python
import time
from IPython.display import clear_output
while(f==0):
    #generate random number
    digit=random.randint(0,9)
    sequence.append(digit)
    print(sequence)
    time.sleep(2)
    clear_output(wait=True)
```
## Step 3: Recognising speech

The most important step was to now integrate a speech recognition module with the game. For that, first you need to download the module, I did it using my pip installer. However, PyAudio Module is a dependency for the SpeechRecognition, and unfortunately you cannot download it directly using pip installer, you will need to use pipwin. After installing the modules, you need to create a function that can be reused to take the input from the user as and when required and compare it to the sequence we have already defined. I used speech recognition by Google API since it gives you the default API key for free. Therefore, an active internet connection is necessary throughout the game to play.

**Special tip:**  _Consider using a try and except block wherever possible so that if anything ever goes wrong, you have a chance to continue the script by returning the same function_

Here, in the try block, I have typecasted the audio recognized as integer type, so that if due to some error in recognition, a string is recogised by the google cloud, the exception block would catch it and return the recogise() function again.
```python
import speech_recognition as sr
r = sr.Recognizer()
def recognise():
    with sr.Microphone() as source:
        print("Speak the number")
        audio = r.listen(source,timeout=3)
    try:
        s=int(sp)
        print("You said ",s)
        return s
    except Exception as e:
        print(e)
        print("Didn't get you, can you say it again?")
        return recognise()
```
### How is your voice recognized??

Firstly, your speech is converted from physical sound to an electrical signal with a microphone, and then to digital data with an analog-to-digital converter. Then, powerful neural network models are used to transform the features and reduce the dimensionality of signals to simplify the speech. Voice activity detectors (VADs) are also used to reduce an audio signal to only the portions that are likely to contain speech. This prevents the recognizer from wasting time analyzing unnecessary parts of the signal.

In the modern speech recognition models like the [Hidden Markov Model](https://en.wikipedia.org/wiki/Hidden_Markov_model), the speech signal is divided into 10-millisecond fragments so that it can be approximated as a stationary process. The power spectrum of each fragment, (a plot of the signal's power as a function of frequency) is mapped to a vector of real numbers known as [cepstral](https://en.wikipedia.org/wiki/Cepstrum) coefficients. The final output of the HMM is a sequence of these vectors.

To decode the speech into text, groups of vectors are matched to one or more [phonemes](https://en.wikipedia.org/wiki/Phoneme)—a fundamental unit of speech. This calculation requires training, since the sound of a phoneme varies from speaker to speaker. A special algorithm is then applied to determine the most likely word (or words) that produce the given sequence of phonemes.

![](/images/blog_1/speech_recognition_flowchart.jpeg "Voice Recognition Steps courtesy Packt Publishing")


## Step 4: Making the computer interact with player

The last step was to make the game more interactive. So, I had used the pyttsx3 module. It is a module that converts string text into speech. You can also set the properties of th e computer voice using the function setproperty(), you can change several attributes of speech, male/female voice, speed, and many other things as per your requirements in a function. Next you directly need to call the function wherever you want in the program, passing the text to be spoken as an argument.

```python
import pyttsx3
def speak(text):
    engine = pyttsx3.init()
    engine.setProperty('rate', 220)  
    voices = engine.getProperty('voices') 
    engine.setProperty('voice', voices[0].id) 
    engine.say(text)
    engine.runAndWait()
```
## Conclusion

Hope you enjoy building and playing this game. You can check my github repo [github repo](https://github.com/sanskritikhare142/Memory-game-using-speech) for the complete code.

Let me know how you renovate the game and take it forward.

Stay home, Stay safe and keep improving your memory! :)

Cheers,

Sanskriti Khare
