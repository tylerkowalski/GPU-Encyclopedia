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