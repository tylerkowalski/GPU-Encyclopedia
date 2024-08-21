# CUDA GTC Talk Notes

[Click here to go back to the CUDA page](CUDA.md)

## Table of contents
- [CUDA: New Features & Beyond 2024](#cuda-new-features--beyond-081924)

## [CUDA: New Features & Beyond 2024](https://www.nvidia.com/en-us/on-demand/session/gtc24-s62400/?playlistId=playList-180a791b-0239-4af2-824b-9e3e7d555a47) [08.19.24]
- Key-metric is accelerated computing is performance per. watt (not strictly performance)
  - Smaller transistors => fewer electrons needed, but has been scaling at a lesser rate than that of transistor density
- From a software-perspective, power is spent primarily on 2 things:
  1. Data movement
  2. Computation
- Power needed for floating point arithmetic roughly scales as the square of the mantissa length
  - Tensor cores are significantly more efficient per FLOP on matrix multiplies (hence such investment in them), but the difference isn't as much as you'd think?
- Stephen Jones predicts there is a power wall coming -- one of the solutions involves new algorithms that can harness the efficiency of lower precision operations (example given was an LU factorization that uses lower precision, then incrementally enhances precision until the same as full precision, which is both faster and more energy efficient)
  - Key-idea: in a fixed-energy data centre, using less energy for certain operations means greater throughput
- cuBlas -> cuBLASLt -> cutlass, in increasing order of control
- cuBlASLt is on the host with more control, while cuBLASDx is a device-side API with the same level of control as cuBLAS (which is a Host API). Device-side API allows for kernel fusion (minimizing data movement)?
- Idea of using these APIs are to harness the speed & efficiency of tensor cores, which can still achieve peak performance but with far less energy
- Kernel fusion can lead to specialization explosion:
  - Dynamically building kernels alleviates this (e.g JIT compilation and run-time kernel generation)
  - Nvida is investing more (as of 2024) in reducing compilation time to make this more feasible
- CUDA inverted pyramid -> changes lower on the stack have snow-balling effects higher up to the stack (e.g enhancing JIT compilation pays dividends for Python developers)
- More work is being done to develop Python library support to increase their reach (more Python developers) 
  - E.g nvmath-python, CUTLASS interface for Python
- Warp -> lets you write kernels in Python through JIT-compilation
  - Backward-differentiable? Lends itself to simulation?
- Legate -> a framework that takes single-threaded code and spreads it across multiple machines?
  - cuNumeric is built upon this and does an implcictly-parallel implementation of the NumPy API
  - Has also been integrated with Jax within the XLA compiler
- Nsight Systems -> a performance viewer for multi-node systems; has had significant improvement to improve this tool
- NVSHMEM -> abstracts any transport from GPU<->GPU
  -  GPUDirect -> allows data to flow directly from the GPU to the network card (as opposed to having to jump many loops through the CPU, but still keeps the CPU in the loop)
  - GPUDirect-Async -> CPU prepares send packets, but the GPU actually signals that the packet is ready to be sent
  - GPUDirect-KI -> kernel directs both the data and control flows to the network controller (CPU is out of the picture)
  - The above plugs into NVSHEMM
- Parallel computing *extends* regular computing, not replaces it -- this concept from Jensen is why the CUDA programming model is heterogenous (host & device code in the same program/file)
  - Historically, PCI-E has been an extreme bottle neck in the programming model due to limited bandwidth -> NVLink C2C used in Grace-Hopper tried to alleviate this
  - With Grace-Hopper, the host/device heterogenous model is a bit askew -- really big processor with 2 characteristics
    - 2 memory systems optimized for each other; CPU [low latency, deep cache hierarchy], GPU [high throughput, high bandwidth cache]
  - Should run latency-sensitive code on the CPU, data/math-intensive on the GPU (give tasks to whatever hardware unit best deals with them)
  - Not just a fast link -- memory systems have the same address space and can access eath other's memory, which helps solve data migration issues? Allows me to put the compute and the data where it needs to be? But fundamentally, it is 1 unified machine
  - Intermediate tensors in LLM training are often discarded and recomputed due to memory constraints, but with the Grace-Hopper programming model, can cache these intermediate tensors, saving significant time
  - GraphSAGE model -> primary model to use NNs to solve graphs. GNNs have CNNs at each node, and there is a ton of data that is randomly accessed
    1. Sample their neighbourhood about a node
    2. Aggregate future information from neighbourhood
    3. Predict graph context and label using aggregated information
  - Random access of memory => need a huge pool of memory with great random access -- perfect for Grace-Hopper architecture 
- Data-dependent execution -> "iterate until converged", but requires CPU in the loop to check if the loop should stop
  - Can now form entire cyclic graphs on the GPU w/o CPU using **conditional nodes**
  - ***IF*** nodes have a subgraph inside and run if the condition is true at runtime
  - ***WHILE*** nodes at the same as ***IF*** but re-evaluate the condition upon completion
  - Is control flow dependency, which allows nodes like ***WHILE*** to be expressed (which data flow dependency would not allow)
  - General trend: getting the CPU to not be needed for hand-holding control -- reduces communication, reduces power, keeps GPU busy
  - Released in CUDA 12.4