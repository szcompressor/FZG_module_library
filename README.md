### IDENTIFIERS

The identifier follows the format of `[category/cat::subcat]::[proj/conference]::[function]`.

```c++
predictor::psz::Lorenzo_{1,2,3}D
predictor::psz::Lorenzo_{1,2,3}D__local_stat
predictor::psz::Lorenzo_{1,2,3}D__local_stat__global_stat
predictor::psz::Lorenzo_{1,2,3}D_ZigZag
predictor::psz::Lorenzo_{1,2,3}D_ZigZag__local_stat
predictor::psz::Lorenzo_{1,2,3}D_ZigZag__local_stat__global_stat
predictor::psz::Spline3D

codec::huffman::psz::Huffman_codec
codec::huffman::psz::Huffman_ReVISIT_encoder
codec::huffman::IPDPS22::Huffman_fast_decoder
codec::huffman::ICPP20::Huffman_codec_gap_array
codec::bitshuffle::fzgpu::bitshuffle
codec::bitshuffle::cudabitshuffle::bitshuffle
codec::bitshuffle::ndzip::bitshuffle
codec::spatial::potz::PoTZ
codec::spatial::gpulz::LZ
codec::spatial::dietgpu::ANS
codec::spatial::psz::gather_scatter

stat::psz::HistogramGeneric
stat::psz::HistogramCauchy
stat::psz::equal
stat::psz::PSNR
stat::psz::extrema_scan
```

### REPO ORGANIZATION

Multiple codebases are re-organized to expose the modules, illustrated as follows.

```
pSZ/cuSZ (v0.16.1)
- predictor::Lorenzo<dimension={1,2,3}, local_stat={enable, disable}, global_stat={enable, disable}
- predictor::Spline<dimension={3}>
- codec::{Huffman_codec, gather_scatter, fzgpu::bitshuffle}
- stat::{HistogramGeneric, HistogramCauchy, equal, PSNR, extrema_scan}

{potz, gpulz, dietgpu}::spatial_based_codec
{cudabitshuffle, ndzip}::bitshuffle

portable (WIP)
- reference memory management, e.g., GPU-related smart pointer
- basic I/O from/to disk
- dataframe-like memory object
```

### MODULAR VS INTEGRATED DESIGN

GPU-implementation is performance-concerning, integrating more functionality in a single kernel ("kernel fusion") is often desired. However, modular design preserves research opportunities, supports integration, improves portability, and keeps flexibility. In this regard, pSZ/cuSZ implements both approaches. Specifically, the basic implementation of pSZ/cuSZ compression follows a modular design:

```
prediction+quantization kernel -> statistics kernel (histogram)  -> Huffman encoding kernels + gather kernel
```
Although pSZ/cuSZ has been optmized to fuse more components, it still retains the modular reference design that mirrors functionality, particularly by generating the same or a subset of the compression output.
