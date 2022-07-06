# Paging: Introduction (Kapitel 18)

**segmentation** -> variable size

**paging** -> fixed size

ein **page frame** entählt eine **page**  

**page table** speichert welche Seiten benutzt bzw frei sind und existiert 
pro Prozess. Er ist für die Adress übersetzung zuständig

**Übersetztung**

    VPN     Offset 
    biiiiiiiiiiiiiiiiiiits
    
    dann VPN im Pagetable nachschauen und durch PFN erstetzen
    
    PFN     Offset
    biiiiiiiiiiiiiiiiiiiits

**Page tables** werden im virtuellen OS memory gespeichert

- **Valid Bit** ungenutzte Pages werden im Page table als invalid markiert
- **Protection Bit** read write execute
- **Present Bit** ob im Memory oder auf der Disk
- **Dirty Bit** ob es bearbeitet wurde seit dem es im Memory ist
- **Reference Bit/Access Bit** ob kürzlich genutzt



- **Page-Table base register** start des page tables 

