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
   - Did your self-directed learning actually prepare you for the questions, or did you over-study things that never came up?
   ```
   some things were very directly related to what i read/studied about, a bunch of stuff i studied related to git and make didn't get covered in the questions and there were also some ideas which i had not studied about while preparing for the questions.
   ```
   - Which questions blindsided you? Why — was the topic missing from your plan, or did you skim past it?
   ```
   the concepts about different types of builds was something that i didn't really pay attention to while studying about g++ and make, but i could infer what they might be about from information i already had.

   using vscode to build projects was also something that i was not familiar with since im way more used to working on code in command line editors or basic text editors like kate(on my os). vscode is really nice for markdown files though.
   ```
   - Map your experience onto the *known knowns / known unknowns / unknown unknowns* idea. The interesting category is usually the last one: things you didn't even realise you should have learned. What were yours, and how could a better learning plan have surfaced them earlier?
   ```
   known knowns -> most ideas about g++, git, make
   known unknowns -> other than the basic socket related stuff covered in the first few chapters of beej, im not really familiar with network programming. also, even currently, some cpp memory management-related paradigms are unfamiliar to me.

   unknown unknowns -> build types, profiling(had to look this up), a bunch of git with multiple branches related commands(you might notice the lack of them in my git answer)

   a big reason for not covering these topics was time mismanagement, if i had spent a bit more time planning what to learn and executed the plan in its entirety, it would have led to a better outcome
   ```

The point of this exercise isn't to get the questions "right" — it's to notice the gap between how you *think* you learn and how you actually do, so you can
close it.

## Learning How to Learn

- Answer the following questions in this file and commit and push your changes.  
- Bonus sections are more difficult and optional.
- How can you find the information required to complete these tasks?
```
by looking through guides, man entries and other sources of information available online
```
- How can you tell if the source of your information is good?
```
the source should either be official(man entries, gnu websites, etc) or reference these official sources
```
- How would you define "good" in this situation?
```
a source is good if it provides verifiably correct information in a well-structured and digestible manner
```

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
```
this is so that in case that the changes made in commit(for that particular task or question) need to be reverted, it can be done cleanly without affecting other unconnected changes
```
- Is it better to have a lot of very small commits, or one big commit when everything is working?
```
it is better to have a lot of very small commits(not too small also though since that would get tedious).
in general, each commit should encapsulate a particular meaningful change made to the codebase.
```
- What are the most important commands to know in git?
```
from the beginning:
init
clone

checking history and status:
diff
status
log

adding and committing:
add
commit

branches:
branch
switch, checkout 
merge
rebase

remote repos:
remote(add, -v, etc)
fetch
pull(fetch + merge)
push

undoing:
revert(reverts to a given commit)
restore(remove staged/unstaged changes)
reset(move HEAD pointer and stage/unstage/completely remove changes from skipped over commits)
```

## Introduction to Sockets

- Read the code in `src/tcp-echo-client.cc` and add a way to change the 
  message sent using command line arguments
- **Example**: `./client "hello message from the command prompt"` should send
  `"hello message from the command prompt"` to the server
```
making client changes is fine but the way the server processes sockets and put their messages into the buffer has problems:
firstly after accepting the connection and processing the communication, the socket should be closed
secondly, a new socket should get a new buffer rather than overwriting from the beginning into the old buffer, or the old buffer can be reused by putting a nullbyte at the end of the new message so that only that message gets printed out
```
- Commit your changes into git
- What do all these headers do?
```
<arpa/inet.h> -> unix header file that offers functions to convert things to network byte order and convert between presentation and network formats of ip addresses, etc
<iostream> -> offers stdio-related functionality like std::cout, std::cin, etc
<netinet/in.h> -> provides protocol related structures, constants, etc
<string> -> offers std::string, a mutable string container
<sys/socket.h> -> provides key socket functionality like socket, connect, bind, etc
<sys/types.h> -> defines system related types
<unistd.h> -> provides access to posix os apis(system call wrappers)
```
- How do you find out which part of the below code comes from which header?
```
for particular functions, their man entry(man7.org) lists which header they come from as well as their function declaration

other thing that can be done is to run the preprocessor(with -E) and figure out which include brings what part of the code from the output
```
- How do you change the code so that you are sending messages to servers other than localhost?
```
if the server's ip address is known, simply take it as input and replace kServerAddress with that input
```
- How do you change the code to send to a IPv6 address instead of IPv4?
```
sockaddr_in -> sockaddr_in6
AF_INET -> AF_INET6 (socket and inet_pton can handle it with this)
sin_port -> sin6_port
sin_addr -> sin6_addr
sin_family -> sin6_family
"127.0.0.1" -> "::1" (localhost/loopback on ipv6)
```
- **Bonus**: How do you change the client code to connect by hostname instead of IP address?
```
we can use getaddrinfo(defined in <netdb.h>) to get a linked list of possible addr_info(based on hostname, port and other hints), from which we can extract the sockaddr we want, to connect to a particular server by its hostname rather than its IP address
```
  
## Introduction to Memory Management

- What is happening in line 26 of `tcp-echo-client.cc`? 
  `if (inet_pton(AF_INET, kServerAddress.c_str(), &address.sin_addr) <= 0) {`
```
inet_pton returns:
1 -> success
0 -> given cstring doesn't represent a valid address in the given family/invalid address under given family
-1 -> given family is not supported/invalid family

so the if statement is just error handling for these cases
```
- What is happening in line 31 of `tcp-echo-client.cc`?
  `if (connect(my_sock, (sockaddr *)&address, sizeof(address)) < 0) {`
```
connect returns:
0 -> successful connection
-1 -> error encountered during connection

so the if statement is just error handling for these cases
```
- What is the difference between a pointer and a reference?
```
a pointer is just a variable storing a memory address
a reference is an alias for an already existing variable or object
```
- When is it better to use a pointer?
```
since pointers can be null(and can be checked for being null), they should be used in situations where one might not be sure that a valid object is returned
also pointers can be reassigned to other addresses
```
- When is it better to use a reference?
```
a reference shouldn't be null(undefined behaviour) and nor can it be reassigned/rebound to a different object and so, it should be used for efficiency/more concise code only for objects which are guaranteed to satisy these requirements

references are usually used to prevent passing values by copying which is inefficient for larger objects and impossible for some objects(like unique_ptr) which cannot be copied
```
- What is the difference between `std::string` and a C-style string?
```
an std::string is an stl class which manages its memory automatically. it is mutable(unless const). also it supports many operations like concatenation, efficient length queries, etc.

a c-style string is a sequence of characters, terminated by a null byte('\0'). it can be stored in a character array(mutable in this case), be referred to by a char* but actually stay in the .data section(string literals - immutable), etc. its length is not stored and thus it has to be scanned until '\0' is encountered.
```
- What type is a C-style string?
```
either a char array(char[]) or a char*, depending on how it was created/how it's being accessed.
```
- What happens when you iterate a pointer?
```
when you iterate a pointer, you get a pointer which points to an address = old address + size of pointer type

for example, if a is an int* then a+1 will increment the address by 4 bytes since int is a 4 bytes datatype(usually)
```
- What are the most important safety tips to know when using pointers?
```
1. don't dereference nullptr(check before if it has a chance to be a nullptr)
2. don't access out of bounds memory using pointers(it's undefined behaviour)
3. don't change pointer type arbitrarily to access fields members
```

## Learn Basics of Creating a C++ Project in Your IDE

- How do you compile and run your project in your IDE?
```
on the command line, just use g++ and run directly or for larger projects, use a makefile or set up build tasks on vscode to run commands for you
```

## Improving Interactions with LLMs

- What is the most authoritative source of information about `socket()` from `<sys/socket.h>`?
```
https://man7.org/linux/man-pages/man2/socket.2.html
```
- What is the most authoritative source of information about the TCP and IP protocols?
```
TCP - https://datatracker.ietf.org/doc/html/rfc793
IP - https://datatracker.ietf.org/doc/html/rfc791
```
- What is the most authoritative source of information about the C++ programming language?
```
https://isocpp.org/std/the-standard
```
- What information can you find about using Markdown when structuring prompts to LLMs?
```
since LLMs were trained on a large scale on many .md files, they are great at understanding and making use of instructions based on this format.

instead of dropping a wall of text at an LLM, it is more effective to use a .md file with different sections of things under different headings(marked by # or ## to specify scale).

using bullet points and other formatting tricks in the .md-style inputs also helps the llm get more context and will usually result in better/more meaningful responses.
```
- What is the difference between LLM and AI?
```
AI as a concept has existed long before LLMs gained popularity. the whole concept of teaching machines to learn from data/give outputs based on prior is how the field of AI/ML came into existence.

on the other hand, LLMs are usually transformer-based models that are trained on a large amount of text data to understand natural language. the ones which have been trained on such an enormous amount of data that they can find relationships and reason using natural language are dubbed foundation models and specialisations of these are the basis of commerical ai usage currently(in the language-based fields at least). 
```
- Is it grammatically correct in English to say "a LLM" or "an LLM"? Why?
```
an LLM(LLM is pronounced as el el em, so it should be an and not a)
```