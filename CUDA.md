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
- todo: (https://developer.nvidia.com/blog/how-access-global-memory-efficiently-cuda-c-kernels/)

## Terminology
- Unit-Stride Access
  - Threads in a warp access adjacent memory locations
- Global memory
  - Virtual address space that is visible on the host (e.g pinned memory?) *and* device
  - Could be physically backed by memory on the host *or* device
- Device memory
  - Physical DDR on the GPU
- Unified memory 
  - todo: (https://developer.nvidia.com/blog/unified-memory-in-cuda-6/)
- Pinned memory
  - todo: