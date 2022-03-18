#Monitors

**Differences between Hoare monitor and Mesa monitor:**
In Hoare’s monitor, the signaler signals a waiter, and does context switch to that waiter. In Mesa monitor, the signaler wakes a waiter, and puts it into the ready queue. 

Hoare vs Mesa vs Java:

![img.png](img.png)