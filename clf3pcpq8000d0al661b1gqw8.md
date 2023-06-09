---
title: "One tool for all your segmentation faults"
datePublished: Sat Mar 11 2023 08:26:03 GMT+0000 (Coordinated Universal Time)
cuid: clf3pcpq8000d0al661b1gqw8
slug: one-tool-for-all-your-segmentation-faults
tags: cpp, c, debugging, gdb, debuggingfeb

---

New programmers often face errors such as segmentation faults during programming in C-type languages. Few of them try to understand otherwise most are just using print statements to narrow down the problem triggering point. And that's completely normal, as one is not aware of the many tools which exist just for the purpose.

These tools come with a name called "Debuggers", which allow developers to

1. step through code line by line,
    
2. set breakpoints,
    
3. inspect values stored in variables,
    
4. and identify errors more easily.
    

Debugging is a crucial part of programming as it helps understanding and then **controlling** the program which in turn allows you to see program with more logical eyes rather than something which works sometimes and sometimes don't.

I often see students unwilling to use a debugger. These students really make their own life hard on themselves, by taking ages to find very simple bugs. The sooner you learn to use a debugger, the sooner it will pay off.

You should be able to follow this guide if

1. you have general idea of programming in C or C++,
    
2. you often find yourself struggling, (I mean debugging) with lots of `printf` or `cout` statements.
    
3. you have moved to \*nix systems and curious to use command line tools.
    

## Compiling you programs with `gcc` (or `g++`) with new flags

To compile a program you generally use `gcc` or `g++`. Syntax of these programs are

```bash
gcc filename.c -o outputname
```

or

```bash
g++ filename.cpp -o outputname
```

but if you intent to debug a program then you should pass one more flag at the time of compilation which tells compiler to be more verbose with the compilation.

### Prepare program for GDB

```bash
g++ filename.cpp -g -Wall -Werror -o outputname
```

We can pass `-g` flag to compilers to make it usable by gdb. This means we want compiler to **compile with debugging symbols enabled** which is used by gdb.

`-Wall` means **enable all warnings**.

`-Werror` means **treat warnings as errors**. This cause compilers to stop if a program has a warning.

## Getting started with GDB

Until now you have compiled your program with debugging symbols which is saved as `<outputname>` depending upon what name you gave it in command line, so you are ready for debugging with GDB.

```bash
gdb <outputname>
```

I had a program named as `gcd` (Greatest common divisor), so I ran it as

```bash
gdb gcd
```

It would welcome you with following output

```bash
GNU gdb (GDB) 13.1
Copyright (C) 2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-pc-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from gcd...
(gdb)
```

### Stopping programs at particular line of code

`gdb` comes with breakpoints which can mark the line at which execution should be paused. Generally, you want to pause before the point where your program is failing or throwing error during compilation.

There are two ways to mention breakpoints:

1. ```bash
      break <line number>
    ```
    
2. ```bash
      break filename.cpp:line_number
    ```
    

For example if I want to set a break-point at line number 15 of the program then I can do it following way

```bash
(gdb) break gcd.cpp:15
Breakpoint 1 at 0x11c7: file gcd.cpp, line 16.
(gdb) break 15
Note: breakpoint 1 also set at pc 0x11c7.
Breakpoint 2 at 0x11c7: file gcd.cpp, line 16.
(gdb)
```

> NOTE: When we are passing line number directly then it will set break point to the file from which output is compiled. As you can see in last prompt it is already mentioning the filename.
> 
> \`Breakpoint 2 at 0x11c7: <mark>file gcd.cpp</mark>, line 16.\`

### Execute your program

To execute it after setting breakpoints, we can simply pass `run` to gdb

```bash
(gdb) run
Starting program: /home/amit/codelib/cpp/gcd

This GDB supports auto-downloading debuginfo from the following URLs:
<https://debuginfod.archlinux.org>
Enable debuginfod for this session? (y or [n]) y
Debuginfod has been enabled.
To make this setting permanent, add 'set debuginfod enabled on' to .gdbinit.
Downloading separate debug info for system-supplied DSO at 0x7ffff7fc8000
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/usr/lib/libthread_db.so.1".

Enter two numbers two get their GCD
```

Depending upon your OS, output of this prompt might be different but overall it's gonna execute your program. At the last you can see `Enter two numbers two get their GCD` which is coming from inside the program. Break points are generally confusing first time, so I would show you what does that mean by showing the execution steps. First of all below is the file `gcd.cpp` (Look on it just as a program not as solution of gcd, as it is not good one)

```cpp
1   │ #include<iostream>
2   │ using namespace std;
3   │ // Description : gcd(a,b) returns greatest common divisor for given integers
4   │
5   │ void gcd(int a,  int b) {
6   │
7   │     //take modulus of both numbers
8   │     if(a < 0) {
9   │         a = -a;
10   │     }
11   │     else if(b < 0) {
12   │         b = -b;
13   │     }
14   │
15   │     //go from lowest no. to zero
16   │     for (int i = min(a,b); i >= 0; i--){
17   │
18   │         //check divisibility of larger no.
19   │         if (a % i == 0 && b % i == 0){
20   │
21   │             //return when divisible
22   │             cout<<i<<endl;
23   │             break;
24   │         }
25   │     }
26   │ }
27   │
28   │ int main(){
29   │     int a,b;
30   │     cout<<"Enter two numbers two get their GCD"<<endl;
31   │     cin>>a>>b;
32   │     gcd(a,b);
33   │     return 0;
34   │ }
```

So, if you see `Enter two numbers two get their GCD` comes on the line number 30 but you might be wondering that why it didn't stopped at 15th line? If you have enough of understanding of execution of programs then you would be aware that c++ files don't usually execute top to bottom line by line instead they start from `main()` function only. So program would start executing from line number 28 directly.

I would give it two numbers(say 8 and 6) to execute further. Now at line 32 it has to calculate `gcd(a, b)` by calling `gcd` function. Now it would go to line number 5 and start executing the function line by line and would suddenly stop before executing line number 16.

```bash
Enter two numbers two get their GCD
8
6

Breakpoint 1, gcd (a=8, b=6) at gcd.cpp:16
16              for (int i = min(a,b); i >= 0; i--){
```

Now, you often like to trace values of some variables during execution of a program. Here, only `i` is gonna vary during each loop execution.

### Watch variables

To watch changing value of `i` during each loop. We can pass `watch <variable_name>` to gdb as shown below

```bash
(gdb) watch i
Hardware watchpoint 2: i
```

Now to move ahead with execution we can use `next` to proceed.

```bash
(gdb) next

Hardware watchpoint 2: i

Old value = 21845
New value = 6
gcd (a=8, b=6) at gcd.cpp:16
16              for (int i = min(a,b); i >= 0; i--){
```

Now it shows value of `i` where *Old value* is some random integer before `i` gets set to `min(a,b)` and *New value* is 6.

Now we will press &lt;return&gt; key or "Enter" to keep moving forward.

On pressing "Enter" two times. We would see

```bash
(gdb)
19                      if (a % i == 0 && b % i == 0){
(gdb)
16              for (int i = min(a,b); i >= 0; i--){
```

Here it checked divisibility(refer to code given at the top) condition and on failing it came back to the loop execution with *probably* decremented value of `i`

Again pressing "Enter" to move into `for` loop

```bash
(gdb)

Hardware watchpoint 2: i

Old value = 6
New value = 5
0x0000555555555227 in gcd (a=8, b=6) at gcd.cpp:16
16              for (int i = min(a,b); i >= 0; i--){
```

First line gets executed and value of `i` gets decremented which can be observed through *Old value* and *New value*.

Now instead of going through it step-by-step we would just go fast forward using `continue`

```bash

(gdb) continue
Continuing.

Hardware watchpoint 2: i

Old value = 5
New value = 4
0x0000555555555227 in gcd (a=8, b=6) at gcd.cpp:16
16              for (int i = min(a,b); i >= 0; i--){
```

Now `i` value gets again decremented. Now press "Enter" to proceed similarly.

```bash
(gdb)
Continuing.

Hardware watchpoint 2: i

Old value = 4
New value = 3
0x0000555555555227 in gcd (a=8, b=6) at gcd.cpp:16
16              for (int i = min(a,b); i >= 0; i--){
```

```bash
(gdb)
Continuing.

Hardware watchpoint 2: i

Old value = 3
New value = 2
0x0000555555555227 in gcd (a=8, b=6) at gcd.cpp:16
16              for (int i = min(a,b); i >= 0; i--){
```

```bash
(gdb)
Continuing.
2

Watchpoint 2 deleted because the program has left the block in
which its expression is valid.
main () at gcd.cpp:33
33              return 0;
```

That's it! We can see for value of i=2, `if` condition gets satisfied and *breaks* out from the `for` loop. Giving 2 as output on screen. GDB also tells us that Watchpoint was deleted because program left that block entirely.

Pressing "Enter" again program will exit normally.

```bash
(gdb)
Continuing.
[Inferior 1 (process 35561) exited normally]
```

To exit out of gdb, use `exit` .

## Bonus

To visualize it simply there is `tui` interface also available in gdb which you can start by passing `tui enable` . After running `gdb <outputfilename>` when you are inside gdb prompt. Then proceed as usual.

This was Programmer's Hiccups you would like to expect for all of your issues. See you.