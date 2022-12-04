
This is an collection of little tools who can help us to understand programs.
It works only under linux.

## Hashdump

```bash
hd {NAME_OF_BINARY}
```

Get only 1frst line -> showing .elf?
```bash
hd {name_of_binary} | head -n 1
```


## File

```bash
file {name_of_bianry}
```


## objdumd

show file head:
```bash
objdump -f {name_of_binary}
```

show all headers:
```bash
objdump -x {name_of_binary}
```

display assembler:
```bash
objdump -d {name_of_binary}
```

get symboltable:
```bash
objdump -t {name_of_binary}
```

## ldd (Dynamic linked dependencies)

```bash
ld {name_of_binary}
```

## readelf

One of the important informations is the Entry point address.
This is where our program starts to live -> `__start`
```bash
readelf -h {name_of_binary}
```

look at the Dynamik Linker:
```bash
readelf -x .interp {name_of_binary}
```
