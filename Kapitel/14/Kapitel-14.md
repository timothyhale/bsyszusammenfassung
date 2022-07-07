# Memory API (Kapitel 14)

Auf dem **stack** wird der Memory automatisch allokiert/deallokiert auch automatic memory genannt

Auf dem heap wird das ganze manuel gehandelt

```
nice to know 

void *malloc(size_t size)

void free(void *ptr)
```

### Common Errors

- Forgetting To Allocate Memory
- Not Allocating Enough Memory
- Forgetting to Initialize Allocated Memory
- Forgetting to Free Memory
- Freeing Memory Before You Are Done With It
- Freeing Memory Repeatedly
- Calling ``free()`` Incorrectly


### Underlying OS Support

``brk`` ändert heap größe (ein argument)

``sbrk`` vergrößert heap

### Other Calls

``calloc()`` gefüllt mit Nullen

``realloc()`` neue gößere region

### Homework

valgrind um managment zu checken 


