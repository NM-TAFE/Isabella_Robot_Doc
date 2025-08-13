# how it works

## commands

### what is a command? really?
to start lets clarify and define the specifics of what a Linux terminal is and what commands exactly are.

When typing a specifically the part before the first space

```bash
sam@Ramiel:~/Documents/repos$ grep
                              -----
                               ^
                              command
``` 

99% of these commands are a literal executable that can be found in a directory somewhere on the computer

unlike on windows executables do not end in a .exe and in-fact do not end in anything the convention on Linux is that a file without a suffixes is usually an executable, but keep in mind that suffixes such as .png, .txt, .exe, .ini, .toml, ect; are all just part of the name of that file and do not on a fundamental level change anything about the content of that file or its a ability to be executed e.g if I changed the name of the executable to grep.exe or grep.jpg it can still be run 

```bash
sam@Ramiel:~/Documents/repos$ grep.jpg

``` 
this will still run like normal!!

but also many programs may use the suffix as a clue on how to treat a file you give as input. so be aware 

### parameters
anything after the command is a parameter
```bash
sam@Ramiel:~/Documents/repos$ grep -r “hello world”
                                   ----------------
                                    ^ all parameters
```
parameters are split be spaces for the most part before being passed to the executable program for it to do what ever it wants.
this means that their are no strict rules about how a command will react to parameters with can lead to some annoyances.
hear is a list some usefully conventions to how parameters are usually treated by a program (and some example exceptions)

1.
most commands use a single dash and a single letter for orderless(can go any where) arguments like for the "grep" example -r or -h -l
these can usually be but all in to one big parameter like so -rhl this will do all the same things as puting them in individually.
exceptions some commands will need all the order-less arguments to be all one after each other at the beginning while others you can put them where ever


2.
double dashes like --help , --recursive or --line-number; usually are for orderless arguments with a full clear name and usually hava a single dash equivalent





