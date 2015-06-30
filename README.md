# Chrome webrtc audio bug

It would seem that if you don't render all remote peer streams then chrome will merge audio streams into the rendered one.

Steps to reproduce:

1. Open http://latentflip.com/webrtc-audio-bug-repro in **three** tabs **across at least two computers** (you can't validate the bug on only one computer)
2. Allow media access, and from the input at the top join the same room on each (choose something pseudo-random, but typable).
3. You should now have three tabs each showing their local video stream, and each with two buttons underneath (to represent the other two peers).
4. You should currently hear no audio at all, as the only media we are rendering is local streams, which are muted. (**which is correct**)
5. Now, on **one** of the tabs, enable one of the video streams. **important:** If you are just using two computers, do it on one of the tabs on the computer which has two tabs open.
6. So we are now rendering one remote peer, on that tab, and no remote peers anywhere else.
7. Now tap the microphone on both computers (or if you are using three computers, tap the microphones on the two which are not rendering any remote peers). You'll hear audio from both of the microphones, even though we're only rendering one video **this is the bug**. If you right-click pause that video, you'll note that it does indeed mute both remote audio sources.
  * So it would seem that chrome is mixing remote audio streams together if they are not all being rendered
8. To examine the bug further. Render the other remote video in that tab. You'll now see that they are being rendered correctly, i.e. muting one video only mutes (the correct) one remote stream.
9. If you now remove one of the video streams (with dev tools or whatever) you'll note that chrome doesn't seem to revert back to re-mixing the streams.

