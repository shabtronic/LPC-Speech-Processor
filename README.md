# LPC-Speech-Processor
LPC algorithm in JSFX

I recently bought a PO-35 speak - and it's soooo much fun, I decided to investigate how it processes it's speech.

I found a video with the developer:

[PO-35 Developer video](https://www.youtube.com/watch?v=D730KX75kHk&list=WL&index=1)

and it turns out - they used LPC to record and compress the audio - and then fiddle with the resynthesis (fundemental OSC and formats) to get all the great whacky effect.

so I decided to write my own in reaper - just for fun!

LPC is one of those damn "Skeleton" algorithms - i.e. not much info on the core parts of the alogrithm. So I spent a few days researching and came up with the following:

Encoding:

1) Decide how many filter co-effcients you want to use - the higher the better (11 is pretty good)
2) Split the audio into overlapping frames, around 13 ms in size is pretty good
3) Apply a raised cosine window to the frame audio (hann/hanning)
4) Decide if the frame is voiced speeh or noise

if it's voiced speech:

6) Calc the autocorrecation values for the frame audio
7) Calc the mean of the frame audio
8) Calc the deviation of the frame audio
9) Calc the filter coeffcients of the frame audio
10) Apply a inverse filter to the frame audio to remove any formats
11) Calc the pitch of the frame audio
  
if it's noise:

Decoding/Resynthesis:

if it's voiced speech:

  1) Generate the pitch using whatever OSC you like (saw,pule,square e.t.c)
  2) Apply the filter coeffcients to the new frame audio

if it's noise:


Because you now have the audio stored as a set of pitches and filter coeffcients - you can easily change the pitch and the formants for fun stuff. You can also "AUTOTUNE" the pitches so they match actual note frequencies e.t.c.
