# Miscellaneous

## Esoteric Languages

Some of the most used esoteric languages are here. For more of them you can visit [esolangs.org](https://esolangs.org/) or view the list of some esolangs [here](https://en.wikipedia.org/wiki/Esoteric_programming_language). And for a place to run these esoteric languages you can try [tio.run](https://tio.run/)

## Brainfuck

Brainfuck is an esoteric programming language created in 1993 by Urban Müller.

Notable for its extreme minimalism, the language consists of only eight simple commands, a data pointer and an instruction pointer. While it is fully Turing complete, it is not intended for practical use, but to challenge and amuse programmers. Brainfuck simply requires one to break commands into microscopic steps. It's common to use brainfuck in CTF challenges.

Hello world in brainfuck will look like:

```brainf
>++++++++[<+++++++++>-]<.>++++[<+++++++>-]<+.+++++++..+++.>>++++++[<+++++++>-]<+
+.------------.>++++++[<+++++++++>-]<+.<.+++.------.--------.>>>++++[<++++++++>-
]<+.
```

- [More Info](https://en.wikipedia.org/wiki/Brainfuck)
- [Interpreter](https://tio.run/#brainfuck)

## Malbolge

Malbolge is a public domain esoteric programming language invented by Ben Olmstead in 1998, named after the eighth circle of hell in Dante's Inferno, the Malebolge. It was specifically designed to be almost impossible to use, via a counter-intuitive 'crazy operation', base-three arithmetic, and self-altering code.

It's common to misunderstand malbolge as `base85` or [rot47](../crypto/#rot47)

Hello world in malbolge will look like:

```malbolge
 (=<`#9]~6ZY32Vx/4Rs+0No-&Jk)"Fh}|Bcy?`=*z]Kw%oG4UUS0/@-ejc(:'8dc
```

- [More Info](https://en.wikipedia.org/wiki/Malbolge)
- [Interpreter](https://malbolge.doleczek.pl/)

## Piet

[Piet](https://www.dangermouse.net/esoteric/piet.html) is a language designed by David Morgan-Mar, whose programs are bitmaps that look like abstract art. The compilation is guided by a "pointer" that moves around the image, from one continuous coloured region to the next. Procedures are carried through when the pointer exits a region.

Hello world in Piet will look like:

![Hello world in Piet](https://esolangs.org/w/images/6/63/Piet_Hello_World.gif "Hello world in Piet")

- [More Info](https://esolangs.org/wiki/Piet)
- [Interpreter](https://www.bertnase.de/npiet/npiet-execute.php)

## Binary to QR Code

If you get binary numbers and you had no luck decoding it, then try this method. I had a similar [challenge](../tfc ctf.md#weird-friend) on a CTF.
I used https://bahamas10.github.io/binary-to-qrcode/ to translate the binary into QR Code and then decoded it using https://zxing.org/w/decode.jspx

## Mersenne Twister Predictor

If you have a challenge with predicting random number in the python's `random` module try using this tool: https://github.com/kmyk/mersenne-twister-predictor. Python uses Mersenne Twister algorithm for generating random numbers. So this tool will be helpful for that.
