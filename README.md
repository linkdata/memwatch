# memwatch

Cross-platform nonintrusive ANSI C memory leak / overwrite detection library.

This information should be enough to get you started, and should be enough for small projects. For more info, see the files USING and the FAQ. If this is not enough, see `memwatch.h`, which is well documented.

### To run the test program

Look at the source code for `test.c` first. It does some really nasty things, and I want you to be aware of that. If memwatch can't capture SIGSEGV (General Protection Fault for Windows), your program will dump core (crash on Windows).

Once you've done that, you can build the test program.

Linux and other *nixes with `gcc`:

`gcc -o test -DMEMWATCH -DMEMWATCH_STDIO test.c memwatch.c`

Windows with Microsoft's `cl`:

`cl -DMEMWATCH -DMEMWATCH_STDIO test.c memwatch.c`

Then simply run the test program.

`./test`


### Quick-start instructions

* Make sure that `memwatch.h` is included in all of the source code files. If you have an include file that all of the source code uses, you might be able to include `memwatch.h` from there.

* Recompile the program with `MEMWATCH` defined. See your compiler's documentation if you don't know how to do this. The usual switch looks like `-DMEMWATCH`. To have memwatch use `stderr` for some output (like, "Abort, Retry, Ignore?"), please also define `MW_STDIO` (or `MEMWATCH_STDIO`, same thing).

* Run the program and examine the output in the log file `memwatch.log`. If you didn't get a log file, you probably didn't do step 1 and 2 correctly, or your program crashed before memwatch flushed the file buffer. To have memwatch _always_ flush the buffer, add a call to `mwDoFlush(1)` at the top of your main function.

### Limitations

Memwatch cannot catch all wild pointer writes. It can catch those it could make itself due to your program trashing memwatch's internal data structures. It can catch, sort of, wild writes into No Mans Land buffers (see the header file for more info). Anything else and you're going to get core dumped, or data corruption if you're lucky.

There are other limits of course, but that one is the most serious one, and the one that you're likely to be suffering from.

### Can use memwatch with compiler X on hardware Y?

Probably the answer is yes. It's been tested with several different platforms and compilers. Try it.
