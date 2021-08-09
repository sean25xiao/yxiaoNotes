# Advanced Optimization 7 - SW Compiler Optimization

### 三大 Compiler 优化

+ Restructure code that affects the data block access
  + Group data accesses together for spatial locality
  + Reorder data access for temporal locality
+ 防止一些非必要数据进入 Cache
  + 如果一个数据只用一次，那就应该防止它进入 Cache
  + 软件或者编译器应该提示硬件，不要为这个只用一次的数据分配空间
    + 可以设计一个新的 load 指令，比如 `load.nc` （not cache）
+ Kill data in cache if it is never used again
  + 就像 Prefetch 的反向操作，我们可以提早将数据从 Cache 踢出去
  + 一些数据是因为 Spatial Locality 而存进 Cache 的，如果这些数据在后面不会用到，那么可以提早将他们逐出 Cache，避免造成 miss