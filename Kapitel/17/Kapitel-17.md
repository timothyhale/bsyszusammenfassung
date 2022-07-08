# Free-Space Managment (Kapitel 17)

```
nice to know 

void *malloc(size_t size)

void free(void *ptr)
```

**Free List** speichert in einer Liste welche Teile des Speichers noch frei sind mit der anfangs 
Adresse und der Länge

**coalescing** zusammen fügen von aneinander hängenden Free spaces

**header** beinhaltet Länge und magic number für integritäts Check

```shell
typedef struct {
  int size;
  int magic;
} header_t
```
```shell
void free(void *ptr) {
  header_t *hptr = (header_t *) ptr - 1
}
```

free errrechnet Größe mit size + headersize

**sbrk** ist der system call zum allokieren von Speicher


#### Strategies for allocating

- Best Fit
  - den am besten passenden Chunck
- Worst Fit
  - den am schlechtesten passenden maximum size
- First Fit
  - den ersten in der Liste der passt 
- Next Fit
  - den der nach dem zuletzt allokierten passenden Chunck
- Slab allocator
  - **bitte folien anschauen** (schwer zu erklären)
- Buddy Allocation 
  - splitting in half until it fits 

**use the one that fits best to your purpose**

### Homework

policy und sortierung der liste machen den großen unterschied und auf header size achten

#### Ohne Header
```shell
./malloc.py -n 10 -H 0 -p BEST -s 0 -c
seed 0
size 100
baseAddr 1000
headerSize 0
alignment -1
policy BEST
listOrder ADDRSORT
coalesce False
numOps 10
range 10
percentAlloc 50
allocList 
compute True

ptr[0] = Alloc(3) returned 1000 (searched 1 elements)
Free List [ Size 1 ]: [ addr:1003 sz:97 ]

Free(ptr[0])
returned 0
Free List [ Size 2 ]: [ addr:1000 sz:3 ][ addr:1003 sz:97 ]

ptr[1] = Alloc(5) returned 1003 (searched 2 elements)
Free List [ Size 2 ]: [ addr:1000 sz:3 ][ addr:1008 sz:92 ]

Free(ptr[1])
returned 0
Free List [ Size 3 ]: [ addr:1000 sz:3 ][ addr:1003 sz:5 ][ addr:1008 sz:92 ]

ptr[2] = Alloc(8) returned 1008 (searched 3 elements)
Free List [ Size 3 ]: [ addr:1000 sz:3 ][ addr:1003 sz:5 ][ addr:1016 sz:84 ]

Free(ptr[2])
returned 0
Free List [ Size 4 ]: [ addr:1000 sz:3 ][ addr:1003 sz:5 ][ addr:1008 sz:8 ][ addr:1016 sz:84 ]

ptr[3] = Alloc(8) returned 1008 (searched 4 elements)
Free List [ Size 3 ]: [ addr:1000 sz:3 ][ addr:1003 sz:5 ][ addr:1016 sz:84 ]

Free(ptr[3])
returned 0
Free List [ Size 4 ]: [ addr:1000 sz:3 ][ addr:1003 sz:5 ][ addr:1008 sz:8 ][ addr:1016 sz:84 ]

ptr[4] = Alloc(2) returned 1000 (searched 4 elements)
Free List [ Size 4 ]: [ addr:1002 sz:1 ][ addr:1003 sz:5 ][ addr:1008 sz:8 ][ addr:1016 sz:84 ]

ptr[5] = Alloc(7) returned 1008 (searched 4 elements)
Free List [ Size 4 ]: [ addr:1002 sz:1 ][ addr:1003 sz:5 ][ addr:1015 sz:1 ][ addr:1016 sz:84 ]

```

#### Mit header
```shell
./malloc.py -n 10 -H 4 -p BEST -s 0 -c
seed 0
size 100
baseAddr 1000
headerSize 4
alignment -1
policy BEST
listOrder ADDRSORT
coalesce False
numOps 10
range 10
percentAlloc 50
allocList 
compute True

ptr[0] = Alloc(3) returned 1004 (searched 1 elements)
Free List [ Size 1 ]: [ addr:1007 sz:93 ]

Free(ptr[0])
returned 0
Free List [ Size 2 ]: [ addr:1000 sz:7 ][ addr:1007 sz:93 ]

ptr[1] = Alloc(5) returned 1011 (searched 2 elements)
Free List [ Size 2 ]: [ addr:1000 sz:7 ][ addr:1016 sz:84 ]

Free(ptr[1])
returned 0
Free List [ Size 3 ]: [ addr:1000 sz:7 ][ addr:1007 sz:9 ][ addr:1016 sz:84 ]

ptr[2] = Alloc(8) returned 1020 (searched 3 elements)
Free List [ Size 3 ]: [ addr:1000 sz:7 ][ addr:1007 sz:9 ][ addr:1028 sz:72 ]

Free(ptr[2])
returned 0
Free List [ Size 4 ]: [ addr:1000 sz:7 ][ addr:1007 sz:9 ][ addr:1016 sz:12 ][ addr:1028 sz:72 ]

ptr[3] = Alloc(8) returned 1020 (searched 4 elements)
Free List [ Size 3 ]: [ addr:1000 sz:7 ][ addr:1007 sz:9 ][ addr:1028 sz:72 ]

Free(ptr[3])
returned 0
Free List [ Size 4 ]: [ addr:1000 sz:7 ][ addr:1007 sz:9 ][ addr:1016 sz:12 ][ addr:1028 sz:72 ]

ptr[4] = Alloc(2) returned 1004 (searched 4 elements)
Free List [ Size 4 ]: [ addr:1006 sz:1 ][ addr:1007 sz:9 ][ addr:1016 sz:12 ][ addr:1028 sz:72 ]

ptr[5] = Alloc(7) returned 1020 (searched 4 elements)
Free List [ Size 4 ]: [ addr:1006 sz:1 ][ addr:1007 sz:9 ][ addr:1027 sz:1 ][ addr:1028 sz:72 ]

```