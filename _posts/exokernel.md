# Exokernel  

**Goals**: Push everything to user level and expose the hardware, in order to maximize the performance. The operating system will be linked to the applications as a library.  
**Motivation of Exokernel**: Some features in the OS are too general, and some features may not be used by the application. L4 chooses to specialize the general features, and Exokernel chooses to delete the unused features (and you can specialize the useful features as well).  
**Additional benefits**: system calls are turned into procedure calls.  
**An example of exposing the hardware**: L4 does not trust the Linux server and uses shadowing page tables, but Exokernel asks user-level applications to manage their own address spaces.   

- **How to keep the system secure?**   
- The Exokernel owns all the physical memory and is in charge of handing out physical pages to every process that is running. Exokernel needs to make sure that for every process, it can only access the physical pages that Exokernel has given to it.   
It turns out to be relatively straightforward: just track the physical pages. And whenever the LibOS wants to make a change, it invokes an Exokernel system call and lets the Exokernel to verify (Is that physical page has been given to that process?).  

- **What happens during a page fault?**   
- The MMU finds a page fault, traps to the Exokernel. The Exokernel does an upcall and asks the running process to give a valid virtual to physical page mapping. Then the process (LibOS) gives the valid mapping to Exokernel. Exokernel installs the mapping in the TLB and resumes the process.  
And in order to reduce the TLB miss overhead, Exokernel caches page tables entries with a software TLB.   

- **What about network operations? The network stacks are all in the LibOS, not in the Exokernel, which means a lot of boundary crossing and overhead.**  
- Use packet filters and application specified handlers. Application specified handlers are application code that gets verified and downloaded into Exokernel, so that when a packet gets received, the Exokernel can handle it without upcalls.  
