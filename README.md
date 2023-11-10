# nes-basic-controls

# Basic Controls for the NES

Here's a sample program that teaches you a little about how to program for the NES controller using NES/ASM 6502.

## ASM 6502? What's that?
ASM 6502 is the aseembly language used to program on the NES. If you've never programmed in an assembly language before, it can be a bit difficult at first, but with a bit of time and patience you can learn it, just like any other language. I've added a lot of comments to the code (most of the code is commented), so this should help you as far as understanding what's going on with each line of the program, but you will probably need to reference the commands for ASM 6502. My recommendation at this point would be to use ChibiAkumas' great "cheat sheet", which is downloadable for free here: https://www.chibiakumas.com/book/CheatSheetCollection.pdf (I also recommend his book on learning ASM6502, and suggest you use it to learn the commands while you're away from the computer!)

## How to Compile "basiccontroller.asm"
If you're looking to compile the code, you'll need to use the VASM compiler. You can get it here: http://www.compilers.de/vasm.html . It's a great compiler that can be used for a variety of systems, and it was made by Dr. Volker Barthelmann. Check it out! To compile simply use: 
<p><code>vasm6502_oldstyle.exe DIR/basiccontroller.asm -chklabels -nocase -Fbin -o "DIR2/basiccontroller.nes"</code></p> where DIR and DIR2 are the paths/directories for the source file and target file, respectively. (If you have the win32 version of vasm, you may need to use vasm_oldstyle_win32.exe instead). I have not compiled this in either linux or on a Mac, so there may be differences, but the documentation on VASM should be able to shed some light on any changes that might exist.

<h1>Differences in Compilers</h1>
A lot of NES programmers use a different compiler than VASM, ca65 (which is a companion assembler to the cc65 crosscompiler). As a consequence, their code will look a little different than what you see here; in particular, the layout of where all of the code is placed will be different. For the sake of learning the code from this tutorial, please don't worry about it for now. At first I found it a bit confusing, but I found that once I understood the basic code and its layout using the VASM compiler, reading code for the ca65 was nearly effortless. I'm confident the same will happen for you! For now just focus on learning with VASM and you can always make the switch later if necessary. 


<h1>How to Run "basiccontroller.nes"</h1>
Assuming you've successfully followed the steps to compile above, you should now have an .nes file, "basiccontroller.nes". This file can be run in any NES (Nintendo Entertainment System) emulator. I tend to use Nestopia, but other NES developers really seem to enjoy FCEUX, so use whichever emulator you like to run it! It's a pretty simple program, you can walk back and forth in the little building that I've drawn. It might be a good thought exercise to see why you can't escape the building!


This readme, coupled with the commenting in the code, should be of help when deciphering the meaning of everything, so you can make modifications and create your own musical masterpieces. Be sure to check out my other tutorials on NES/ASM6502 programming, ideally in the following order:
1) NES Hello World: https://github.com/thomaslantern/nes-hello-world/
2) NES Basic Graphics: https://github.com/thomaslantern/nes-basic-graphics/
3) NES Basic Sound: https://github.com/thomaslantern/nes-basic-sound
4) NES Basic Controls (this one!)
5) NES First Game! (Birthday Blast): https://github.com/thomaslantern/nes-birthday-blast

As I've mentioned in my other tutorials, it's always a good idea to check out other resources (like https://www.nesdev.org/wiki/APU_basics), to top up your knowledge.

Happy Coding!
