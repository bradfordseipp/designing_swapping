# Lets Design Swapping!

We're going to explore the idea of **swapping**.
Swapping tries to solve the problem: "can we allocate more memory to processes than we have physical memory in the system?"
It says "yes" by using a portion of the disk/SSD to store additional memory.
Swapping is enabled by the bits in each page-table entry making so that memory that would otherwise be accessible in the VAS, can instead be written out to disk/SSD, thus not held in physical memory.
When that virtual memory is accessed, it will cause a page fault exception.
We need to design how we can use page-tables in coordination with the disk/SSD to provide the illusion to processes that they are just accessing normal virtual addresses, while the OS has the flexibility to have the process's memory be in either physical memory, or on disk/SSD.
Swapping effectively expands the amount of physical memory on the system with disk/SSD (not literally).
A few things to understand:

- We will want to think of having some data that a process has been allocated in physical memory, and some out on disk/SSD.
- We *cannot* do loads and stores to disk/SSD as they use DMA for data transfer.
	Thus, if we want to store some process memory out to disk/SSD, we need to *make the corresponding virtual addresses inaccessible* so that if the processes accesses them, exceptions are triggered.
- We need to define what logic needs to be added to the page-fault exception (PFE) path (trap 14).
	Normally, we have PFE logic to handle Copy-on-Write logic for `fork`, and for converting the exception into a segmentation fault.
	Now we need to expand that path to include swapping logic.
- When we need to bring process data from disk/SSD into physical memory, if physical memory is "full", you need to take other process memory, and write it out to disk, thus making "room".

We're going to practice trying to *design* a system feature.
In your group, make your way through the following questions:

1. What is the purpose of swapping?
	What is the goal?
Swapping is designed to allocate more memory that we have physical memory.	

1. What is the benefit of swapping?
Proper swapping allows for more memory.

1. What are the potential downsides and challenges with swapping?


1. How could you use page-table bits to enable swapping?
1. How would you modify the page-fault exception path (triggered when 1. a page mapping isn't present/valid, or 2. a load/store/execution of the page is attempted when the page doesn't have read/write/execution permissions).
1. How and when do you interact with the disk device?
1. Are there any wait-queues that must be added into the system?
1. What data-structures might you need to implement swapping?
	Don't worry about designing the *type* of data-structure (array, list, tree), and instead just focus on *which* data-structures you'd need (and could implement later).

Page-tables have an additional bit per entry, the "accessed" bit.
It gets *set* by the hardware page-table walker when a page is accessed (read or written).
This bit is used in every swapping implementation.

9. For what purpose might this bit be used?
