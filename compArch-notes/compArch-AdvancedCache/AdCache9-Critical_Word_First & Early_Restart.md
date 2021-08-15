# Advanced Optimization 9 - Critical Word First & Early Restart

## Critical Word First

+ 重要的先行 -- 优先拿取重要的 word，这样处理器就可以提前开始工作

![image-20210815112715781](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/image-20210815112715781.png)

+ 例如上图，数据 `3` 更重要，所以 `3` 的 miss 最先被处理，CPU 下一次的工作时间提早了
+ 这个方法可以减少 Miss Penalty

## Early Restart

+ 在通常情况下，CPU 需要等到整个 Cache line 都上来之后才 restart
+ Early Restart ---- 当想要的数据一上来之后，CPU 立马开始工作

![image-20210815113216557](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/image-20210815113216557.png)

+ 例子
  + 数据 `3` 是处理器想要的数据，但正常处理器需要等到 `0~7` 全部写入 Cache，才重新开始工作
  + 如果有 Early Restart 功能，`0~7` 拿取的顺序还是一样的，但是当数据 `3` 写入缓存之后，CPU 不必等接下来的数据，直接可以重新工作
+ 这个方法可以减少 Miss Penalty