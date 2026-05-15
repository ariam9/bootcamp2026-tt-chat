# Exercise 2

**Update this README with your answers to the questions below.**

## Sources of Information for Questions from Before

### Socket 
- https://man7.org/linux/man-pages/man2/socket.2.html - System call reference
  for creating communication endpoints
- Or type `man socket` in terminal
- https://man7.org/linux/man-pages/man7/socket.7.html - Socket interface 
  overview and protocol families
- Or type `man 7 socket` in terminal
- When would you want to use a `SOCK_RAW` stream?
```
if you don't want to rely on the TCP and UDP packet construction that the kernel provides, you can use SOCK_RAW to access network layers in a low-level manner.

this can be used if:
1. you're developing a protocol which doesn't rely on tcp or udp
2. you're making a packet-sniffer(like wireshark), in which case you want to see every packet as a whole
3. in cybersecurity-related tasks to test system behaviour in response to packets with fake or artificial headers, concealing malicious data
```

### TCP and IP Protocols
- [IPv4](https://www.rfc-editor.org/info/rfc791) - Internet Protocol 
  specification defining packet structure and routing
- [IPv6](https://www.rfc-editor.org/info/rfc8200) - Next-generation Internet 
  Protocol with expanded address space
- [TCP](https://datatracker.ietf.org/doc/html/rfc9293) - Transmission Control 
  Protocol providing reliable, ordered data delivery
    
### C++
- [C++23 ISO standard draft](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2023/n4950.pdf) - 
  Working draft of the C++ language specification
- Is the above the official C++23 spec?
```
no, it's a working draft of the spec
```
- Where is the official C++23 spec?
```
https://isocpp.org/std/the-standard
```
- Why was this link chosen instead?
```
probably because the original spec is not free to view, and must be purchased from ISO store(they have copyright over it :|)
```
- Is this a helpful reference for learning C++?
```
for learning more about the internal implementation of features and also more technical details about specific features, it might be helpful to refer to this

however for learning the language itself, it's not a very helpful source due to its lack of learning-focused structure(it's not the purpose of the spec either)
```
- Can the various implementations of C++ compilers be different from the C++ standard?
```
as long as the compilers implement the spec's core features, they're well and good but for new specs like cpp20 and cpp23, most compilers are not completely in adherence with the spec(it takes time to implement new features) and thus their implementations can be different.

added to this is that the cpp spec is a text-written document, and so it might have different interpretations in places leading to varying implementations.

added to this again is the fact that many compilers support compiler specific features(not mentioned in the cpp spec) like __int128 in gcc.
```
- What are the most widely used and most significant C++ compilers?
```
the ones i have heard of are:
gcc(gnu compiler collection)
clang(based on llvm)
msvc(microsoft visual c++)
```
- Where is the equivalent spec for C++26?
```
https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2025/n5014.pdf
```

- Where do you find the spec for the HTTP protocol?
```
http1:
https://datatracker.ietf.org/doc/html/rfc2616
http2:
https://datatracker.ietf.org/doc/html/rfc7540
```
- What about HTTPS? Is there a spec for that protocol?
```
since https is just http over tls(http encapsulated by tls), there's no separate specific spec for the https protocol
```

## Introduction to C++ and Sockets Programming

- Read the code in `src/`
- Are there any bugs in this code?
```
in both client and server, sockaddr_in type objects have been created without nulling(or default initialisation by sockaddr_in addr{}). however sockaddr_in has sin_zero, which should be set to zero during initialisation, otherwise we might get unintended behaviour.

read and send are operating on bytestreams so they might not read the entire data that was sent or send the entire data that we want to send.

in the case of read(or recv), we should have sentinel values which demarcate the end of messages sent(defining a protocol on top of tcp).

in the case of send, we should track the number of bytes sent and keep sending further bytes till the entire message is sent(send outputs a size_t value which represents the number of bytes sent by this send call).

in tcp_echo_server.cc, setsockopt is called with SO_REUSEADDR | SO_REUSEPORT, which is incorrect because setsockopt should be used to set the value of one option at a time and doing bitwise or with these options doesn't make sense(it's not like permissions after all). instead two setsockopt calls are needed, one to set SO_REUSEADDR to 1 and other to set SO_REUSEPORT to 1.

also there's no null-byte that's entered at the end of the message in the receiving buffer, before printing. this could lead to unintended behaviour, depending on the bytes present inside and around the buffer.

in handle_connections, create_address(port) call doesn't make sense since it's simply supposed to be the storage container for client address, why fill it with the server's address which is going to be overwritten anyway.
```
- What can you do to identify if there are bugs in the code?
```
im going to write this specifically for socket-related code
some good practices(probably) are:
1. always deal with the return value of calls like socket, send, recv, connect, accept, bind, etc.
2. make sure to initialise all structs properly
3. make sure you know what the function you're using does and don't make assumptions about its arguments(verify with man page), in order to avoid things like what happened with setsockopt
```

## Refactoring: Extract Function

- What is different in this code compared to exercise-1?
```
since this code is refactored into multiple functions for both client and server, some bugs from exercise-1 have been corrected like leaking socket file descriptors, not closing connections and not reading client-sent data into a buffer correctly(here we have a new buffer for each client/accepted connection, so it works fine).
```
- Is this code better or worse than exercise-1?
```
it's definitely more readable and the architecture is extendable to bigger projects as compared to exercise-1.
```
- What are the tradeoffs compared to exercise-1?
```
there are way more function calls.
for setup of the sockets, it makes negligible difference.
however, since a function is called every time the server accepts a client's connection, there's significant cumulative overhead as function calls are somewhat expensive(they require the setup of a stackframe and finally the popping off of it as well).
furthermore locals like buffer which holds the client's sent data are created new for every single client, which can be inefficient as compared to just clearing out the buffer after handling a client.
```
- Are you able to spot any mistakes or inconsistencies in the changes?
```
one difference is that in exercise 2, exit(EXIT_FAILURE) exits with error code 1 but in exercise 1, there's return 1 in some places and return -1 in some places.

the way the receiving buffer is dealt with in the client accepting loop is definitely better in exercise 2 code as compared to exercise 1 code, in which the same buffer was overwritten by each client's message, wherease in exercise 2, there's a new function call for handling each client's message and thus a new buffer too.
```
  
## Thinking About Performance

- Does writing code this way have any impact on performance?
```
since there are more function calls(and thus more stackframes being pushed and popped), there's a slight hit on performance, especially due to the function call for every accepted client in the exercise-2 code.
added to this is the cost of making new locals inside the repeated client-handling function calls, most particularly, the 1024-byte buffer for receiving messages.
```
- What do we mean when we say performance?
```
usually for programs, performance is measured using two aspects:
1. latency - time delay between events, here it could be the time delay between client sending and recving message, or a client trying to connect and actually getting accepted(after waiting in queue)

2. thoroughput - the amount of work done during the lifetime of the program, measured in terms like number of clients handled per second, number of requests handled per second, number of bytes sent per second
```
- How do we measure performance in a program?
```
performance can be measured using some tools like:
1. profiling tools like prof and gprof
2. checking client latency by measuring time difference before and after a call
3. checking thoroughput by stress testing the server by creating many requests/clients at the same time, and measuring the rate at which they are served
```

## Play with Git

- There isn't necessarily a single correct answer for how to abstract the code from exercise-1 into functions
- Try different ways to refactor the code from exercise-1 to make it more readable.
- Make sure to commit each change as small and self-contained commit
- This will make it easier to revert your code if you need to
- What is `git tag`? How is `git tag` different from `git branch`?
```
git tag marks a specific commit with a name, so that it can be returned to later(by using git checkout). it is usually used to mark important commits, perhaps commits where everything was working with some specific features(v1, v2, etc.).

git branch creates a new branch pointer which can be moved forward by commits, independently of the main branch pointer. such a branch can later be merged in with main to integrate the commits made in the branch with main through a merge commit.

the main difference is that if you checkout to a particular commit(say, through a tag), then commits made in this state can be lost since they're not associated with any branch(unless you create a new branch to keep track of the commit).
```
- How can you use `git tag` and `git branch` to make programming easier and
  more fun?
```
instead of having to work on one feature at a time, you can use git tag and git branch to mark a stable commit(using tag) and then start working on multiple features in parallel at the same time. if something breaks, you can easily return to the stable commit and figure out how things went wrong. these features enable you to focus on the work, rather than the process of getting back to previously working code. 
```

## Learn Basics of Debugging in Your IDE

- How do you enable debug mode in your IDE?
```
on vscode, you just press the debug c/c++ file button, which builds with debugging.

on cli, you can use gdb(which is what vscode uses anyway, it just gives you buttons and a nice pane for all the locals values).
```
- In debug mode, how do you add a watch?
```
on vscode, just add variable to the watch pane
on cli(gdb) watch [name] will pause execution whenever the variable changes
```
- In debug mode, how do you add a breakpoint?
```
on vscode, just click the red dot beside a line
on cli(gdb), break [line number]
```
- In debug mode, how do you step through code?
```
on vscode, just click step over or step into
on cli(gdb), just next(step over) or step(step into)
```

### Memory Management and Debug Mode in Your IDE

- How do you see the memory layout of a `std::string` from your IDE debug mode?
```
for a local variable, it'll show up on the left side pane in vscode or use the debug console to do things in the same way as gdb(given below)
in cli(gdb), you'll have to find the address of the variable and then examine around the address(x/s)
```
- How do you see the memory layout of a struct from your IDE debug mode?
```
in vscode, use debug console to do things in the same way as gdb(given below)
in cli(gdb), similarly to strings, find address of struct and examine around the address
```