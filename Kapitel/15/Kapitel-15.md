## Mechanism: Address Translation (Kapitel 15)

### limited direct execution (LDE)

die Programme hauptsächlich direct auf der hardware laufen lassen nur bei z.B Systemcalls oder 
interrupts geht die controlle an OS.
**Versichert dass, das OS die Kontrolle hat und alles effizent läuft**

### hardware-base address translation (address translation)

die hardware rechnet virtuelle Adressen in Physische um.

```
void func() {
    int x = 3000; // thanks, Perry.
    x = x + 3; // line of code we are interested in
```
in assambly

```
128: movl 0x0(%ebx), %eax               ;load 0+ebx into eax
132: addl $0x03, %eax                   ;add 3 to eax register
135: movl %eax, 0x0(%ebx)               ;store eax back to mem
```

- Problem es ist nicht nur ein Programm im Hauptspeicher

### Dynamic (Hardware-based) Relocation

**static relocation** via Software

**dynamic relocation** via Hardware Support (standart meist bessere Lösung)

- Idee **base and bounds(limit)**
- base and bounds wird verarbeitet in der **MMU (memory management unit)** Hardware

#### Calcuations

**physical addresss = vritual address + base**

#### Summary of Hardware Support

| Hardware Req                                                     | Notes                                                                        |
|------------------------------------------------------------------|------------------------------------------------------------------------------|
| Privileged mode                                                  | Needed to prevent user-mode processes form executing privelged Operations    |
| Base/Bounds registers                                            | Need pair of registers per CPU to support addres translation and bounds check|
| Ability to translate virual addresses and check if within bounds | Circuitry to do translations and check limits                                |
| Privileged instructions to update base/bounds                    | Os must be able to set thes values before letting a user program runs        |
| Privileged instructions to register exception handlers           | Os must be abel to tell hardware what code to run if exception occurs        |
| Ability to raise exceptions                                      | When processes try to acces privileged instructions or out-of-bounds memory  |

| OS Req                | Notes                                                                                                                          |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------|
| Memory management     | Need to allocate memory for new proccesses; Reclaim memory form terminated processes; Generally manage memory via **free list**|
| Base/Bounds managment | must set Base/Bounds properly upon context switch                                                                             |
| Exception handling    | Code to run wenn excetions arise; likely action is to terminate offendig proccess                                              |

| OS @ boot               | Hardware                                                                                                                             | No Program Yet |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------|----------------|
| initialize trap table   |                                                                                                                                      |                |
|                         | remember  addresses of ... system call handler<br/> timer handler <br/> illegal mem-access handler <br/> illegal instruction handler |                |
| start interrupt timer   |                                                                                                                                      |                |
|                         | start timer; interrupt after X ms                                                                                                    |                |
| initialize process table|                                                                                                                                      |                |
| initialize free list    |                                                                                                                                      |                |

 ### Homework



