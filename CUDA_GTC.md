# CUDA GTC Talk Notes

[Click here to go back to the CUDA page](CUDA.md)

## [CUDA: New Features & Beyond](https://www.nvidia.com/en-us/on-demand/session/gtc24-s62400/?playlistId=playList-180a791b-0239-4af2-824b-9e3e7d555a47)
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
- Idea of using these APIs is to harness the speed & efficiency of tensor cores, which can still achieve peak performance but with far less energy
- Basics of kernel fusion: