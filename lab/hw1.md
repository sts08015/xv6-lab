# [EE817 Spring 2025] Homework 1
 

## Page table
Due: Mar. 27, 2025, 2:30 pm (submission: by email)
Submit a source code and report to TA
Name your file with “hw1_[your studentid]”. ex: hw1_20235221.zip

This project will help you understand the page table. In this project, you will modify the code for better memory usage. Every process has a page directory and a page table. The page table provides mapping information for the memory region used by the process. This memory region contains the kernel region. In xv6, every process has page table that maps the physical memory of the kernel to the virtual memory area (2 GB to 4 GB). However, since the mapping information for the kernel area is all the same, it is not necessary for each process to have one, and it is advantageous from the point of view of memory usage to modify the code so that all processes share one page table. Implementing the modifications described above is the goal of this assignment.

## Part One : fork()
In order for all processes to share the page table for the kernel area, you have to know the code that makes the page table what maps kernel area. In xv6, when creating and running a new process, it allocates a page table for the kernel area and stores the mapping information.

First is the fork(). The fork() creates a child process by copying the parent’s memory area. You must modify the code that generates the page table of the child process that will be created in the function. You must also modify the code that deallocates the page table when the process is terminated or when process creation fails.

* Hint1 : There is a copyuvm() function that copies the page table of the parent in the function. This function is in the vm.c file. This function calls setupkvm() function to generate mapping information for the kernel area. In the setupkvm() function, allocate memory to create a page table that contains mapping information for the kernel area, and store the entries for the generated page table in the page directory. Instead of using setupkvm(), you have to add code that copy the page directory entry of the parent. You should allocate memory only for page directories, not page tables.

* Hint2 : You have to modify the freevm() function, which is a function that deallocates the process’s page table. The function is also in the vm.c file. In the freevm(), all page tables pointed by the page directory are deallocated. However, if the copyuvm() is modified, the page tables for the kernel area should not be deallocated.

## Part Two : exec()
Next is the exec() function. The exec() function is implemented in the exec.c file. This function loads the program to be executed from disk into the memory of the current process and executes it. Because the function changes the code and data of the process, a new page table is created.

Here, too, there is a setupkvm() function that generates mapping information for the kernel area. Instead of using that function, you need to modify the code to copy and save the existing page directory entry.


**Submit : vm.c file with modified copyuvm() and freevm() and exec.c file with modified exec().**