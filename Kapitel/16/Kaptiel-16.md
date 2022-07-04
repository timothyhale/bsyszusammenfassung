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







