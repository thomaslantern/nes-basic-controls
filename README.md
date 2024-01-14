# nes-basic-controls

# Basic Controls for the NES

Here's a sample program that teaches you a little about how to program for the NES controller using the Assembly 6502 programming language (ASM6502).

## Note: This is a series of tutorials, which I strongly recommend you go through in order, _especially_ if you are not familiar with ASM6502/NES programming! Each tutorial builds on the last one, so you might miss important information by skipping over any of them. (The code from each tutorial is also largely based on its predecessors.) 

## All of the tutorials from this series can be found here:

1) **NES Hello World**: https://github.com/thomaslantern/nes-hello-world/
2) **NES Basic Graphics**: https://github.com/thomaslantern/nes-basic-graphics/
3) **NES Basic Sound**: https://github.com/thomaslantern/nes-basic-sound
4) **NES Basic Controls** (this one!)
5) **NES First Game! (Birthday Blast)**: https://github.com/thomaslantern/nes-birthday-blast

## How to Compile "basiccontroller.asm"
**_(the instructions here are similar to previous tutorials - if you've got a firm grasp on how to compile at this point, you can probably skip this section)_**

If you're looking to compile the code, you'll need to use the VASM compiler. You can get it here: http://www.compilers.de/vasm.html . It's a great compiler that can be used for a variety of systems, and it was made by Dr. Volker Barthelmann. Check it out! 

### To compile, we simply use: 
<pre><code>vasm6502_oldstyle.exe DIR/basiccontroller.asm -chklabels -nocase -Fbin -o "DIR2/basiccontroller.nes"</code></pre> where _DIR_ and _DIR2_ are the paths/directories for the source file and target file, respectively. (If you have the win32 version of vasm, you may need to use vasm_oldstyle_win32.exe instead). I have not compiled this in either Linux or on a Mac, so there may be differences, but the documentation on VASM should be able to shed some light on any changes that might exist.

## Differences in Compilers
As mentioned in previous tutorials, a lot of NES programmers use a different compiler than VASM, **_ca65_** (which is a companion assembler to the cc65 crosscompiler). Their code will look a little different than what you see here. I strongly recommend you stick with VASM as a compiler while learning these tutorials, at least until you better understand the basic code of ASM6502. Once you've got the fundamentals down for ASM6502, you shouldn't have too much trouble reading it when using ca65 (I can read it now with minimal effort).

# How to Run "basiccontroller.nes"
Assuming you've successfully followed the steps to compile above, you should now have an .nes file, "basiccontroller.nes". This file can be run in any NES (Nintendo Entertainment System) emulator. I tend to use Nestopia, but other NES developers really seem to enjoy FCEUX, so use whichever emulator you like! 

It's a pretty simple program, as all you can do is walk back and forth in the little building that you're trapped inside. It might be a good thought exercise to look at the code to see why you can't escape the building! (I may spoil the thought exercise later in this tutorial, so be forewarned.)

# How to Learn from the Basic Controller Tutorial

In a way, this is one of the simplest tutorials of the five, because if you've understood everything from the previous three tutorials (which you should definitely go through if you haven't already!), then this one should be pretty easy. The main changes to our code involve the _nmi handler_. Let's analyze this code, one chunk at a time.

<pre><code>
  pha 
	php

	lda #1		; Begin logging controller input
	sta $4016	; Controller 1
	lda #0		; Finish logging
	sta $4016	; Controller 1
</code></pre>
So what do we have here? Well, it's not too complicated. _pha_ and _php_ are just commands to push the current value of your accumulator and of your processing flags, respectively, onto the stack. We won't go into too much detail about those, but rest assured that what we're doing here is just making sure we have a back up of those values in case we need them later. Because our NMI fires about sixty times a second, we can't always guarantee that we'll be able to keep whatever value we're using in our accumulator (if we were busy doing something else), so this helps us make sure that any other code we're in the middle of processing doesn't get disturbed.

The next four lines of code (from _lda #1_ to _sta $4016_) are the set of commands we need to log all of the buttons pushed on controller one, as indicated by the comments. Changing _sta $4016_ to _sta $4017 would log the buttons pressed by controller 2 instead of controller 1.

Let's take a look at the next section...
<pre><code>
	ldx #8
	
	readctrlloop:
	pha		; Put accumulator on stack
	lda $4016	; Read next bit from controller

	and #%00000011	; If button is active on 1st controller,
	cmp #%00000001	; this will set the carry
	pla		; Retrieve current button list from stack

	ror		; Rotate carry onto bit 7, push other
			; bits one to the right

	dex		
	bne readctrlloop
	
	sta playerbuttons
</code></pre>

This section is heavily commented, both for your sake and mine. Basically the NES controllers need to be read one bit at a time, each bit representing one button. We logged what was happening with controller one in our last block of code, and here we're reading it one bit at a time. The _and_ and _cmp_ command, combined with the loop, basically check one button at a time to see if it is pressed. If it is, the carry is set, and we use _pha_ and _ror_ to put the carry onto our currently button list. In the last block of code, we ended with our accumulator set to 0, so each time we use ror we are throwing up a one in our accumulator if that button is set.

After looping 8 times (through all eight buttons) we now have an accumulator loaded with **1** in each slot that has/had that button pressed. We can use that to move on to our next block of code to check if the appropriate button is being pushed.

<pre><code>
	checkright:
	lda playerbuttons	; Load buttons
	and #%10000000		; Bit 7 is "right"
	beq checkleft		; Skip move if zero/not pressed
	moveright:
		clc
		lda playerpos	; Load current position
		cmp #$A9	; Make sure it's not $A9
		beq noadd	; If it is, don't move!
		adc #1		; If it's not, add 1 to x-position
		sta playerpos	; Store in playerpos
</code></pre>

This section of code is a little easier to understand. Basically we're checking to see if bit 7 of **playerbuttons** (currently loaded in our accumulator) is set to **1**. If it is, the player is pushing the right button, which will mean that beq checkleft will "fail" (i.e. since our _and_ does not give zero, we don't skip to _checkleft_). Now as for _moveright_, we're loading the player's position (from _playerpos_), checking to make sure it's not already $A9 (that or any higher number is too far to the right of the screen), and then we're adding 1 to playerpos.

_checkleft_ is remarkably similar to _checkright_, so I will leave it as an exercise for the reader to determine what the code does (hint: most of it is commented pretty clearly, anyway). This leaves only to look over one more small section of code:
<pre><code>
	noadd:
	
	
storenewpos:
	lda playerpos
	sta $0203 

	lda #$02
	sta $4014

	plp
	pla

	rti
</code></pre>
_noadd_ is there as a label so that, in our previous two sections (_checkleft_ and _checkright_), if we've reached our boundary (i.e. the brick wall), we branch to noadd so that we don't actually change the player's position. Other than that, we move to _storenewpos_, which is where finally load our player's position into $0203, which is the x-coordinate of our first sprite (i.e. our player). We then load #$02 into our accumulator (_lda #$02_) and store it into $4014 (_sta $4014_), which basically takes all of our sprite data and updates it, in case there were any changes to x-coordinates, y-coordinates, etc.
**(_under construction, to be added: how $4014 generally works_)**

(This tutorial is under construction. While I continue tweaking this "how-to" guide, please visit https://www.nesdev.org/wiki/Controller_reading for your NES dev needs. It's probably the single best source of info for NES programming out there (and the page I linked to in particular has to do with controller input.)

This readme, coupled with the commenting in the code, should be of help when deciphering the meaning of everything, so you can make modifications and create your own musical masterpieces. Be sure to check out my other tutorials on NES/ASM6502 programming, ideally in the following order:
1) NES Hello World: https://github.com/thomaslantern/nes-hello-world/
2) NES Basic Graphics: https://github.com/thomaslantern/nes-basic-graphics/
3) NES Basic Sound: https://github.com/thomaslantern/nes-basic-sound
4) NES Basic Controls (this one!)
5) NES First Game! (Birthday Blast): https://github.com/thomaslantern/nes-birthday-blast

As I've mentioned in my other tutorials, it's always a good idea to check out other resources (like https://www.nesdev.org/wiki/APU_basics), to top up your knowledge.

Happy Coding!
