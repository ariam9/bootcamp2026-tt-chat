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
```
- What can you do to identify if there are bugs in the code?
```
```

## Refactoring: Extract Function

- What is different in this code compared to exercise-1?
```
```
- Is this code better or worse than exercise-1?
```
```
- What are the tradeoffs compared to exercise-1?
```
```
- Are you able to spot any mistakes or inconsistencies in the changes?
```
```
  
## Thinking About Performance

- Does writing code this way have any impact on performance?
- What do we mean when we say performance?
- How do we measure performance in a program?

## Play with Git

- There isn't necessarily a single correct answer for how to abstract the code from exercise-1 into functions
- Try different ways to refactor the code from exercise-1 to make it more readable.
- Make sure to commit each change as small and self-contained commit
- This will make it easier to revert your code if you need to
- What is `git tag`? How is `git tag` different from `git branch`?
- How can you use `git tag` and `git branch` to make programming easier and
  more fun?

## Learn Basics of Debugging in Your IDE

- How do you enable debug mode in your IDE?
- In debug mode, how do you add a watch?
- In debug mode, how do you add a breakpoint?
- In debug mode, how do you step through code?

### Memory Management and Debug Mode in Your IDE

- How do you see the memory layout of a `std::string` from your IDE debug mode?
- How do you see the memory layout of a struct from your IDE debug mode?