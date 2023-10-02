# LPC-Speech-Processor
LPC algorithm in JSFX

Linear Predictive Coding

I recently bought a PO-35 speak - and it's soooo much fun, I decided to investigate how it processes it's speech.

I found a video with the developer:

[PO-35 Developer video](https://www.youtube.com/watch?v=D730KX75kHk&list=WL&index=1)

and it turns out - they used LPC to record and compress the audio - and then fiddle with the resynthesis (fundemental OSC and formats) to get all the great whacky effects.

so I decided to write my own in Reaper JSFX - just for fun!

LPC is one of those damn "Skeleton" algorithms - i.e. the implementation details of the various part of the algorithms are not explained in detail. So I spent a few days researching and came up with the following:

Sources found:

[Skantar LPC Source code](https://github.com/SKantar/LPC/blob/master/LPC/LPC/main.cpp)

[Arduino talkie playback lib](https://github.com/ArminJo/Talkie/tree/master/src)

[C code for Levinson-Durbin recursion](http://computer-programming-forum.com/47-c-language/5944d236d7d209fa.htm)

[Embedded Levinson-Durbin implementation](https://www.nxp.com/docs/en/application-note/AN2197.pdf)

[Python LPC](https://www.kuniga.me/blog/2021/05/13/lpc-in-python.html)

[LP Synth dissertation](https://core.ac.uk/download/pdf/11040755.pdf)

[LPC all pole modelling](https://ccrma.stanford.edu/~hskim08/lpc/)

Encoding:

1) Decide how many filter co-effcients you want to use - the higher the better (11 is pretty good)
2) Split the audio into overlapping frames, around 13 ms in size is pretty good
3) Apply a raised cosine window to the frame audio (hann/hanning)
4) Decide if the frame is voiced speech or noise

if it's voiced speech:

6) Calc the autocorrelation values for the frame audio
7) Calc the mean of the frame audio
8) Calc the deviation of the frame audio
9) Calc the filter coeffcients of the frame audio
10) Apply a inverse filter to the frame audio to remove any formats
11) Calc the pitch of the frame audio
12) Calc the gain of the frame audio
  
if it's noise:

Decoding/Resynthesis:

if it's voiced speech:

  1) Generate the pitch using whatever OSC you like (saw,pule,square e.t.c) using the pitch and gain values from the encoding
  2) Apply the filter coeffcients to the new frame audio

if it's noise:


Because you now have the audio stored as a set of gains, pitches and filter coeffcients - you can easily change the pitch and the formants for fun stuff. You can also "AUTOTUNE" the pitches so they match actual note frequencies e.t.c. And you can also time-strech without changing the pitch by changing the output window size - which in turn plays back the audio frames slower.
