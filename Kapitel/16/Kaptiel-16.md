# Segmentation (Kaptiel 16)

die ersten 2 bits geben an welches Segement es is der Rest is der Offset

```shell
Segment = (VirtualAddress & SEG_MASK) >> SEG_SHIFT

Offset = VirtualAddress & OFFSET_MASK
if (Offset >= BOunds[Segment])
  RaiseException(PROTECTION_FAULT)
else
  PhysAddr = Base[Segment] + Offset
  Register = AccessMemory(PhysAddr)
```

häufig muss die hardware auch handlen ob **negativ oder positiv** wachsendes Segment(zusätzliches 
bit)

manchmal ein Protection bit (read, write, execute) für **code sharing**

**`coarse-grained** unterscheidet nur zwischen heap und stack etc

**fine-grained** kann verwendet werden um Segmente zusammen zulegene keine statische Größe wird 
im **segment table** gespeichert 

**external fragmentation** Platz zwischen den allokierten Bereichen der nicht genutzt werden kan
- Lösung **compact** neu Anordnung der Segmente 

### Homework

```shell
./segmentation.py
```


hier max imal in segment 0 ist adresse 19

minimal in segment 0 ist 108
```shell
ARG seed 0
ARG address space size 128
ARG phys mem size 512

Segment register information:

  Segment 0 base  (grows positive) : 0x00000000 (decimal 0)
  Segment 0 limit                  : 20

  Segment 1 base  (grows negative) : 0x00000200 (decimal 512)
  Segment 1 limit                  : 20

Virtual Address Trace
  VA  0: 0x0000006c (decimal:  108) --> VALID in SEG1: 0x000001ec (decimal:  492)
  VA  1: 0x00000061 (decimal:   97) --> SEGMENTATION VIOLATION (SEG1)
  VA  2: 0x00000035 (decimal:   53) --> SEGMENTATION VIOLATION (SEG0)
  VA  3: 0x00000021 (decimal:   33) --> SEGMENTATION VIOLATION (SEG0)
  VA  4: 0x00000041 (decimal:   65) --> SEGMENTATION VIOLATION (SEG1)


```








