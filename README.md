# nes-basic-controls

# Basic Controls for the NES

Here's a sample program that teaches you a little about how to program for the NES controller using NES/ASM 6502.

## Note: This is a series of tutorials, which I strongly recommend you go through in order, _especially_ if you are not familiar with ASM6502/NES programming! Each tutorial builds on the last one, so you might miss important information by skipping over any of them. (The code from each tutorial is also largely based on its predecessors.)
The tutorials (in order) are here:
1) NES Hello World: https://github.com/thomaslantern/nes-hello-world/
2) NES Basic Graphics: https://github.com/thomaslantern/nes-basic-graphics/
3) NES Basic Sound: https://github.com/thomaslantern/nes-basic-sound
4) NES Basic Controls (this one!)
5) NES First Game! (Birthday Blast): https://github.com/thomaslantern/nes-birthday-blast

## How to Compile "basiccontroller.asm"
**_(the instructions here are similar to previous tutorials - if you've got a firm grip on how to compile at this point, you can probably skip this section)_**

If you're looking to compile the code, you'll need to use the VASM compiler. You can get it here: http://www.compilers.de/vasm.html . It's a great compiler that can be used for a variety of systems, and it was made by Dr. Volker Barthelmann. Check it out! To compile simply use: 
<p><code>vasm6502_oldstyle.exe DIR/basiccontroller.asm -chklabels -nocase -Fbin -o "DIR2/basiccontroller.nes"</code></p> where DIR and DIR2 are the paths/directories for the source file and target file, respectively. (If you have the win32 version of vasm, you may need to use vasm_oldstyle_win32.exe instead). I have not compiled this in either linux or on a Mac, so there may be differences, but the documentation on VASM should be able to shed some light on any changes that might exist.

<h1>Differences in Compilers</h1>
As mentioned in previous tutorials, a lot of NES programmers use a different compiler than VASM, ca65 (which is a companion assembler to the cc65 crosscompiler). Their code will look a little different than what you see here. I strongly recommend you stick with VASM as a compiler while learning these tutorials, at least until you better understand the basic code of ASM6502.

<h1>How to Run "basiccontroller.nes"</h1>
Assuming you've successfully followed the steps to compile above, you should now have an .nes file, "basiccontroller.nes". This file can be run in any NES (Nintendo Entertainment System) emulator. I tend to use Nestopia, but other NES developers really seem to enjoy FCEUX, so use whichever emulator you like to run it! It's a pretty simple program, you can walk back and forth in the little building that I've drawn. It might be a good thought exercise to see why you can't escape the building!

# How to Learn from the Basic Controller Tutorial

In a way, this is one of the simplest tutorials of the five, because if you've understood everything from the previous three tutorials (which you should definitely go through if you haven't already!), then this one should be pretty easy. The main changes to our code involve the _nmi handler_. Let's analyze this code, one chunk at a time.

<pre><code>
  pha 
	php

	lda #1		; Begin logging controller input
	sta $4016	; Controller 1
	lda #0		; Finish logging
	sta $4016	; Controller 1

	ldx #8
</code></pre>
So what do we have here? Well, it's not too complicated. _pha_ and _php_ are just commands to push whatever value is in your accumulator and in your processing flags, respectively. We won't go into too much detail about those, but rest assure that what we're doing here is just making sure we have a back up of those values in case we need them later. Because our NMI fires about sixty times a second, we can't always guarantee that we'll be able to keep whatever value we're using in our accumulator (if we were busy doing something else), so this helps us make sure that any other code we're in the middle of processing doesn't get disturbed.

(This tutorial is under construction. While I continue tweaking this "how-to" guide, please visit https://www.nesdev.org/wiki/Controller_reading for your NES dev needs. It's probably the single best source of info for NES programming out there (and the page I linked to in particular has to do with controller input.)

This readme, coupled with the commenting in the code, should be of help when deciphering the meaning of everything, so you can make modifications and create your own musical masterpieces. Be sure to check out my other tutorials on NES/ASM6502 programming, ideally in the following order:
1) NES Hello World: https://github.com/thomaslantern/nes-hello-world/
2) NES Basic Graphics: https://github.com/thomaslantern/nes-basic-graphics/
3) NES Basic Sound: https://github.com/thomaslantern/nes-basic-sound
4) NES Basic Controls (this one!)
5) NES First Game! (Birthday Blast): https://github.com/thomaslantern/nes-birthday-blast

As I've mentioned in my other tutorials, it's always a good idea to check out other resources (like https://www.nesdev.org/wiki/APU_basics), to top up your knowledge.

Happy Coding!
