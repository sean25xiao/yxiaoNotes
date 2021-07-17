# Limitation of Scalar Pipeline

+ Scalar Pipeline 标量流水线 ---- 只有一条有着 k stage 的指令流水线
+ 标量流水线的缺点
  + 最大吞吐量被限制在 IPC = 1, or CPI = 1
  + 所有类型的指令只能在这一条流水线下被执行, 有的时候因为硬件没有优化, 所以十分低效
  + "Stalling" 会引起不必要的流水线空转 (Pipeline Bubbles)

