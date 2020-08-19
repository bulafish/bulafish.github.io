---
title: Linux Memory Management
author: bulafish
date: 2018-07-13 +0800
categories: [Study Note]
# tags: []
---

[Readahead](https://en.wikipedia.org/wiki/Readahead)：
 - Linux kernel system call that loads a file's content into page cache.  This prefetches the files so when it is subsequently accessed, the content is read from main memory rather then hdd, resulting in faster response time
 - [Readahead](http://blog.nutsfactory.net/2008/09/09/readahead-on-eeepc/) 的基本原理是預先載入開機過程可能使用的檔案到記憶體中 (page cache)。如此開機程式執行時，便可節省讀入檔案的時間，進而加快開機速度

[Page cache](https://www.thomas-krenn.com/en/wiki/Linux_Page_Cache_Basics)：
 - 用來加速系統讀取檔案速度
 - 當資料 write to / read from storage, 資料也會儲存一份到空閒的記憶體中 (unused areas of memory), act as cache
 - 當該資料再度需要時就可以從 cache 裏讀出, 加速速度
 - CentOS 6 中, free -m 出現的 cached 欄位就是 page cache 目前使用的大小
 - 當目前的記憶體大小無法滿足程式的記憶體需求時, areas of page cache that are no longer in use will be automatically deleted

[Dirty page](https://www.thomas-krenn.com/en/wiki/Linux_Page_Cache_Basics)：
 - 當寫入資料時, 會先寫入 page cache, 然後當作 dirty pages
 - Dirty pages means 儲存在 page cache 的資料但需要寫入 storage (hdd, NAS 等)
 - 測試
   - dd if=/dev/zero of=testfile.txt bs=1M count=10
   - cat /proc/meminfo \| grep Dirty, Drity 數字會增加約 10240 KB
   - 執行 sync 後數字會立即降低, 代表 page cache 資料已回寫到 hdd
   - 系統也會透過 [flush](https://stackoverflow.com/questions/25859996/what-does-flush-2530-in-iotop-file-on-rhel) 自動將 dirty pages 寫入 hdd

[Flush](https://stackoverflow.com/questions/25859996/what-does-flush-2530-in-iotop-file-on-rhel)
 - Kernel process flushes dirty pages from page cache to storage (hdd, NAS 等)
 - On CentOS 6, [try](https://serverfault.com/questions/500833/what-is-causing-these-flush-processes)
   - ps aux \| grep flush ; [flush-253:0]
   - grep ^ /sys/class/block/\*/dev
   - Compare both results
   - Extra [reading](https://lwn.net/Articles/326552/)

[TLB-translation lookaside buffer](https://en.wikipedia.org/wiki/Translation_lookaside_buffer)
 - Memory cache used to reduce the time taken to access a user memory location ( page table in main memory)
 - Sits between CPU and main memory
 - Is associative memory
 - Used by MMU when virtual-physical address mapping is required
 - Stores the recent translation of virtual memory to physical memory (recent access of page table entries)
 - Access to main memory is slower then access to CPU cache, so below
 - [TLB](http://www.cis.upenn.edu/~lee/03cse380/lectures/ln11-vm-v6.4pp.pdf) is mostly stored on CPU cache such as L1, L2, L3 cache (?)
 - More [reading](http://www.cs.iit.edu/~cs561/cs351/VM/TLB.html),
 [reading](https://www.quora.com/Where-in-the-computer-architecture-is-the-page-table-stored-in),
 [reading](https://whatis.techtarget.com/definition/translation-look-aside-buffer-TLB),
 [reading](http://blog.xuite.net/tzeng015/twblog/113272471-MMU+%28%E8%BD%89%E9%8C%84%E6%96%BC%E5%A4%A7%E9%BB%91%E7%8B%97%29),
 [reading](https://www.cnblogs.com/pengdonglin137/p/3362274.html),
 [reading](https://www.geeksforgeeks.org/whats-difference-between-cpu-cache-and-tlb/)

{% include ads3.html %}

[Paging](http://mropengate.blogspot.com/2015/01/operating-system-ch8-memory-management.html?m=1)：
 - Physical memory break down to frame,  pfn is physical frame number

Physical memory：
 - Page frame is the smallest fixed-length contiguous block of physical memory
 - Page frame is 4KB (4096bytes) in size
 - Use command `getconf PAGESIZE` to get the page frame size
 - More [reading](https://en.wikibooks.org/wiki/The_Linux_Kernel/Memory)

Paging daemon：
 - Background process
 - Responsible to maintain a pool of free clean page frames
 - Checks if at least 20% of frames are free every 250ms (/proc/sys/vm/dirty_ratio)
   - Select pages to evict using the replacement algorithm
   - Schedule disk writes for dirty pages (flush process?)

Swapper：
 - When paging daemon is not keeping up with the demand for free pages on the system
 - Swapper swaps out the entire process to free momery
 - Typically swaps out large, sleeping processes in order to free memory quickly

Virtual memory：
 - A page, memory page, or virtual page is a fixed-length contiguous block of virtual memory, described by a single entry in the page table
 - Size of a page is equal to the size of page frame
 - Page is initially read-only
 - When someone wants to write to page, it traps kernel, then OS copies the page and change it to read-write (copy on write)
 - Process can only access to virtual memory so each process is virtual memory isolated

Page fault：
 - When a desired page is not in the memory
 - Traps to kernel to exec further process

Page table：
 - Table for process to map virtual address to physical address
 - Each process has it's on page table
 - TLB is used to cache and speed up the mapping access time between virtual and physical address
 - Page table is stored in main memory (physical memory)

Invert page table：

Multi-level paging：
 - Linux uses three-level page tables
   - Global directory
   - Page middle directory
   - Page table
   - Page

MMU-momery management unit：
 - The job of MMU is to translate page number to (page)frame number
