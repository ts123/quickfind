QuickFind
=========

Quickfind (or `qf`) is a UN**X tool, inspired by TextMate's Command-T plugin, for
quickly finding directories under your working directory. It is primarily meant
to be used as a part of a fast approximate `cd` command.

Using QuickFind
---------------

`qf` Uses a very simple pattern matching algorithm to find what you are looking
for. You specify a pattern and then `qf` finds all directories whose pathname
includes all the letters (case insensitive) in the pattern, in the given order.
The letters need not be consequtive in the path, but more weight is given to
paths where the letters in the pattern are next to each other.

Note that `qf` ignores all hidden directories (e.g. directories whose name
starts with a `.`).

For example, if I were to find the location of all ABBA albums in my iTunes
library:

```shell
iTunes$ qf abba
./Music/ABBA
./Music/ABBA/Gold_ Greatest Hits
./Books/Edwin Abbott Abbott
./Music/Black Sabbath
./Music/Black Sabbath/Sabbath Bloody Sabbath
./Music/Nat Newborn/Back To The Moon
./Music/Carbonne - Di Piazza - Manring/Carbonne - Di Piazza - Manring
./Music/Iron Maiden/The Number of the Beast
./Music/Laura Branigan/The Best of Branigan
./Music/Manu Dibango/African Soul - The Very Best of Manu Dibango
./Music/The Cranberries/Everybody Else Is Doing It, So Why Can't We_
```

As you can see, all the results contain the letters "ABBA" in order, but the
paths where the letters are next to each other were given first.

Or, maybe I'd like to go see the tracks on the 'Sabbath Bloody Sabbath' album:

```shell
iTunes$ qf sablosab
./Music/Black Sabbath/Sabbath Bloody Sabbath
Music$ cd "$(qf sablosab)"
Sabbath Bloody Sabbath$
```

Note that in the last example I had to quote the search result since the
pathname contained spaces.

To use `qf` as a part of an approximate `cd` command, I've included a sample
shell function `acd` in the file `acd.sh`. Just copy the function somewhere
where your shell will load it during startup (like `.profile`, `.bashrc` etc.)
and you have a quick way to go anywhere under your current working directory
without having to type full pathnames.

Building and Installing QuickFind
---------------------------------

```shell
make
make install
```

By default `qf` installs into `/usr/bin`, but you can easily change the location
by giving a different prefix during installation:

```shell
PREFIX=~/bin make install
```

Similarly, since the `qf` Makefile only uses implicit rules, you can control the
C compiler and the compiler flags with environment variables. Maybe you'd like
to compile `qf` with `clang` with full optimizations:

```shell
CC=clang CFLAGS=-O4 make
```

You get the idea.

Removing `qf` is as simple as a `make uninstall`. Just remember to give the same
prefix that you used during installation since the Makefile doesn't remember it
(maybe it should).