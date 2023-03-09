# umax

**MicroMax** (umax or µmax) is a small chess engine written in C by [HG Muller](https://www.chessprogramming.org/Harm_Geert_Muller). 

On the web page [Micro-Max, a 133-line Chess Source](https://home.hccnet.nl/h.g.muller/max-src2.html) Muller describes the project:

> My original aim was to write a chess program smaller than 1024 characters. I could not do it, so far. Even when I dropped the nitty gritty details of the FIDE rules, like castling and en-passant capture, I could not get the size much below 1200 characters.

> So I shifted my aim somewhat, and wrote something less minimalistic in up to 2000 characters of source-code. This gave me enough space to implement a (hash) transposition table, checking of the legality of the input moves, and full FIDE rules. Except for under-promotions, which I considered a dull input problem, since the program is unlikely to ever find itself in a situation where it would be useful to play one. 

This final 4.8 version was [announced on talkchess](https://www.talkchess.com/forum3/viewtopic.php?f=2&t=13837) in 2007.  MicroMax has also been [discussed on Hacker News](https://news.ycombinator.com/item?id=28621143).

## Compile and run
You can compile the code like this:

`gcc -Wall umax4_8.c -o umax4_8`

It will give numerous warnings but should compile.

Run it:

`./umax4_8`

To play a move you'll type four characters: the from-square and to-square.  For instance `e2e4` moves the white Pawn on e2 to the e4 square, and `g1f3` moves the white Knight on g1 to the f3 square.  And then press **enter**.

Then, if you want the computer to move, press **enter** again.  It will briefly calculate, and then move.

Or you can type in another move (so that you play both sides of the board), press **enter**, and so on.

Example game with MicroMax playing black:

1. e2e4 b8c6
2. d2d4 e7e5

In the terminal:

```
$ ./umax4_8 
rnbqkbnr
++++++++
........
........
........
........
********
RNBQKBNR
e2e4
rnbqkbnr
++++++++
........
........
....*...
........
****.***
RNBQKBNR

r.bqkbnr
++++++++
..n.....
........
....*...
........
****.***
RNBQKBNR
d2d4
r.bqkbnr
++++++++
..n.....
........
...**...
........
***..***
RNBQKBNR

r.bqkbnr
+++.++++
..n.....
...+....
...**...
........
***..***
RNBQKBNR
```

## Modification

A couple interesting points you can play with in the source code:

### Turn on kibitz mode
Line 125 looks like this:

`/* if(z)printf("%2d ply, %9d searched, score=%6d by %c%c%c%c\n",d-1,N-S,m,`

To enable that kibitzer code to run remove the `/*` comment mark, like so:

`if(z)printf("%2d ply, %9d searched, score=%6d by %c%c%c%c\n",d-1,N-S,m,`

### Increase search depth

Line 146 invokes the recursive [minimax](https://en.wikipedia.org/wiki/Minimax) search:

`D(-I,I,Q,O,1,3);`

The last parameter is the search depth.  Increase that for stronger play.  For instance:

`D(-I,I,Q,O,1,11);`

**Example:**

Here are just two moves played using the above changes.  White plays `e2e4` and MicroMax responds with `b8c6`.

```
$ ./umax4_8 
rnbqkbnr
++++++++
........
........
........
........
********
RNBQKBNR
e2e4
 0 ply,         1 searched, score=     0 by a8a8
 1 ply,         2 searched, score=     0 by a8a8
rnbqkbnr
++++++++
........
........
....*...
........
****.***
RNBQKBNR

 0 ply,         1 searched, score=     0 by a8a8
 1 ply,         2 searched, score=     0 by a8a8
 2 ply,        13 searched, score=    15 by b8c6
 3 ply,       374 searched, score=     0 by b8c6
 4 ply,       772 searched, score=    15 by b8c6
 5 ply,     15164 searched, score=     2 by b8c6
 6 ply,     32643 searched, score=    15 by b8c6
 7 ply,    308953 searched, score=     5 by d7d5
 8 ply,   1354765 searched, score=    14 by b8c6
 9 ply,   5012912 searched, score=     4 by b8c6
10 ply,  18852526 searched, score=    11 by b8c6
r.bqkbnr
++++++++
..n.....
........
....*...
........
****.***
RNBQKBNR
```