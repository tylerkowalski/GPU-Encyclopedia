# CUDA

## Best Practices

### Grid-Stride Loop
  - Rather than assume a thread grid is large enough to cover the entire data structure, have the kernel loop over the DS one grid size at a time
  - Benefits:
    - Program correctness is not dependent on hardware
    - Thread reuse amortizes thread construction/destruction cost 
    - Adddressing within warps is unit-stride (maximal memory coalescing)
    - Can easily make the kernel serial for debugging

### Memory Coalescing
- Device coalesces (combines) *global* memory stores/loads issued by threads in a warp into as few transactions as possible to minimize DRAM/HBM bandwidth 
- There is **no** penatly for strided access of shared memory (on-chip), but there **is** significant penatly for global memory accesses (more transactions)

## Terminology
- Unit-Stride Access
  - Threads in a warp access adjacent memory locations
- Global memory
  - Logical addres space that is *globally* accessible (by all threads in the GPU & the host)
  - Backed by device DDR, HBM, etc..., although it could be cached depending on the devices compute capability
- Device memory
  - Physical DDR on the GPU
- Unified memory 
  - Managed memory that is accessible to both the CPU and GPU through a *single* pointer
  - Driver & CUDA runtime *automatically* migrates memory between host and device
  - Carefully-tuned programs with *streams* and `cudaMemcpyAsync` are likely to perform better
  - Unified memory is fundamentally restricted since the driver/runtime cannot know as much as the programmer about where/when data is needed
  - Need not be pinned memory, meaning that unified memory is able to migrate data at the level of individual pages between host & device 
- Unified Virtual Addressing
  - UVA is a single virtual address space for all memory
  - Unified memory depends on UVA, but they are not the same!
  - Enabled "zero-copy memory":
    - pinned host memory is accessed by device code over PCI-E without a `memcpy`
  - Does **not** automatically migrate data
- Pinned memory
  - The GPU cannot directly access pageable host memory, so if you initiatite a transfer between pageable host memory -> device memory, the CUDA driver will do the following:
    1. Allocate a temporary page-locked/pinned host array
    2. Copy host data to pinned array
    3. Transfer data from pinned aray to device memory
  - If the host memory is pageable, DMA cannot be used immediately as the data my reside on the disk, which would lead to a page fault (paged memory can be *paged* out to the disk by the OS)
- Warp
  - The threads running in lockstep on a SM
  - Effectively the SIMD width of a SM, which is 32

## Misc.
- For devices of compute capability 2.x and up, the (theoretical) maximum number of blocks in a (launched) grid is 2^31 - 1 in x, and 65,535 for y & z. It is *highly* unlikely you will run into limitations with the grid size launched 
- Modern GPUs (as of 2024) support a (theoretical) maximum of 2048 threads  (64 warps) per SM (excluding other constraints like registers / thread), and each thread block has a maximum of 1024 threads
- The warp scheduler can switch between executing warps in 1 clock cycle
- Thread blocks are "all-or-nothing" with regards to scheduling, allowing them to access memory local to the SM (e.g shared memory)
- It is undefined when resources in a thread block are freed at warp granularity or not, however simple experimenting show this happens *sometimes*