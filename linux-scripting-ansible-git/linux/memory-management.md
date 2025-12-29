# What is Memory management in Linux
- How memory was allocated to process
- Take backend when it is not needed
- Prevents once process consumes whole memory

efficiently use the memory

Memory was divided into pages each size is 4KB this is called virtual memory where each and every process treats it's own memory but in reality it is sharable

virtual address space --map to---> physical memory using page table

2 process can not see each others memory 

but internally those 2 process are maps to different physical ram locations

So process are isolated from each other which gives security and stability

Linux memory is divided into small fixed-size blocks called pages 
- page size 4KB
- virtual page --> mapped to --> physical page

If RAM is full pages can be moved to swap 

# SWAP Memory[Disk used as memory]
- First of all it is very slow compare to RAM
- RAM is full pages are not actively used

Swap helps 
- prevent OOM [out of memory]
- keep system alive under pressure

swapon --show
free -h 

OOM killer[last defence]
When RAM and SWAP is full kernal unable to allocate new RAM to process then to instead of killing the system it just kill the process which consumes more RAM

cat /proc/sys/vm/overcommit_memory

free -h           # memory overview
top / htop        # live memory usage
vmstat            # memory pressure
ps aux --sort=-%mem
cat /proc/meminfo

# What is MemoryManagement
# Pages
# Virtual Memory
# Swap Memory
# OOM killer
# Commands