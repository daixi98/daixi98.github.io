# VAX/VMS*
## Segmented FIFO:
Without a reference bit in its page table entry, it is hard for VAX to use LRU algorithm directly for page replacement. So, it chooses a FIFO algorithm with two second-chance lists.   
**FIFO**: Each process has a maximum number of pages it can keep in memory, known as its resident set size (RSS). Each of these pages is kept on a FIFO list; when a process exceeds its RSS, the “first-in” page is evicted.  
**Second-chance lists**: a global clean-page free list and dirty-page list. When a process P exceeds its RSS, a page is removed from its per-process FIFO; if clean (not modified), it is placed on the end of the clean-page list; if dirty (modified), it is placed on the end of the dirty-page list. If another process Q needs a free page, it takes the first free page off of the global clean list. However, if the original process P faults on that page before it is reclaimed, P reclaims it from the free (or dirty) list, thus avoiding a costly disk access.  

## Page Clustering:
With such small pages, disk I/O during swapping could be highly inefficient, as disks do better with large transfers. With clustering, VMS groups large batches of pages together from the global dirty list, and writes them to disk in one fell swoop (thus making them clean).

## Demand Zero:
In a naïve implementation, the OS responds to a request to add a page to your heap by finding a page in physical memory, zeroing it (required for security), and then mapping it into the address space. With demand zeroing, the OS instead does very little work when the page is added to the address space; it puts an entry in the page table that marks the page inaccessible. If the process then reads or writes the page, a trap into the OS takes place. When handling the trap, the OS notices (usually through some bits marked in the “reserved for OS” portion of the page table entry) that this is actually a demand-zero page; at this point, the OS then does the needed work of finding a physical page, zeroing it, and mapping it into the process’s address space. If the process never accesses the page, all of this work is avoided, and thus the virtue of demand zeroing.

## Copy on Write (COW):
When the OS needs to copy a page from one address space to another, instead of copying it, it can map it into the target address space and mark it read-only in both address spaces. If both address spaces only read the page, no further action is taken, and thus the OS has realized a fast copy without actually moving any data. If, however, one of the address spaces does indeed try to write to the page, it will trap into the OS. The OS will then notice that the page is a COW page, and thus (lazily) allocate a new page, fill it with the data, and map this new page into the address space of the faulting process. The process then continues and now has its own private copy of the page.

*Most of the notes of VAX/VMS are from OSTEP: https://pages.cs.wisc.edu/~remzi/OSTEP/
