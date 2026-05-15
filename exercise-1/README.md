# Exercise 1

**Update this README with your answers to the questions below.**

## How to Approach This Exercise

Before you scroll down to the questions, try this:

1. **Don't peek at the questions yet.** Pretend you've just been handed a brief
   that says: *"Learn these topics as deeply as you can — g++ CLI, Make, Git,
   sockets, and memory management in C++."* That's the whole assignment.
   How would you go about it? What would you read, what would you try, in what
   order, and how would you know when you've understood something well enough?
   Write that plan down in this README, then actually follow it.
```
Plan:
1. search up and read the g++ man page, look at synopsis and go through related arguments in detail, test out some interesting arguments
2. for make, there are several resources which i've already looked at like https://makefiletutorial.com/ and https://swcarpentry.github.io/make-novice/, something else to look at is https://www.gnu.org/software/make/manual/make.html
3. for git, resources that i've looked at before include https://swcarpentry.github.io/git-novice/ and what else :/
4. for sockets, an initial way to learn it would be to read a few chapters of beej(i've read the c programming part of it while i was studying for cysec and i've heard good things about it), along with man pages for reference since they contain indepth info about the topics
5. memory management in c++ is interesting to read about. i've heard of new, delete, rule of three and rule of five from programming club sessions so that would be a starting point and from then onwards whenever something new comes up like some paradigms(copy-and-swap which i learnt about recently), read more about it.

the best way to learn all of these would be by using the concepts on a practical thing, like making a cpp data structure(vector or avl tree or hash map), an http server or just messing around with git branches in a repo until something breaks catastrophically and something goes wrong. for git, i think some cysec challenges were also interesting since they involved delving into git's blob tree and figuring out the flag from there.
```

2. **Now go through the questions below and answer them like you're an LLM.**
   No live Googling, no Stack Overflow, no asking ChatGPT mid-question. You may
   refer to notes *you* took during step 1 — that's your context window. Answer
   from what you've internalised.

3. **Reflect on how it went.** Honestly:
   - Did your self-directed learning actually prepare you for the questions, or
     did you over-study things that never came up?
   - Which questions blindsided you? Why — was the topic missing from your
     plan, or did you skim past it?
   - Map your experience onto the *known knowns / known unknowns / unknown
     unknowns* idea. The interesting category is usually the last one: things
     you didn't even realise you should have learned. What were yours, and how
     could a better learning plan have surfaced them earlier?

The point of this exercise isn't to get the questions "right" — it's to notice
the gap between how you *think* you learn and how you actually do, so you can
close it.

## Learning How to Learn

- Answer the following questions in this file and commit and push your changes.  
- Bonus sections are more difficult and optional.
- How can you find the information required to complete these tasks?
- How can you tell if the source of your information is good?
- How would you define "good" in this situation?

## Learn Basics of g++ CLI

- Compile the TCP client and server using `g++` from command line.
- What are the most important command line arguments to learn for `g++`?
``` 
important command line arguments:
-c -> compilation without linking, converts source files to object files
-S -> stops before calling the assembler, useful for analysing the assembly generated from source code
-E -> stops after calling the preprocessor, useful for debugging any issues arising from macro usage
-o -> change output file name
-v -> verbose
-std= -> pick a particular c/cpp language standard to compile with
-Wall -> generates warnings for many kinds of bad patterns/questionable code
-Wextra -> additional warnings compared to -Wall
-Werror -> treat warnings as errors
-O0, -O1, -O2, -O3 -> different levels of optimisation(in increasing order), also in decreasing order of ease of debugging an executable that was compiled with these flags
-Og -> optimisation level which prioritises ease of debugging(-O1 with those optimisations disabled which interfere with debugging)
-Os -> optimisation for size(-O2 with size-increasing optimisations disabled)
-I<dir> -> header search directory
-L<dir> -> library search directory
-l<name> -> linking against a named library
-fsanitize=<runtime-check> -> switch on a particular runtime-check to detect undesirable behaviour
-g -> keep debugging information in executable/binary for usage with gdb(debug build)
-p -> generate profiling info helpful for prof(flat)
-pg -> generate profiling info helpful for gprof(call graph based)
-s -> strip symbols(release build)
```
- What is the difference between debug vs release versions?
```
a debug build/version will have flags enabled which are helpful for debugging and do not interfere in the process

a release build/version has such flags enabled which reduce the size of the binary and optimise the code, along with stripped symbols since they're not necessary for a production binary
```
- What are the tradeoffs between debug and release versions?
```
the debug version will usually have smaller binary size, shorter compile-time(no/less optimisations) and less efficient execution(optimisations have to do something), along with having the symbol table to facilitate debugging via gdb

the release version will usually have larger binary size, longer compile-time(more optimisations) and more efficient execution(same logic as before), along with stripped symbols because they are not necessary for execution
```
- What arguments would you use in a debug build?
```
-g -> keep debugging info in the binary
-O0/Og -> no optimisations/optimisations that shouldn't interfere with debugging
-Werror -> to analyze warnings without silently ignoring them
```
- What about for release?
```
-O2/O3 -> O2 is usually preferred over O3 because O3 can break code written with bad patterns due to its aggressive optimisations
-s -> stripped symbols since the symbol table is not necessary for execution of the binary
```
- What other kinds of build types are useful?
```
profiling build -> using -p and -pg, to profile either the release build or the debug build

release build with symbols -> analyse the optimisations done and ensure the correctness of the binary
```

## Learn Basics of Make

- Create a Makefile that will speed up the process.
- [Quickstart tutorial to make](https://makefiletutorial.com/) - Learn make 
  fundamentals with practical examples and common patterns.
- How else can you learn about make?
```
one of the best ways would be to look at the Makefiles of open-source projects because based on the solutions implemented there, one can understand the issues they faced along with how they ended up fixing them(one can also look at their Makefile commit history for this)
```
- How can you tell if the resource you are using is correct?
```
the resource should be verifiably correct(hopefully from an official source like the creators or a big software giant) or should reference verifiably correct materials
overall, specs and manuals are way more likely to be correct as compared to blogs and tutorials and should be used as reference to verify one's knowledge
```
- Create a makefile such that when you run `make` with no arguments, it will:
  - Create `build/` directory if it does not exist
  - Create executables **client** and **server** in `build/`, if needed
  - How does make know when it needs to rebuild the executables?
  ```
  make creates a dependency graph among the files based on the content of Makefile and then based on whether a file's dependencies were changed/modified since the file's last change, it recompiles that file from its dependencies and propagates this change forward towards the root of the tree
  ```
  - Change your Makefile such that `make clean` will remove `build/` and all its contents
- What are the most important command line arguments to learn for make?
```
-C -> change directory to search for Makefile and run make
-n -> only print what's to be executed, don't actually execute
-f -> use different makefile instead of default Makefile
-s -> remove command echoes
-B/--always-make -> run make on everything from scratch
--trace -> gives a trace of everything that was built
--debug -> specifically for debugging purposes
-k -> continue running make even in the face of errors
```
- What are the most important directives to learn about in Makefile?
```
override -> to override variables that come from command line arguments
include -> suspend reading current makefile and read other makefiles
vpath -> add directories to check for prerequisites
conditional directives:
ifeq/ifneq -> check for equality/inequality
ifdef/ifndef -> check for defined variables
else -> standard else
endif -> ends the conditional directive
```
- What are the most important commands to implement in your Makefile?
```
all - make everything that we want to make
build - build the final executable
run - run the final executable 
test - build and run tests(though using testing frameworks is probably better than using make for testing)
clean - clear the built files
check - check for the build's existence?
format - in case any code formatting is being enforced(like clang-format)
```
- Which ones are essential, which ones are nice to have?
```
essential ones:
build, all, test(in case of testing)

nice ones to have:
run, clean, check, format
```

## Learn Basics of Git

- Read through the code in `src/`
- Answer any `#Questions` as a comment
- Commit and push your changes to git
- Each commit should be responding to a single task or question
- Why is it important to keep your commit to a single task or question?
- Is it better to have a lot of very small commits, or one big commit when 
  everything is working?
- What are the most important commands to know in git?

## Introduction to Sockets

- Read the code in `src/tcp-echo-client.cc` and add a way to change the 
  message sent using command line arguments
- **Example**: `./client "hello message from the command prompt"` should send
  `"hello message from the command prompt"` to the server
- Commit your changes into git
- What do all these headers do?
- How do you find out which part of the below code comes from which header?
- How do you change the code so that you are sending messages to servers
  other than localhost?
- How do you change the code to send to a IPv6 address instead of IPv4?
- **Bonus**: How do you change the client code to connect by hostname instead
  of IP address?
  
## Introduction to Memory Management

- What is happening in line 26 of `tcp-echo-client.cc`? 
  `if (inet_pton(AF_INET, kServerAddress.c_str(), &address.sin_addr) <= 0) {`
- What is happening in line 31 of `tcp-echo-client.cc`?
  `if (connect(my_sock, (sockaddr *)&address, sizeof(address)) < 0) {`
- What is the difference between a pointer and a reference?
- When is it better to use a pointer?
- When is it better to use a reference?
- What is the difference between `std::string` and a C-style string?
- What type is a C-style string?
- What happens when you iterate a pointer?
- What are the most important safety tips to know when using pointers?

## Learn Basics of Creating a C++ Project in Your IDE

- How do you compile and run your project in your IDE?

## Improving Interactions with LLMs

- What is the most authoritative source of information about `socket()`
  from `<sys/socket.h>`?
- What is the most authoritative source of information about the TCP and IP
  protocols?
- What is the most authoritative source of information about the C++
  programming language?
- What information can you find about using Markdown when structuring prompts 
  to LLMs?
- What is the difference between LLM and AI?
- Is it grammatically correct in English to say "a LLM" or "an LLM"? Why?