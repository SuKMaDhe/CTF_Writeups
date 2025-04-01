I will work on solving the remaining challenges as well and write their writeups here :)


### rev_sealrune
This was the easiest warmup challenge, the flag was reavealed after a simple binary patch.

![image](https://hackmd.io/_uploads/HJDgKWj2Jl.png)

### rev_encryptedscroll
We were again given a non stripped elf. 
Being a lazy guy, I patched this using binary ninja.
![image](https://hackmd.io/_uploads/ry0V9-jhke.png)



It didn't reveal the flag. Seems like can't go lazy on this one.

![image](https://hackmd.io/_uploads/rkHMqWj3yx.png)

In strings and in decompiled code too, I saw this sussy baka string

![image](https://hackmd.io/_uploads/H1L3cWj3Jl.png)

IUC could very well stand for HTB. So, I tried to use gdb and I got this message which felt like anti-debug. I didn't have experience with anti-debugging so i looked it up.
-> Anti-debugging: The act of preventing others to use debuggers to learn the intricacies of your software. [here](https://www.youtube.com/watch?v=fcYnmudVH5c) is a video I looked up on antidebugging techniques. TLDR: ptrace is the bad guy, and if we just remove/patch it, life would be good.

![image](https://hackmd.io/_uploads/r1jfiWj3yg.png)

So I patched the anti-debug detector to never run, now I could analyse the binary in gdb.
Putting a breakpoint at a strcmp revealed the flag in rsi and rdi.

![image](https://hackmd.io/_uploads/Sy4s0-ihye.png)

### rev_endlesscycle
This time we had a stripped binary. Programs when compiled usually store whats called as symbol tables, these store the names of identifiers for debugging. But we can instruct gcc to strip the binary of its symbol tables.
In ctf this usually adds a layer of difficulty as we have to rename variables, functions ourselves.
Nothing interesting in strings, so I jumped to decompiled code.
We can see srand() and rand() being called, this challenge might require our understanding of pseudo random number generators.

![image](https://hackmd.io/_uploads/rk6c-zs2ke.png)

But we also have what looks like a shellcode in here.
Phew finally, with help of grok I was able to solve this, challenge. Its probably the most valuable knowledge I have gained from this ctf.
So, in the assembly code

![image](https://hackmd.io/_uploads/r1YVaJk6kg.png)

We can see that rax is being called, whats happening is that the mmap(which if we notice has executable permission too), with srand and rand they are making changes in the bytes(making them into runnable code), after all this is done rax is called as a function.
So now we checked what instruction are at the address of rax, BOOM!!

![image](https://hackmd.io/_uploads/BJra6k16Jg.png)

I was sooo happy when I saw this, cuz theres an xor with (0xbeefcafe) signifying that a human was here :sob: . We could see "what is flag?" string
(in the hex 0x67616c6620656874 and 0x207369... they are in ascii range)
then we see syscalls being made. rax = 1, rdi = 1, rdx = 18. 
`ssize_t read(int fd, void *buf, size_t count)` rdi 1 represents standard output. `mov rsi, rsp` moves the string what is the flag in rsi and calls the sys_call which in this case would be write.
Next we see, rsp making space for 0x100. syscall again.
rax = 0, rdi = 0, edx = 256 (0x100) is sys_read call.
Now whatever our input was it is taking that input and xor with beefcafe four bytes at a time. 
in the repz cmps it is comparing rdi and rsi, rdi has r12 which is our input after manipulation, rsi has hardcoded xor string at 0x7ffff7fbf084. BINGO!!
I extracted those bytes

![image](https://hackmd.io/_uploads/SJ_Pfekayg.png)

And xor'ed them 4 bytes a time with beefcafe.
And I got the flag, `HTB{l00k_b3y0nd_th3_w0rld}`

### rev_impossible_maze
first of all I saw many unfamiliar functions so I had to look em up
`int j = snprintf(buffer, 6, "%s\n", s);` it is used to write s into buffer of size 6. j = length of s (with \0).
Phew solved by a teammate solved it. :)
Apparently I was not viewing the terminal in full screen and hence I could only see gibberish.

