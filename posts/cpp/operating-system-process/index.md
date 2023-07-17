# [OS] Process


## Process

Process 是一個執行中的 program，就是執行中的程式；換言之，靜態的程式稱為 program，動態的程式稱為 process。

Process 是作業系統分配資源的單位，所以有時會被稱作 tasks or jobs。

一個 Process 主要有：

1. Code text (程式碼)
2. Data section (資料)
3. Heap & stack (放動態/暫時性資料)
4. CPU register value
5. program counter (程式計數器 擺放下一個要執行程式 section 的起始地址)
6. Program ID (又稱作 pid)
7. Process state

{{< image src="/images/operating-system/process/process_memory_allocation.png"  height="200"
          src_s="/images/operating-system/process/process_memory_allocation.png"
          src_l="/images/operating-system/process/process_memory_allocation.png"
alt="memory allocation of a process" caption="memory allocation" linked="false">}}

## Reference

-   https://medium.com/@wangwei09310931/notes-of-csapp-3-virtual-memory-523acdda7c77

